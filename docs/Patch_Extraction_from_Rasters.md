Patch extraction is a technique used in geospatial analysis and machine learning to break down large raster images into smaller, manageable chunks or **patches**.

This is useful for:

- Training machine learning models
- Managing memory and computational load
- Spatial tiling for deep learning

---

#### Required Libraries

```python
import rasterio
import numpy as np
from rasterio.windows import Window
```

---

#### Set Patch Size and Loop Through Image

```python
patch_size = 64  # Size of each patch (64x64 pixels)

with rasterio.open("nyagatare_image.tif") as src:
    for i in range(0, src.height, patch_size):
        for j in range(0, src.width, patch_size):
            window = Window(j, i, patch_size, patch_size)
            patch = src.read(window=window)
            np.save(f"patch_{i}_{j}.npy", patch)
```

This will save each patch as a `.npy` file, which can later be loaded for training or analysis.

---

#### Optional: Filter Valid Patches

You can skip patches that are full of NoData values or have too much cloud cover:

```python
if np.all(patch == 0):
    continue  # skip empty patch
```

---

#### Exercises

1. Extract 64×64 patches from a raster and save them as `.npy` files.
2. Try different patch sizes like 32, 128.
3. Count how many patches were extracted.
4. Create a mask to skip patches with low information content.
5. Visualize one patch using `matplotlib`.

```python
import matplotlib.pyplot as plt
plt.imshow(patch[0], cmap='gray')
plt.title("Sample Patch - Band 1")
plt.show()
```

---

#### Tip

If you need overlapping patches (e.g., for segmentation), reduce the stride:

```python
stride = 32  # overlap
```

This increases the number of patches and improves spatial learning in ML.