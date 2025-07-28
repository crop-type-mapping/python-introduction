Google Earth Engine (GEE) is a cloud-based platform for planetary-scale geospatial analysis. Its Python API allows users to access GEE datasets and run computations using Python scripts.

---

#### Setup and Authentication

Install Earth Engine Python API:

```bash
pip install earthengine-api
```

Authenticate and initialize:

```python
import ee
ee.Authenticate()
ee.Initialize()
```

---

#### Load and Filter a Dataset

Example: Load Sentinel-2 imagery for a region and time range.

```python
point = ee.Geometry.Point([30.05, -1.95])  # Example location

collection = (
    ee.ImageCollection('COPERNICUS/S2_SR')
    .filterBounds(point)
    .filterDate('2021-01-01', '2021-03-31')
    .filter(ee.Filter.lt('CLOUDY_PIXEL_PERCENTAGE', 10))
)

image = collection.median()
```

---

#### Export Image to Google Drive

```python
task = ee.batch.Export.image.toDrive(
    image=image,
    description='S2_Nyagatare_2021Q1',
    folder='GEE_exports',
    fileNamePrefix='sentinel2_2021',
    region=point.buffer(10000).bounds().getInfo()['coordinates'],
    scale=10,
    crs='EPSG:4326'
)
task.start()
```

---

#### Visualize with Folium

```python
import folium
from geemap import foliumap

Map = folium.Map(location=[-1.95, 30.05], zoom_start=10)
Map.add_child(folium.TileLayer('Stamen Terrain'))

vis_params = {'min': 0, 'max': 3000, 'bands': ['B4', 'B3', 'B2']}
Map.add_ee_layer(image, vis_params, 'Sentinel-2')
Map
```

> Note: You need `geemap` for `add_ee_layer`.

```bash
pip install geemap
```

---

#### Tips

- Use `.first()` to inspect the first image in a collection.
- Combine with `geopandas` for geospatial integration.
- Use the Code Editor (code.earthengine.google.com) to validate expressions.

---

#### Exercises

1. Load Landsat-8 images for a region of interest and filter by date/clouds.
2. Export an NDVI image to Google Drive.
3. Visualize an image using Folium and geemap.
4. Use `.mean()` instead of `.median()` and compare results.

---

#### Resources

- [GEE Python API Docs](https://developers.google.com/earth-engine/guides/python_install)
- [Geemap Docs](https://geemap.org/)


---

⬅️ [Previous: Patch Extraction from Rasters](Patch_Extraction_from_Rasters.md) | [Next: Geospatial AI Summary ➡️](Crop_Classification_ML_DL.md)
