

This section introduces both **Machine Learning (Random Forest)** and **Deep Learning (Convolutional Neural Networks - CNN)** for geospatial applications.

---

## Use Case: Classifying Crop Types

Geospatial AI enables us to classify agricultural land based on satellite imagery. In this example, we use machine learning and deep learning models to classify **crop types** from raster patches derived from Sentinel-2 imagery or other multispectral sources.

### Workflow Overview
1. Clip and mask raster imagery using crop boundaries (shapefiles)
2. Extract image patches (e.g., 64×64) labeled by crop type
3. Prepare features or patch arrays
4. Train Random Forest or CNN models
5. Predict crop types for new imagery

---

## Machine Learning with Random Forest

Random Forest is an ensemble learning method effective for structured features like NDVI, texture stats, or spectral means per patch.

### Random Forest Example

```python
import numpy as np
import geopandas as gpd
import rasterio
from rasterio.features import rasterize
from rasterio.windows import Window
from shapely.geometry import mapping
from sklearn.ensemble import RandomForestClassifier
from sklearn.model_selection import train_test_split
from sklearn.metrics import classification_report, accuracy_score
from collections import Counter

# -----------------------
# CONFIG
# -----------------------
RASTER_PATH = r"data\rasters\s2_2025-02_feb.tif"          # multiband GeoTIFF
TRAIN_PATH = r"data\shapefiles\Boundingbox_features.shp" 
TRAIN_LAYER = None                 #
CLASS_COL = "code"                # field holding class label (int or str)
N_SAMPLES_PER_CLASS = 200        # set a per-class cap to control memory 
SAVE_FEATURES = "features.npy"
SAVE_LABELS = "labels.npy"
OUTPUT_CLASSIFIED = "classified.tif" # 


# Read raster & polygons

with rasterio.open(RASTER_PATH) as src:
    profile = src.profile.copy()
    bands = src.count
    height, width = src.height, src.width
    transform = src.transform
    crs = src.crs

gdf = gpd.read_file(TRAIN_PATH, layer=TRAIN_LAYER) if TRAIN_LAYER else gpd.read_file(TRAIN_PATH)
if gdf.crs is None:
    raise ValueError("Training polygons have no CRS; please set it before proceeding.")
if crs is None:
    raise ValueError("Raster has no CRS; please set it before proceeding.")

# Reproject polygons to raster CRS
gdf = gdf.to_crs(crs)

# Map class values to integers (if not already)
classes_raw = gdf[CLASS_COL].astype("category")
class_to_int = {cat: i for i, cat in enumerate(classes_raw.cat.categories, start=1)}  # start at 1
gdf["_class_id"] = classes_raw.map(class_to_int)

print("Class mapping:", class_to_int)


# Rasterize polygons into label image

shapes = [(geom, cid) for geom, cid in zip(gdf.geometry, gdf["_class_id"]) if not geom.is_empty]
labels_raster = rasterize(
    shapes=shapes,
    out_shape=(height, width),
    transform=transform,
    fill=255,                 # 255 = background/ignore
    dtype="int32"
)


# Read raster bands and build features

with rasterio.open(RASTER_PATH) as src:
    arr = src.read()  # shape: (bands, height, width)
    # Build a raster NoData mask: invalid where any band is nodata or NaN
    nodata_mask = np.zeros((height, width), dtype=bool)
    if src.nodata is not None:
        for b in range(bands):
            nodata_mask |= (arr[b] == src.nodata)
    # Also protect against NaNs
    nodata_mask |= ~np.isfinite(arr).all(axis=0)

# -----------------------
# Flatten features (X) and labels (y)
# -----------------------
X_all = arr.reshape(bands, -1).T         # (n_pixels, n_bands)
y_all = labels_raster.reshape(-1)        # (n_pixels,)

# -----------------------
# 5) Keep only labeled, valid pixels
# -----------------------
valid = (y_all != 255) & (~nodata_mask.reshape(-1))
X_valid = X_all[valid]
y_valid = y_all[valid]

print("Pixels per class (before subsampling):", Counter(y_valid))

# -----------------------
# Optional: Subsample per class to balance
# -----------------------
if N_SAMPLES_PER_CLASS is not None:
    idxs = []
    rng = np.random.default_rng(42)
    for c in np.unique(y_valid):
        class_idx = np.where(y_valid == c)[0]
        if len(class_idx) > N_SAMPLES_PER_CLASS:
            class_idx = rng.choice(class_idx, size=N_SAMPLES_PER_CLASS, replace=False)
        idxs.append(class_idx)
    idxs = np.concatenate(idxs)
    # shuffle
    rng.shuffle(idxs)
    X_valid = X_valid[idxs]
    y_valid = y_valid[idxs]

print("Pixels per class (after subsampling):", Counter(y_valid))

# -----------------------
# Save to .npy so your snippet can run
# -----------------------
np.save(SAVE_FEATURES, X_valid)
np.save(SAVE_LABELS, y_valid)
print(f"Saved {SAVE_FEATURES} with shape {X_valid.shape}, {SAVE_LABELS} with shape {y_valid.shape}")

# -----------------------
# Train & evaluate (your snippet, with a few best practices)
# -----------------------
X_train, X_test, y_train, y_test = train_test_split(
    X_valid, y_valid, test_size=0.3, random_state=42, stratify=y_valid
)

model = RandomForestClassifier(
    n_estimators=500,
    n_jobs=-1,
    class_weight="balanced_subsample",
    random_state=42
)
model.fit(X_train, y_train)

y_pred = model.predict(X_test)
print("Accuracy:", accuracy_score(y_test, y_pred))
print(classification_report(y_test, y_pred))


```

