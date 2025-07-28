Managing Python environments is essential for avoiding package conflicts and keeping your projects isolated. Below are common tools and methods for environment management.

---

#### Using `venv` (Built-in Python Virtual Environment)

```bash
# Create environment
python -m venv myenv

# Activate
source myenv/bin/activate      # Linux/macOS
myenv\Scripts\activate.bat   # Windows

# Deactivate
deactivate
```

Install packages inside the environment:

```bash
pip install numpy pandas
```

---

#### Using `virtualenv`

A more flexible tool, especially for older Python versions.

```bash
pip install virtualenv
virtualenv myenv
source myenv/bin/activate
```

---

#### Using `conda` (Anaconda or Miniforge)

```bash
# Create a new environment
conda create -n crop_env python=3.10

# Activate
conda activate crop_env

# Install packages
conda install numpy pandas rasterio

# Deactivate
conda deactivate
```

##### Miniforge

Lightweight alternative to Anaconda with conda-forge as default channel:

```bash
# Download and install Miniforge
wget https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-Linux-x86_64.sh
bash Miniforge3-Linux-x86_64.sh
```

---

#### Export and Reuse Environment

Export environment:

```bash
conda env export > environment.yml
```

Recreate environment from file:

```bash
conda env create -f environment.yml
```

---

#### Using `pip-tools` for better dependency control

```bash
pip install pip-tools

# Create requirements
echo "rasterio" > requirements.in
pip-compile requirements.in
pip install -r requirements.txt
```

---

#### Tips

- Use separate environments per project.
- Prefer `conda` for geospatial libraries (e.g., rasterio, geopandas).
- Save `requirements.txt` or `environment.yml` in your repo for reproducibility.

---

#### Exercises

1. Create a `venv` or `conda` environment for your crop mapping project.
2. Install `rasterio`, `numpy`, `geopandas`, and test import.
3. Export the environment and share with a colleague.

---

⬅️ [Home](index.md) | [Next: Introduction to Python Syntax ➡️](Syntax.md)
