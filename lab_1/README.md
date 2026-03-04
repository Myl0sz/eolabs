# EO Labs

Practical notebooks for exploring Earth Observation data with Python.

---

## Contents

| File | Description |
|---|---|
| `visualise_imagery.ipynb` | Quick side-by-side visualisation of VHR, Sentinel-2 and Landsat over Istanbul |
| `eo_lab_1.ipynb` | Guided lab: multi-resolution comparison, spectral indices (NDVI, NDWI), GeoTIFF export |
| `environment.yml` | Conda environment file (recommended, works on all platforms) |
| `requirements.txt` | Pip dependencies (Linux / macOS only) |
| `data/` | Local raster data (see structure below) |

### Data layout

```
data/
  vhr/
    istanbul_vhr.tiff          # Very high resolution RGB (~1 m), EPSG:32635
  stac_downloads/
    sentinel2/
      s2_B02.tif               # Blue  (10 m)
      s2_B03.tif               # Green (10 m)
      s2_B04.tif               # Red   (10 m)
      s2_B08.tif               # NIR   (10 m)
    landsat/
      landsat_blue.tif
      landsat_green.tif
      landsat_red.tif
      landsat_nir08.tif        # all bands 30 m
```

All rasters are in **EPSG:32635 (UTM zone 35N)**, covering Istanbul, Turkey.

---

## Setup

### 1. Clone the repository

```bash
git clone https://github.com/mloopa/eolabs.git
cd eolabs/lab_1
```

> The data files are stored with Git LFS. Make sure
> [Git LFS](https://git-lfs.com) is installed before cloning,
> otherwise the raster files will be downloaded as LFS pointer stubs.
> Run `git lfs pull` inside the repo if that happens.

---

### Option A — Conda (recommended, works on Windows, macOS and Linux)

`rasterio` depends on GDAL. On Windows in particular, installing via pip
alone often produces a build without the required GDAL drivers, causing
errors like *"not recognized as being in a supported file format"*.
Conda resolves all native dependencies automatically.

Install [Miniforge](https://github.com/conda-forge/miniforge/releases/latest)
(or Anaconda / Miniconda), then:

```bash
conda env create -f environment.yml
conda activate eolabs
```

That single command installs Python, rasterio with full GDAL support,
numpy, matplotlib and JupyterLab.

---

### Option B — pip (Linux / macOS only)

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

> **Do not use pip on Windows.** The rasterio pip wheel for Windows
> does not bundle all GDAL drivers and will fail to open GeoTIFF files.
> Use Option A instead.

---

### 2. Launch Jupyter

```bash
jupyter lab
```

Make sure you launch Jupyter **from inside the `lab_1/` directory**
so that relative paths to `data/` resolve correctly:

```bash
cd eolabs/lab_1
jupyter lab
```

Open `eo_lab_1.ipynb` to start the lab.

---

## Lab 1 overview

The notebook walks through:

1. Loading raster files with `rasterio` and inspecting metadata
2. Clipping all sensors to a common geographic extent
3. Visualising true colour composites at native resolution
4. Grayscale conversion and false colour (NIR/R/G)
5. Computing NDVI and comparing it across sensors
6. **Final challenge:** implement NDWI and export results to GeoTIFF for QGIS

Output files are written to `data/outputs/`.
