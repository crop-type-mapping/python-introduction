This section introduces both **Machine Learning (Random Forest)** and **Deep Learning (Convolutional Neural Networks - CNN)** approaches.

---

#### Machine Learning with Random Forest

Random Forest is an ensemble learning method that works well with tabular data extracted from raster patches.

##### Example Workflow

```python
from sklearn.ensemble import RandomForestClassifier
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score
import numpy as np

# Load features and labels
X = np.load("features.npy")  # shape: (n_samples, n_features)
y = np.load("labels.npy")    # shape: (n_samples,)

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

#### Deep Learning with CNN (using Keras)

CNNs are ideal for image classification. Here, we classify raster patches.

##### CNN Example

```python
import tensorflow as tf
from tensorflow.keras import layers, models

# Assume X has shape (n_samples, height, width, bands)
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

#### Model Evaluation

```python
# Evaluate accuracy
loss, accuracy = model.evaluate(X_test, y_test)
print("CNN Accuracy:", accuracy)
```

---

#### Tips

- Normalize image data before training.
- Use class weights if dataset is imbalanced.
- Save models using `model.save()` for later reuse.

---

#### Exercises

1. Train a Random Forest model using CSV or NumPy feature data.
2. Extract patches and labels to train a CNN model.
3. Compare accuracy between RF and CNN.
4. Visualize a prediction mask using matplotlib.
5. Save and reload your trained model.