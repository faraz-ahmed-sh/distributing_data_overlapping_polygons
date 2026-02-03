# Claude.md - Project Guide

## Project Overview

This is a geospatial data analysis project that demonstrates how to distribute data from one geographic level to another using spatial area overlap calculations. The method apportions data based on the proportion of area that one polygon occupies within another.

**Primary Use Case**: Distributing American Community Survey (ACS) demographic data from Census Tract level to Election District level in NYC.

## Tech Stack

- **Language**: Python 3
- **Core Libraries**:
  - GeoPandas - GIS operations on geometries
  - Pandas - Data manipulation
  - Shapely - Geometric operations
  - Fiona - Vector spatial data I/O
  - Rtree - Spatial indexing
  - Pyproj - Coordinate reference system projections

## Project Structure

```
distributing_data_overlapping_polygons/
├── README.md                                         # Project description
├── requirements.txt                                  # Python dependencies
├── distributing_data_by_overlapping_polygons.ipynb  # Main Jupyter notebook
└── img/                                              # Reference images
    ├── overlay.JPG
    └── projections.jpg
```

## Setup & Running

### Installation (conda-forge recommended)

```bash
conda install -c conda-forge geopandas fiona shapely rtree
pip install pandas numpy pyproj six
```

### Running the Notebook

```bash
pip install -r requirements.txt
jupyter notebook distributing_data_by_overlapping_polygons.ipynb
```

**Note**: Internet access required - the notebook fetches shapefiles from NYC Open Data Portal API.

## Key Concepts

### Coordinate Reference Systems
- **Input**: WGS 84 (EPSG:4326) - geographic coordinates
- **Calculation**: Mercator (EPSG:3395) - for accurate area measurements

### Data Processing Workflow
1. Fetch GeoJSON from NYC Open Data via HTTP
2. Load into GeoPandas GeoDataFrame
3. Project to Mercator for accurate area calculations
4. Perform geometric intersection overlay
5. Calculate proportional metrics
6. Validate with QA checks

### Main Function
- `import_shapefile_and_area_in_sqkm()` - Downloads and processes shapefiles, calculates area in km²

## Data Sources

- **Election Districts**: NYC Open Data (ID: h2n3-98hq) - 5,840 districts
- **Census Tracts 2010**: NYC Open Data (ID: fxpq-c8ku) - 2,165 tracts

## Important Notes

- GeoPandas has complex spatial dependencies (GDAL, GEOS, Proj) - use conda-forge for reliable installation
- The method does not account for population density - it's purely area-based
- Quality assurance: proportions should sum to ~1.0 per census tract
