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

## Dataset Versioning & Updates

### Current Project Data (Outdated)

The project currently uses **2010 Census-era data**:

| Dataset | Version | Year | Records |
|---------|---------|------|---------|
| Census Tracts | 2010 Decennial Census | 2010 | 2,165 |
| Election Districts | ~2010 boundaries | 2010 | 5,840 |

### What Changed Since 2010

**Census Tracts (2020 Decennial Census)**:
- Boundaries redrawn every 10 years based on population changes
- 2020 Census introduced new tract boundaries
- New FIPS codes assigned to modified tracts

**Election Districts (Post-2020 Redistricting)**:
- NYC election districts redrawn after 2020 Census
- NY lost 1 Congressional seat; courts invalidated initial maps
- Current boundaries reflect 2020 redistricting

**ACS Data**:
- 2020-2024 ACS 5-year estimates now available (most recent)

### Updated Data Sources

**Census Tracts (2020)**:
| Source | URL/ID |
|--------|--------|
| NYC Open Data | https://data.cityofnewyork.us/City-Government/2020-Census-Tracts/63ge-mke6 |
| NYC Open Data (Mapped) | https://data.cityofnewyork.us/City-Government/2020-Census-Tracts-Mapped/weqx-t5xr |
| NYC DCP | https://www.nyc.gov/content/planning/pages/resources/datasets/census-tracts |
| US Census TIGER | https://catalog.data.gov/dataset/tiger-line-shapefile-2020-state-new-york-census-tracts |

**Election Districts (Current)**:
| Source | URL/ID |
|--------|--------|
| NYC Open Data | https://data.cityofnewyork.us/City-Government/Election-Districts/h2n3-98hq (same ID, updated data) |
| ArcGIS Hub | https://hub.arcgis.com/datasets/DCP::nyc-election-districts |
| NYC DCP | https://www.nyc.gov/content/planning/pages/resources/datasets/election-districts |

**ACS Demographic Data**:
| Source | URL |
|--------|-----|
| Census Bureau API | https://www.census.gov/data/developers/data-sets/acs-5year.html |
| data.census.gov | https://www.census.gov/acs/www/data/data-tables-and-tools/data-profiles/ |
| ACS Summary File | https://www.census.gov/programs-surveys/acs/data/summary-file.html |

### Upgrade Recommendations

1. Replace 2010 Census Tracts with 2020 Census Tracts (Dataset ID: `63ge-mke6`)
2. Use current Election Districts from same endpoint (ID: `h2n3-98hq`) - now reflects 2020 redistricting
3. Update ACS variables to 2020-2024 5-year estimates
4. Note: Historical comparisons require Census Bureau tract relationship/crosswalk files

## Important Notes

- GeoPandas has complex spatial dependencies (GDAL, GEOS, Proj) - use conda-forge for reliable installation
- The method does not account for population density - it's purely area-based
- Quality assurance: proportions should sum to ~1.0 per census tract
