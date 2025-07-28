

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
from sklearn.ensemble import RandomForestClassifier
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score
import numpy as np

# Load tabular features and crop labels
X = np.load("features.npy")  # (n_samples, n_features)
y = np.load("labels.npy")    # (n_samples,)

# Split data
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3)

# Train model
model = RandomForestClassifier()
model.fit(X_train, y_train)

# Predict and evaluate
y_pred = model.predict(X_test)
print("Accuracy:", accuracy_score(y_test, y_pred))
```

---

## Deep Learning with CNN (using Keras)

CNNs are ideal for classifying raw raster patches without manual feature extraction.

### CNN Example

```python
import tensorflow as tf
from tensorflow.keras import layers, models

# Load image patches (e.g., 64×64×bands)
X = np.load("patches.npy")
y = np.load("labels.npy")

# Split data
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3)

# Define CNN model
model = models.Sequential([
    layers.Conv2D(32, (3, 3), activation='relu', input_shape=X.shape[1:]),
    layers.MaxPooling2D((2, 2)),
    layers.Conv2D(64, (3, 3), activation='relu'),
    layers.Flatten(),
    layers.Dense(64, activation='relu'),
    layers.Dense(len(np.unique(y)), activation='softmax')
])

model.compile(optimizer='adam',
              loss='sparse_categorical_crossentropy',
              metrics=['accuracy'])

# Train model
model.fit(X_train, y_train, epochs=10, batch_size=32, validation_split=0.2)
```

---

## Model Evaluation

```python
# Evaluate CNN accuracy
loss, accuracy = model.evaluate(X_test, y_test)
print("CNN Accuracy:", accuracy)
```

---

## Tips
- Normalize patch values (0–1) before training CNNs.
- Use class weights if crop types are imbalanced.
- Use `model.save()` to reuse trained models later.

---

## Exercises

1. Train a Random Forest model using tabular crop feature data.
2. Extract and save raster patches labeled by crop type.
3. Train a CNN on patch data and compare results to RF.
4. Visualize model predictions as classification maps.
5. Save trained models and test them on new images.

---

⬅️ [Previous: Google Earth Engine Python API](GEE_Python_API.md)
