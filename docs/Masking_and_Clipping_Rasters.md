Clipping a raster by a shapefile (Area of Interest - AOI) allows you to focus your analysis on a specific region, such as a farm or district.

---

#### Required Libraries

```python
import rasterio
import geopandas as gpd
from rasterio.mask import mask
from rasterio.plot import show
```

---

#### Load the Shapefile (AOI)

```python
# Read AOI shapefile
aoi = gpd.read_file("rwanda_districts.shp")
print(aoi.crs)
```

Make sure the CRS of the shapefile matches the raster.

---

#### Clip Raster by AOI

```python
with rasterio.open("nyagatare_image.tif") as src:
    # Reproject AOI to match raster CRS
    aoi = aoi.to_crs(src.crs)

    # Mask raster using shapefile geometry
    clipped_image, clipped_transform = mask(src, aoi.geometry, crop=True)
    clipped_meta = src.meta.copy()

# Update metadata
clipped_meta.update({
    "height": clipped_image.shape[1],
    "width": clipped_image.shape[2],
    "transform": clipped_transform
})

# Save clipped raster
with rasterio.open("nyagatare_clipped.tif", "w", **clipped_meta) as dst:
    dst.write(clipped_image)
```

---

#### Visualize the Clipped Raster

```python
show(clipped_image[0], title="Clipped Raster")
```

---

#### Why Clip Rasters?

- Reduce file size and processing time
- Focus analysis on relevant regions
- Prepare data for machine learning

---

#### Exercises

1. Load a shapefile and print its CRS.
2. Reproject the shapefile to match the raster CRS.
3. Use `rasterio.mask.mask` to clip the raster.
4. Save the clipped raster and view its metadata.
5. Display the clipped raster using matplotlib or rasterio.

---

#### Tips

- If your AOI has multiple polygons, consider simplifying it or clipping one feature at a time.
- You can use `.buffer(0)` on geometries to fix invalid shapes.

---

⬅️ [Previous: Opening and Exploring Raster Files](Opening_and_Exploring_Raster_Files.md) | [Next: Patch Extraction from Rasters ➡️](Patch_Extraction_from_Rasters.md)
