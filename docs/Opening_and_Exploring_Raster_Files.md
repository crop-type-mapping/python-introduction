Raster data represents the world as a grid of cells (pixels), commonly used for satellite imagery, elevation models, and NDVI.

Each pixel contains a value such as reflectance, temperature, or vegetation index.

---

#### Opening a Raster File

Use the `rasterio` library to open and read raster files like `.tif`.

```python
import rasterio

# Open raster
raster_path = "nyagatare_image.tif"
with rasterio.open(raster_path) as src:
    print("Raster opened successfully!")
    print(src.profile)  # Metadata
    print("CRS:", src.crs)
    print("Width, Height:", src.width, src.height)
    print("Number of Bands:", src.count)
```

---

#### Reading Raster Bands

You can read individual bands or all bands into NumPy arrays.

```python
with rasterio.open(raster_path) as src:
    band1 = src.read(1)  # Read the first band
    print("Band shape:", band1.shape)
```

Bands represent different wavelengths or indices (e.g., Red, NIR, NDVI).

---

#### Visualizing Raster Data

Use `rasterio.plot.show()` or matplotlib to display raster images.

```python
from rasterio.plot import show
import matplotlib.pyplot as plt

with rasterio.open(raster_path) as src:
    show(src.read(1), title="Raster Band 1")

# Or manually plot with matplotlib
plt.imshow(src.read(1), cmap='viridis')
plt.title("Raster Band 1")
plt.colorbar()
plt.show()
```

---

#### Raster Metadata and Properties

Each raster contains metadata describing its structure and location.

```python
with rasterio.open(raster_path) as src:
    print("Bounds:", src.bounds)
    print("Resolution:", src.res)
    print("Data Type:", src.dtypes)
```

---

#### Exercises

1. Load a raster and print its metadata and number of bands.
2. Read and display the first band using `rasterio`.
3. Extract the pixel resolution and CRS.
4. Calculate the min and max of a raster band.
5. Try plotting the band with a different color map (`cmap`).

---

#### Tip

If you're using multi-band images (e.g., RGB or multispectral), explore combinations like:

```python
r = src.read(3)
g = src.read(2)
b = src.read(1)
rgb = np.stack([r, g, b], axis=-1)
plt.imshow(rgb / 255)
```