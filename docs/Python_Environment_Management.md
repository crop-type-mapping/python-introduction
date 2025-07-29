
Managing Python environments is essential for avoiding package conflicts and keeping your projects isolated. Below are common tools and methods for environment management.

---

## Required Python Packages

To run all examples in this Training, install the following:

### Core Packages

| Package         | Purpose                                     |
|-----------------|---------------------------------------------|
| `numpy`         | Numerical operations                        |
| `pandas`        | Data manipulation                           |
| `matplotlib`    | Plotting and visualization                  |
| `scikit-learn`  | Machine learning (Random Forest, etc.)      |
| `tensorflow`    | Deep learning (CNN, model training)         |

### Geospatial Libraries

| Package         | Purpose                                     |
|-----------------|---------------------------------------------|
| `rasterio`      | Reading/writing raster data                 |
| `geopandas`     | Handling shapefiles (vector data)           |
| `fiona`         | Shapefile I/O (used by geopandas)           |
| `pyproj`        | Coordinate reference system support         |
| `shapely`       | Geometry manipulation                       |
| `earthengine-api`   | Google Earth Engine Python API       |


### Extras

| Package             | Purpose                              |
|---------------------|--------------------------------------|
| `jupyterlab`        | Interactive notebooks (optional)     |
| `ipykernel`         | Jupyter support for virtual envs     |
| `h5py`              | Reading `.h5` format (deep learning) |
| `tqdm`              | Progress bars                        |

---

## Environment Setup Methods

### A. Using `venv` (Built-in Python Virtual Environment)

```bash
python -m venv geopy
source geopy/bin/activate        # Linux/macOS
.\geopy\Scriptsctivate         # Windows
pip install numpy pandas geopandas rasterio scikit-learn tensorflow matplotlib
```

---

### B. Using `conda` (With Miniforge)

```bash
conda create -n geopy python=3.10
conda activate geopy
conda install numpy pandas matplotlib scikit-learn tensorflow rasterio geopandas fiona shapely pyproj earthengine-api

```

---

### C. Installing Miniforge

Miniforge is a lightweight Conda alternative using conda-forge:

```bash
# Download and install
wget https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-Linux-x86_64.sh
bash Miniforge3-Linux-x86_64.sh
```

---

## Using `environment.yml`

### Step 1: Create the `environment.yml` file

```yaml
name: geopy
channels:
  - conda-forge
dependencies:
  - python=3.10
  - numpy
  - pandas
  - matplotlib
  - scikit-learn
  - tensorflow
  - rasterio
  - geopandas
  - fiona
  - shapely
  - pyproj
  - earthengine-api
```

### Step 2: Create the environment from file

```bash
conda env create -f environment.yml
```

### Step 3: Activate the environment

```bash
conda activate geopy
```

### Step 4: Export your environment (for sharing)

```bash
conda env export > environment.yml
```

---

## Optional: Using `pip-tools`

```bash
pip install pip-tools

# Create a minimal requirements file
echo "rasterio
tensorflow
scikit-learn" > requirements.in
pip-compile requirements.in
pip install -r requirements.txt
```

---

## Tips

- Use a fresh environment for each new project.
- Prefer `conda` for geospatial libraries (easier installation).
- Save `environment.yml` in your repo for reproducibility.

---

## Exercises

1. Create a new conda environment for crop type classification.
2. Install required packages and verify import.
3. Export and share your environment using `environment.yml`.
4. Try launching a Jupyter Notebook to test.

---

⬅️ [Home](index.md) | [Next: Introduction to Python Syntax ➡️](Syntax.md)
