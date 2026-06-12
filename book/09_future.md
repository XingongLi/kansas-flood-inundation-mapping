---
jupytext:
  text_representation:
    extension: .md
    format_name: myst
    format_version: 0.13
    jupytext_version: 1.11.5
kernelspec:
  display_name: Python 3
  language: python
  name: python3
---


# Future Improvements

* Code improvements
  * Consolidate the code on PC and Z drive
  * Create a github repo for the project (using cookie cutter?)
  * Blob storage mapping error
  * Use VSC to run code on PC
  * Warnings (chunk size?) when writing some COG files
  * Mosaic COG or compression
* Implement vertical DOF interpolation
  * Using 100-year flood profile if available
  * See Jude’s slides and James notebook
* Forecast and postcast
  * Use NOAA 2-week max stage forecast, but many gauges are don'r have forecasts
  * NWM discharge forecast at the gauges with rating curves or use NWM synthetic rating curves
* Postcast and historical events
  * past n-day max gauge stage
  * Mining gauge observations and identify historical flood event stages and flood maps for reference
  * Mining [NOAA National Water Model Reanalysis](https://www.cuahsi.org/about/news/big-data-dreaming-a-42-year-conus-hydrologic-retrospective) for historical flood events
* **Streamlit flood dashboard and streamlit geospatial dashboard template**
  * Streamlit dashboard for flood mapping
  * Streamlit dashboard for general geospatial mapping
* Stream orders
  * Change “stream_order_info.csv” to “stream_order_network _info.csv” for a more meaningful name?
  * Automate stream orders; same order for different tributaries?
    * Flow accumulation or longest consecutive segments is first order, other orders from there. Segment’s downstream distance has already calculated! And flow accumulation can be calculated in a similar way!
    * Assign a stream ID for each branch
    * Use detailed stream flow data for determining stream order and flood priority analysis?
      * StreamStats
      * NHDPlus HR (high resolution) which has the mean annual flow (1971-2000) for each flowline
      * [NOAA National Water Model Reanalysis](https://www.cuahsi.org/about/news/big-data-dreaming-a-42-year-conus-hydrologic-retrospective)
        * Hourly stream flow (1979-2020) for millions of NWM reaches
        * Available on AWS and Google Cloud. See this [notebook](https://nbviewer.jupyter.org/gist/rsignell-usgs/d3dfaf3cd3d8b39894a69b22127dfe38) on how to use the stream flow data. Additional notebooks using the data can be found at [jmframe github site](https://github.com/jmframe/nwm-reanalysis-model-data-processing), especially [average_streamflow_local_day.ipynb](https://github.com/jmframe/nwm-reanalysis-model-data-processing/blob/master/average_streamflow_local_day.ipynb). The data is available on AWS.
* Automatically update gage datum elevation from AHPS/USGS web site? 
* EstimateFspDofFromGauge() may be further improved using just FSP unique ID (i.e., ‘FspId’) instead of FSP’s coordinates.
* Mosaic tile GeoTifs (implemented 3 functions). Tested on the largest library morv and it works even with the in-memory mosaicing function. **The most comprehensive version (filed-based) is still not working and slower, see TestMosiac.py.**
* Update the script in this doc. Open source and arcpy versions
* **Speed up the merge in TileFspFppRelations2Array()**
  * Another possibility is to index the FSP IDs before merging. Index can be stored with the relations or created in memory before merge. 
  * Saving the index (FSP coordinates) seemed not speeding up compared with creating index on-the-fly!
* MapBySegment.py using MapFloodDepth()?
* Flood mapping on GEE using reduceToRaster() to turn FSP-FPP relations into raster?
* **Can we use FLDPLN to estimate channel roughness and channel geometry?**
