# Introduction to Python Programming

Welcome to the **Introduction to Python Programming**  
This course introduces Python programming and its application in geospatial data processing and geospatial artificial intelligence (GeoAI).

---

## Course Objectives

By the end of this course, you will be able to:
- Understand Python basics and data structures
- Work with shapefiles and raster data
- Clip, mask, and extract raster patches
- Apply machine learning and deep learning to classify crop types
- Use Google Earth Engine (GEE) for large-scale geospatial analysis

---

## Course Structure

The course is organized into four main sections:

1. **Python Environments**  
   Create and manage environments using tools like Miniforge and `venv`.


2. **Python Basics**  
   Learn variables, loops, functions, and file handling.


3. **Working with Geospatial Data**  
   Learn about vectors, rasters, CRS, and raster processing with Python and the GEE Python API.

4. **Geospatial AI**  
   Train machine learning and deep learning models to classify crop types.

---

## Requirements to Run Python

To successfully follow along, you’ll need:

### 1. Python Installation

- Recommended: [Miniforge](https://github.com/conda-forge/miniforge)
  - Lightweight Conda-based Python distribution
  - Works well for geospatial libraries (e.g., `rasterio`, `geopandas`)
- Alternative:
  - [Python.org](https://www.python.org/downloads/)
  - [Anaconda](https://www.anaconda.com/products/distribution)

### 2. Code Editors

You can write and run Python code using:

- [Visual Studio Code (VS Code)](https://code.visualstudio.com/)
- [JupyterLab](https://jupyter.org/)
- [PyCharm](https://www.jetbrains.com/pycharm/)
- Online alternative: [Google Colab](https://colab.research.google.com) (no installation needed)

> Google Colab is especially helpful when using GPUs or accessing cloud resources.

### 3. Creating Python Environments

- **With Miniforge (recommended)**:
  ```bash
  conda create -n geopy python=3.10
  conda activate geopy
  conda install geopandas rasterio scikit-learn matplotlib
  ```

- **With venv (built-in)**:
  ```bash
  python -m venv geopy
  source geopy/bin/activate  # or .\geopy\Scripts\activate on Windows
  pip install geopandas rasterio scikit-learn matplotlib
  ```

---

## Let's Get Started

Head to the next section: [Python Environment Management](Python_Environment_Management.md)