---

## Deep Learning

```python
import json
import numpy as np
import tensorflow as tf
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score, classification_report
from sklearn.utils.class_weight import compute_class_weight


# Load per-pixel features and labels
#    (same .npy files you created for RandomForest)

X = np.load("features.npy")  # shape: (n_samples, n_features)
y = np.load("labels.npy")    # shape: (n_samples,)

# If background 255 slipped in, drop it
mask = (y != 255)
X = X[mask]
y = y[mask]
print("Data:", X.shape, y.shape)


# Remap labels to 0..K-1 (required by SparseCategoricalCrossentropy)

classes = np.unique(y)
orig2new = {int(c): i for i, c in enumerate(classes)}
new2orig = {i: int(c) for i, c in enumerate(classes)}
y_mapped = np.vectorize(lambda t: orig2new[int(t)])(y)
num_classes = len(classes)
n_features = X.shape[1]
print("Classes:", classes)
print("num_classes:", num_classes)

# Save mapping for later (e.g., when exporting a map)
with open("class_mapping.json", "w") as f:
    json.dump({"orig2new": orig2new, "new2orig": new2orig}, f, indent=2)


# Train/test split and simple normalization (z-score)

X_train, X_test, y_train, y_test = train_test_split(
    X, y_mapped, test_size=0.3, random_state=42, stratify=y_mapped
)

mu = X_train.mean(axis=0)
sd = X_train.std(axis=0)
sd[sd == 0] = 1.0

def norm(z): return (z - mu) / sd
X_train = norm(X_train).astype(np.float32)
X_test  = norm(X_test).astype(np.float32)

# Optionally compute class weights for imbalance
class_weight_vals = compute_class_weight(
    class_weight="balanced",
    classes=np.arange(num_classes),
    y=y_train
)
class_weight = {i: float(w) for i, w in enumerate(class_weight_vals)}
print("Class weights:", class_weight)


# Build a tiny MLP (very simple)

inputs = tf.keras.Input(shape=(n_features,))
x = tf.keras.layers.Dense(128, activation="relu")(inputs)
x = tf.keras.layers.Dropout(0.2)(x)
x = tf.keras.layers.Dense(64, activation="relu")(x)
outputs = tf.keras.layers.Dense(num_classes, activation="softmax")(x)
model = tf.keras.Model(inputs, outputs)

model.compile(
    optimizer=tf.keras.optimizers.Adam(1e-3),
    loss=tf.keras.losses.SparseCategoricalCrossentropy(),
    metrics=[tf.keras.metrics.SparseCategoricalAccuracy(name="acc")]
)


# Train with EarlyStopping (simple & robust)

callbacks = [
    tf.keras.callbacks.EarlyStopping(monitor="val_acc", mode="max", patience=10, restore_best_weights=True)
]

history = model.fit(
    X_train, y_train,
    validation_split=0.15,
    epochs=100,
    batch_size=4096,
    callbacks=callbacks,
    class_weight=class_weight,
    verbose=2
)


# Evaluate

y_pred = np.argmax(model.predict(X_test, verbose=0), axis=1)
print("Test accuracy:", accuracy_score(y_test, y_pred))
print(classification_report(y_test, y_pred, digits=4))
model.save("mlp_pixel_model")
np.savez("normalizer_stats.npz", mean=mu, std=sd)
print("Saved model and normalizer stats.")

```



## Exercises

1. Train a Random Forest model using tabular crop feature data.
2. Extract and save raster patches labeled by crop type.
3. Train a CNN on patch data and compare results to RF.
4. Visualize model predictions as classification maps.
5. Save trained models and test them on new images.

---

⬅️ [Previous: Google Earth Engine Python API](GEE_Python_API.md)
