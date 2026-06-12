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


# System Description

This serves as a general system description for the Flood Mapping project which presents the major components and steps of the project. It's useful to understand, use and replicate the project.

## Generate FLDPLN Libraries

Jude & James

## Tile Libraries. 

See ReorganizeLibrary.ipynb. The general steps include:
* Get or download original segment-based libraries 
* Get cell size and spatial reference and save them in a library meta file
  * Library’s spatial reference is saved in ESRI shapefile’s .prj file
* Clean up FSPs and segments and create FSP and segment info 
  * Assign FSPs unique IDs. 
  * Calculate FSP and segment downstream distance which creates two files: fsps_info.csv and segments_info.csv. 
  * In the process, clean up the segment table:
    * Remove the segment if it's not in the FSP table
    * If the removed segment is the downstream segment of another segment, set it as 0 (i.e., as watershed outlets). Those missing segments are usually because of they are close to or in waterbodies. By removing those segment, a library may have several separate watersheds/outlets!
    * For example, neosho has 3 separate watersheds (segment 13, 104, 186 as the outlet segments). 
* Tile libraries (i.e., segments to tiles)
  * Turn segment-based FSP-FPP relations to tile-based relations
  * Turn coordinate-based relations to FSP ID and FPP row & column (within a tile) based relation
  * Create tile index file (FLDPLN_tiled_tile_index.csv), and  tile-fsp index file (FLDPLN_tiled_fsp_index.csv) for the relations
* Generate segment shapefile for manually assign stream order which is used in interpolating FSP depth of flow (DOF)  
* Manually assign stream order (in GIS)
* Get stream order for both FSPs and segments for use in FSP DOF interpolation  
* Create stream order network information table storing the connectivity among stream orders with columns: [‘StrOrd’, ‘DsStrOrd’, ‘JunctionFspX’, ‘JunctionFspY’]. This information is used in stage interpolation.

### Scripts
* fldpln_header.py – contains all the constants used in both building tiled library and mapping
* fldpln_reorg.py – contains all the functions for building tiled library
* ReorganizeLibrary.py – main script to build tiled library
* ReorganizeLibrary.ipynb - notebook to build tiled library

## Stream flood mapping
The steps are:
* Get gauge observations from NOAA AHPS web site
* Snap the gauges to FSPs
* Interpolate DOF for FSPs using snapped gauges
* Map flood depth with interpolated FSP DOFs

### Scripts
* fldpln_header.py – contains all the constants used in both building tiled library and mapping
* fldpln.py --  contains all the functions for flood mapping
* FloodMapping.py – main script to map flood depth
* ???

## Reservoir flood mapping
### Scripts

## Serve Flood Maps on Web

The steps are:
* update gauge table
* schedule jobs
* Build dashboard

### Scripts and batch files
* job_nowcast.bat – batch file to run stream flood mapping with current gauge stage observations
* job_reservoir.bat – batch file to run reservoir flood mapping