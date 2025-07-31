Google Earth Engine (GEE) is a cloud-based platform for planetary-scale geospatial analysis. Its Python API allows users to access GEE datasets and run computations using Python scripts.

---

#### Setup and Authentication

Install Earth Engine Python API:

```bash
pip install earthengine-api
```

```python
# --- Optional: help pyproj/GDAL find data on Windows (Conda) ---
import os

import pyproj
os.environ["PROJ_LIB"] = pyproj.datadir.get_data_dir()

import json
import geopandas as gpd
import matplotlib.pyplot as plt
import requests
from io import BytesIO
from PIL import Image
import ee
import calendar

# ------------------ CONFIG ------------------
PROJECT_ID = "#"
SHP_PATH   = r"data\shapefiles\Boundingbox_5Kmby5Km.shp"
YEAR, MONTH = 2025, 2

# Drive export settings
DRIVE_FOLDER = "gee_exports"
FILE_PREFIX  = f"ndvi_{YEAR}_{MONTH:02d}"
EXPORT_SCALE = 10
MAX_PIXELS   = 1e13
# --------------------------------------------

# ee.Authenticate()  # run once interactively
ee.Initialize(project=PROJECT_ID)

# Load AOI
gdf = gpd.read_file(SHP_PATH)
if gdf.crs is None or gdf.crs.to_epsg() != 4326:
    gdf = gdf.to_crs(4326)
gdf["geometry"] = gdf.buffer(0)
aoi_geojson = json.loads(gdf.dissolve().to_json())
aoi_fc = ee.FeatureCollection(aoi_geojson)
aoi = aoi_fc.geometry()

# Build monthly composite
start = ee.Date.fromYMD(YEAR, MONTH, 1)
end   = start.advance(1, 'month')
s2 = (ee.ImageCollection('COPERNICUS/S2_SR_HARMONIZED')
      .filterBounds(aoi)
      .filterDate(start, end))
comp = s2.median().clip(aoi)

print("Composite band names:", comp.bandNames().getInfo())

# NDVI image
ndvi = comp.normalizedDifference(['B8', 'B4']).rename('NDVI')

# --- A) Display NDVI thumbnail ---
thumb_params = {
    'region': aoi,
    'dimensions': 1024,
    'min': -0.2,
    'max': 0.9,
    'palette': ['blue','white','green']
}
thumb_url = ndvi.visualize(min=thumb_params['min'],
                           max=thumb_params['max'],
                           palette=thumb_params['palette'])                  .getThumbURL({'region': aoi, 'dimensions': thumb_params['dimensions']})

resp = requests.get(thumb_url, timeout=120)
resp.raise_for_status()
from PIL import Image
img = Image.open(BytesIO(resp.content))

plt.figure()
plt.title(f'NDVI – {YEAR}-{MONTH:02d} (no masking)')
plt.imshow(img)
plt.axis('off')
plt.show()

# --- Export the COMPOSITE (B2,B3,B4,B8,B11,B12) to Google Drive ---
analysis_comp = comp.select(['B2','B3','B4','B8','B11','B12'])

MONTH_TAG   = f"{YEAR}-{MONTH:02d}_{calendar.month_abbr[MONTH].lower()}"  # e.g., 2025-02_feb
BASE_PREFIX = f"s2_{MONTH_TAG}"  # e.g., s2_2025-02_feb


task = ee.batch.Export.image.toDrive(
    image         = analysis_comp,
    description   = f"{BASE_PREFIX}_composite_analysis_toDrive",
    folder        = DRIVE_FOLDER,
    fileNamePrefix= BASE_PREFIX,
    region        = aoi,
    scale         = 10,           
    crs           = "EPSG:4326", 
    maxPixels     = MAX_PIXELS,
    filePerBand   = False,
    fileFormat    = 'GeoTIFF'
)
task.start()
print("Started Drive export (composite analysis bands).")



```

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
