In geospatial applications, data is primarily represented in two formats: **vector** and **raster**.  
Each format has its strengths depending on the type of spatial feature or analysis you're performing.

---

#### Vector Data

Vector data represents real-world features using geometric shapes:

- **Points**: Individual locations (e.g., trees, wells)
- **Lines**: Linear features (e.g., rivers, roads)
- **Polygons**: Areas (e.g., farm plots, districts)

Each feature can have **attributes** (e.g., crop type, owner).

```python
import geopandas as gpd

# Load a shapefile
gdf = gpd.read_file('farms.shp')

# Inspect the data
print(gdf.head())
print(gdf.geometry[0])
```

Vector data is ideal for:

- Boundary definitions
- Spatial joins and queries
- Attribute-rich analysis

---

#### Raster Data

Raster data is a grid of pixels where each pixel holds a value (e.g., reflectance, temperature, elevation).

```python
import rasterio
from rasterio.plot import show

# Load a raster image
with rasterio.open('satellite_image.tif') as src:
    print("Bands:", src.count)
    print("Size:", src.width, "x", src.height)
    show(src.read(1))  # Show first band
```

Raster data is ideal for:

- Satellite imagery
- Continuous surfaces (NDVI, elevation)
- Image classification and time series

---

#### Key Differences

| Feature      | Vector                         | Raster                        |
| ------------ | ------------------------------ | ----------------------------- |
| Structure    | Points, lines, polygons        | Grid of pixels                |
| Best For     | Boundaries, discrete objects   | Surfaces, continuous data     |
| Storage      | Geometries + attributes        | Numeric pixel values          |
| File formats | `.shp`, `.geojson`, `.gpkg`    | `.tif`, `.img`, `.nc`, `.hdf` |
| Resolution   | Based on digitization accuracy | Based on pixel size           |

---

#### Choosing the Right Format

| Task                         | Recommended Format |
| ---------------------------- | ------------------ |
| Mapping farm boundaries      | Vector             |
| Analyzing vegetation (NDVI)  | Raster             |
| Combining spatial attributes | Vector             |
| Image classification         | Raster             |

---

#### Exercises

1. Open a shapefile using GeoPandas and print the geometry type.
2. Open a raster using Rasterio and print metadata like CRS and size.
3. Plot a vector and raster layer together using `matplotlib`.
4. Describe three real-world use cases where each format is better suited.
5. Bonus: Convert NDVI raster to vector by thresholding and vectorizing high NDVI areas.

---

⬅️ [Previous: Functions and File I/O](Functions_and_File_IO.md) | [Next: Coordinate Reference Systems (CRS) ➡️](Coordinate_Reference_Systems.md)
