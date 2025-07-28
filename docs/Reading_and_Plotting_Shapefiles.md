Shapefiles are one of the most common formats for storing vector geospatial data.  
They typically contain **points**, **lines**, or **polygons** representing spatial features such as farms, roads, or regions.

---

#### Components of a Shapefile

A shapefile actually consists of multiple files with the same name but different extensions:

| Extension | Description                        |
| --------- | ---------------------------------- |
| `.shp`    | Geometry (points, lines, polygons) |
| `.shx`    | Shape index                        |
| `.dbf`    | Attribute table (metadata)         |
| `.prj`    | Coordinate reference system        |

You must keep these files together when loading a shapefile.

---

#### Reading a Shapefile with GeoPandas

```python
import geopandas as gpd

# Load shapefile
gdf = gpd.read_file("Nyagatare_A2021.shp")

# View first 5 rows
print(gdf.head())

# View column names
print(gdf.columns)
```

---

#### Plotting the Shapefile

```python
import matplotlib.pyplot as plt

# Simple plot
gdf.plot()
plt.title("Farm Boundaries in Nyagatare")
plt.show()

# Plot by crop type
gdf.plot(column="crop_type", legend=True, cmap="Set3")
plt.title("Crop Types in Nyagatare")
plt.xlabel("Longitude")
plt.ylabel("Latitude")
plt.show()
```

You can customize plots with colors, legends, titles, and labels.

---

#### Exploring Attribute Data

```python
# Access specific columns
print(gdf["crop_type"].value_counts())

# Filter polygons by crop type
maize_farms = gdf[gdf["crop_type"] == "Maize"]
print(maize_farms)
```

---

#### Exercises

1. Load a shapefile and print its first 5 records.
2. Plot the shapefile with default style using `.plot()`.
3. Create a color-coded plot based on the crop type or ownership.
4. Count how many features belong to each crop type.
5. Filter out polygons larger than a certain area and plot them separately.

---

#### Reproject

Use `.to_crs()` to reproject your shapefile before plotting if it appears distorted or does not align with a base map.

```python
gdf_utm = gdf.to_crs(epsg=32636)
gdf_utm.plot()
```

---

⬅️ [Previous: Coordinate Reference Systems (CRS)](Coordinate_Reference_Systems.md) | [Next: Opening and Exploring Raster Files ➡️](Opening_and_Exploring_Raster_Files.md)
