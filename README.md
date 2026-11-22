# Identifying the Impacts of the 2021 Texas Winter Storms on Houston's Power Grid

## Purpose
This repo contains the fourth homework assignment for EDS 223, Geospatial Analysis and Remote Sensing. The course introduces the spatial modeling and analytic techniques of geographic information science to data science students, with an emphasis on spatial data models and the analytic operations they enable.

In February 2021, a series of severe winter storms caused a major power crisis in Texas, leaving millions of residents without electricity. For this assignment, I estimate how many homes in the Houston metropolitan area lost power as a result of the storms. Using VIIRS night light imagery from before (February 7, 2021) and after (February 16, 2021) the storms, I identify areas where night light intensity dropped by more than 200 nW cm⁻² sr⁻¹. I then exclude areas near highways and use OpenStreetMap building data to count the residential buildings within the blackout areas.

## Highlights
- Merging VIIRS raster tiles and calculating the change in night light intensity with `stars`
- Reclassifying the difference raster into a blackout mask and converting it to vector data
- Cropping to the Houston area and reprojecting to EPSG:3083 (NAD83 / Texas Centric Albers Equal Area)
- Using SQL queries in `st_read()` to load only highways and residential buildings
- Buffering highways and performing spatial operations to exclude highway light and identify affected homes
- Mapping night light intensity and homes that lost power with `tmap`

## Repository Organization
```
houston-blackout-analysis
└─── README.md                  # Overview of the repository
└─── houston_blackout.qmd       # Quarto document with analysis
└─── qmd/Rmd/Proj files
└─── .gitignore                 # Excludes the data folder
     └─── data                  # Not tracked on GitHub (see Data Access)
          └─── gis_osm_buildings_a_free_1.gpkg    # OpenStreetMap buildings
          └─── gis_osm_roads_free_1.gpkg          # OpenStreetMap roads
          └─── VNP46A1                            # VIIRS night light tiles
               └─── VNP46A1.A2021038.h08v05.001.2021039064328.tif    # 2021-02-07, tile h08v05
               └─── VNP46A1.A2021038.h08v06.001.2021039064329.tif    # 2021-02-07, tile h08v06
               └─── VNP46A1.A2021047.h08v05.001.2021048091106.tif    # 2021-02-16, tile h08v05
               └─── VNP46A1.A2021047.h08v06.001.2021048091105.tif    # 2021-02-16, tile h08v06
```

## Data Access
Because the files are too large for GitHub, the `data/` folder is listed in `.gitignore`. To run the analysis, download the data from [this Google Drive link](https://drive.google.com/file/d/1bTk62xwOzBqWmmT791SbYbHxnCdjmBtw/view?usp=drive_link), unzip it, and place the files in a `data/` folder at the root of this repository.

The data includes:
- **Night lights** (`data/VNP46A1/`): VIIRS Day/Night Band daily night light imagery (VNP46A1) from NASA's Level-1 and Atmosphere Archive & Distribution System Distributed Active Archive Center (LAADS DAAC). The Houston area spans two tiles (h08v05 and h08v06), with images from February 7, 2021 (before the storms) and February 16, 2021 (after the storms).
- **Roads** (`gis_osm_roads_free_1.gpkg`): OpenStreetMap road data for the Houston metropolitan area, redistributed by Geofabrik. Only highways (motorways) are used in the analysis.
- **Buildings** (`gis_osm_buildings_a_free_1.gpkg`): OpenStreetMap building footprints for the Houston metropolitan area, redistributed by Geofabrik. Only residential buildings are used in the analysis.

## Author
Kailani Latimer ([GitHub](https://github.com/kailani-boop))

## References
- EDS 223 Homework 4 instructions: [EDS 223 HW 4](https://eds-223-geospatial.github.io/assignments/HW4.html)
- Román, M. O., Wang, Z., Sun, Q., Kalb, V., Miller, S. D., Molthan, A., Schultz, L., Bell, J., Stokes, E. C., Pandey, B., Seto, K. C., Hall, D., Oda, T., Wolfe, R. E., Lin, G., Golpayegani, N., Devadiga, S., Davidson, C., Sarkar, S., ... Masuoka, E. J. (2018). NASA's Black Marble nighttime lights product suite. *Remote Sensing of Environment*, 210, 113–143. [https://doi.org/10.1016/j.rse.2018.03.008](https://doi.org/10.1016/j.rse.2018.03.008)
- Román, M. O., Wang, Z., Shrestha, R., Yao, T., & Kalb, V. (2019). *VIIRS/NPP Daily Gridded Day Night Band 15 arc-second Linear Lat Lon Grid Night (VNP46A1), Version 1* [Dataset]. NASA LAADS DAAC. [https://doi.org/10.5067/VIIRS/VNP46A1.001](https://doi.org/10.5067/VIIRS/VNP46A1.001)
- OpenStreetMap contributors. (2021). *OpenStreetMap data for Texas* [Dataset]. Retrieved from Geofabrik Download Server. [https://download.geofabrik.de/](https://download.geofabrik.de/)
- NASA Worldview. [https://worldview.earthdata.nasa.gov/](https://worldview.earthdata.nasa.gov/)
- Wikipedia contributors. (2021, October 2). 2021 Texas power crisis. In *Wikipedia, The Free Encyclopedia*. [https://en.wikipedia.org/wiki/2021_Texas_power_crisis](https://en.wikipedia.org/wiki/2021_Texas_power_crisis)
