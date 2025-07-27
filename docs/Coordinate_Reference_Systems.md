A **Coordinate Reference System (CRS)** defines how the two-dimensional, projected map in your GIS relates to real places on the earth.

CRS is crucial for **aligning** and **analyzing** geospatial data accurately.

---

#### Types of CRS

There are two main types of CRS:

| Type           | Description                                 | Example            |
| -------------- | ------------------------------------------- | ------------------ |
| Geographic CRS | Uses latitude and longitude (angular units) | EPSG:4326 (WGS 84) |
| Projected CRS  | Uses meters or feet on a flat surface       | EPSG:32636 (UTM)   |

---

#### Checking and Setting CRS

```python
import geopandas as gpd

# Load shapefile
gdf = gpd.read_file("farms.shp")

# Check CRS
print(gdf.crs)

# Reproject to UTM Zone 36N
gdf_utm = gdf.to_crs("EPSG:32636")
print(gdf_utm.crs)
```

You can reproject raster files too:

```python
import rasterio
from rasterio.warp import calculate_default_transform, reproject, Resampling

with rasterio.open("satellite.tif") as src:
    dst_crs = "EPSG:4326"
    transform, width, height = calculate_default_transform(
        src.crs, dst_crs, src.width, src.height, *src.bounds
    )

    kwargs = src.meta.copy()
    kwargs.update({
        'crs': dst_crs,
        'transform': transform,
        'width': width,
        'height': height
    })

    with rasterio.open("reprojected.tif", 'w', **kwargs) as dst:
        for i in range(1, src.count + 1):
            reproject(
                source=rasterio.band(src, i),
                destination=rasterio.band(dst, i),
                src_transform=src.transform,
                src_crs=src.crs,
                dst_transform=transform,
                dst_crs=dst_crs,
                resampling=Resampling.nearest
            )
```

---

#### Why CRS Matters

- Ensures datasets align correctly on the map
- Allows spatial measurements (distance, area)
- Required for overlays, joins, or clipping operations

---

#### Common EPSG Codes

| EPSG Code | Name                    | Description                    |
| --------- | ----------------------- | ------------------------------ |
| 4326      | WGS84                   | Global Lat/Lon (GPS)           |
| 3857      | Web Mercator            | Used in web maps (Google, OSM) |
| 32636     | UTM Zone 36N (WGS84)    | East Africa (meters)           |
| 21097     | Arc 1960 / UTM zone 37S | Kenya                          |

---

#### Exercises

1. Load a vector file and print its CRS.
2. Reproject a shapefile from WGS84 to UTM.
3. Load a raster and display its CRS.
4. Reproject a raster using Rasterio and save the output.
5. Bonus: Search the EPSG registry and find the correct code for your country’s UTM zone.
