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


# Python Environments

## Basics of Python environments

Brief introduction on Python virtual environment and options to manage the environment. The last 3 options don't come with standard Python installation and have to be installed separately.
* [Python venv module](https://realpython.com/python-virtual-environments-a-primer/), preinstalled with Python starting from version 3.3
* Virtualenv, a superset of venv module
* Conda, a Python package manager
* Pip, a Python package manager

[Pip vs Conda: an in-depth comparison of Python's two package management systems](https://pythonspeed.com/articles/conda-vs-pip/).

[A way, which combines pip and conda, and works](https://towardsdatascience.com/environments-conda-pip-aaaaah-d2503877884c):
* conda for Python and virtual environments
* pip for Python package management inside the virtual environment

[Python, The System Path and how conda and pyenv manipulate it](https://towardsdatascience.com/python-the-system-path-and-how-conda-and-pyenv-manipulate-it-234f8e8bbc3e). A deep dive into what happens when you type ‘python’ and how popular tools manipulate this.

## The "fldpln" Python environment

This is the python environment available on my laptop, office desktop computer and KARS server which has the necessary packages for flood mapping. It has been used for both tiling and mapping scripts and they work fine on all the computers for the open source version of the software.

### Install and Use the FLDPLN Python Package

* Create a Python environment with Python (3.9, 3.10 or 3.11) supported by the MATLAB Runtime (release 2024a)
  * Install necessary packages needed by FLDPLN Python package
  * miniconda
  * Use mamba/conda to install the necessary packages
* Install the fldpln_py Python package
  * The simplest way we can zip the package and make it available for download and install it by running "python setup.py install" or "pip install ." in the package folder.
  * We can also put the package on Github's root folder and install it using "pip install git+https://github.com/opengeos/leafmap.git"
  * Ideally, we should publish the package to PyPI or Anaconda Cloud and install it using pip or conda
* Install the FLDPLN Python package
  * Use pip to install after publish the package to PyPI or Anaconda Cloud
* Install the MATLAB Runtime
  * The MATLAB Runtime installer for Windows automatically sets the library path during installation, but on Linux or macOS you must add the libraries manually. See [here](https://www.mathworks.com/help/compiler_sdk/cxx/mcr-path-settings-for-run-time-deployment.html) for more information.

### Build the "fldpln" environment
Below are the major steps to build the environment:
* Anaconda Individual Edition (with python 3.9 by default) was downloaded and installed on office desktop computer. On laptop, the existing Anaconda installation was used when building the environment. 
  * Based on University of Helsinki’s [Automating GIS-processing 2021 course materials](https://autogis-site.readthedocs.io/en/latest/course-info/installing-miniconda.html), **Miniconda** should be used instead.
* Create the “fldpln” env by running the following command:
    ```
    conda create -n fldpln python=3.7
    ``` 
  Note that although the Anaconda comes with Python 3.9 at the time, we create **an env with python 3.7 as some packages may not be compatible with the latest python version**.
* Install geopandas package using the following command:
    ```
    conda install -c conda-forge geopandas
    ```
  which installs many dependent general and geospatial packages including numpy, pandas, scipy, gdal, shapely etc.
* Install “h5py” (for reading .mat file), “rasterio”, “openpyxl” (for reading Excel xls file) and “fastparquet” (for reading parquet file, i.e., .snz file) using the following command:
    ```
    conda install -c conda-forge h5py rasterio openpyxl fastparquet
    ```
  After installing “rasterio”, the env couldn’t import geopandas and had some issue with the rtree library, which was resolved by downgrading rtree version from 0.9.7 to 0.9.3 using the following command:
    ```
    conda install -c conda-forge rtree=0.9.3
    ```
* It’s interest that the GDAL versions installed on office desktop (3.0.2) and laptop (3.4.0) are different! On laptop, I cannot use “import gdal” but have to use “from osgeo import gdal” even though I can see gdal listed! On desktop, I can import either way. As explained [here](https://opensourceoptions.com/blog/how-to-install-gdal-with-anaconda/), **The GDAL distribution from conda-forge installs GDAL as parts of the OSGEO package, which includes GDAL, OGR, and OSR**.

### Replicate the python environment using YAML file

Once the pyhton environment is built, it is possible to export the environment as a YAML configuration file and replicate the environment. Below are the major steps to do this:
* Export the fldpln env to a .yaml (or .yml) configuration file using the following command:
    ```
    conda env export -n fldpln > fldpln.yaml
    ```
    or using the following command if you are already in the fldpln env:
    ```
    conda env export > fldpln.yaml
    ```
* Install Python and conda with Miniconda. 
  * Go to [Miniconda download page](https://docs.conda.io/en/latest/miniconda.html#windows-installers). Download **Python 3.7 Miniconda3 Windows 64-bit** instead of the latest Python 3.9 as some of the packages in fldpln env may not be compatible with Python 3.9.
  * Add miniconda to system path so that you can use conda in command line window
  * Another option is to use the conda installation comes with ArcGIS Pro to replicate the env.
    *	Go to ArcGIS Pro from Start
    * Select Python Command Prompt under ArcGIS Pro
* Download the fldpln.yaml configuration file
* In a command window, navigate to the folder where the .yaml file is saved, and run the following conda command to create the fldpln env using the file:
    ```
    conda env create -f fldpln.yaml
    ```
Solving the environment and installing all the packages might take a surprisingly long time, so be patient. After installing all the package, you can activate the env using the following command:
  ```
  conda activate fldpln
  ```

### Use ‘fldpln’ env in VSC

* Add the conda command to system’s PATH environment variable so that VSC can use the conda to activate a specific python env.
  * When install Anaconda, the default installation setting doesn’t add its executable path (on my machine “C:\Users\lixi\Anaconda3\”) to the PATH environment variable.
  * This is done by using the Advanced System Settings in Control Panel (see the screenshot below)
    ![Setup PATH variable](./images/PATH_environment_variables.png)
* VSC terminal 
  * VSC uses powershell as the default terminal which cause some issues even after including conda path in the PATH environment variable.
  * A quick solution is to change the default powershell terminal in VSC to the regular cmd terminal by press CTRL+SHIFT+P in VSC and search for “terminal select default profile” and select “Command Prompt C:\WINDOWS\System32\cmd.exe”
* When using VSC to run Jupyter notebooks, VSC automatically installed ipykernel into the environment.

### Use ‘fldpln’ env in JupyterLab

We can also use the python env in JupyterLab with Jupyter notebooks. We need to install jupyterlab package into the env using the following command:
  ```
  conda install -c conda-forge jupyterlab
  ```
After installation, type in “jupyter lab” in the command widow to start JupyterLab in a web browser. Note the space between the words. In addition, It’s a good idea to first navigate to the folder where your Jupyter notebooks are located before launching Jupyter Lab. You may also want to install the Git extension for JupyterLab for version control:
  ```
  conda install -c conda-forge jupyterlab-git
  ```

## Other Python Environments

In addition to the fldpln env, I have a few other python environments on my laptop. The following are the environments.

### The “arcgidpro-py3” environment

This is the default env for ArcGIS Pro and should/can not be changed in anyway according to ESRI. This env has the scipy.io and 5py libraries which are needed to read and write various versions of .mat files. The arcpy version of the code uses this env where a user can run the mapping notebook in ArcGIS Pro. This env (and arcpy version code), however, can only use mat-based tiles whose file size is only 60% of snappy-based tiles but runs 30% slower than sanppy-based files (i.e., snz).

conda does not show the env when listing available envs. But it can be activated by providing the path(in quotation marks because of the space in “Program Files”)to the python.exe as
```
conda activate "C:\Program Files\ArcGIS\Pro\envs\arcgispro-py3"
```
VSC can use the env by providing the path of C:\Program Files\ArcGIS\Pro\bin\Python\envs\arcgispro-py3\python.exe without problem!

The env works fine for the mapping scripts in the open source version of the code with mat-based tiles. But it won’t work with snappy-based tiles. Also it doesn’t work for the tiling scripts in the open source version of the code as it doesn’t have the geopandas package.

### The “arcgisrpo-py3-clone” environment

This env is only available on my laptop, and the original goal is to expend the arcgispro-py3 env which comes with ArcGIS Pro so that the env can be used for both the arcpy and open source version of the scripts. As we cannot make changes on the arcgispro-py3 env, a copy of the “arcgispro-py3” env was cloned using ArcGIS Pro’s python env manager.  

when I tried to install the “fastparquet” package (version 0.7.2), which is needed to save tiles in snappy (i.e., snz) format, use [ArcGIS Pro Python package manager](https://pro.arcgis.com/en/pro-app/latest/arcpy/get-started/work-with-python-environments.htm), several issues were encountered. 
* First, the “pyarrow” library comes with ArcGIS Pro is NOT usable (version issue) for saving and reading parquet files. It FAILED to install the “fastparquet” package for unknown reason in the cloned ArcGIS Pro Python environment.
* [This ESRI support page](https://support.esri.com/en/technical-article/000020560) covers on how to manage the Python environment in a cloned ArcGIS Pro Python environment. However, I was unable to activate the cloned environment through the Python Command Prompt under ArcGIS. When I tried to activate the cloned environment, I got the following error:
  ```
  Error: Your shell has not been properly configured to use 'conda activate'
  ```
  This may be because that I have two conda installed on my system, one from ESRI and the other from Anaconda. So I used my Anaconda Prompt (command window) to install fastparquet in the cloned ArcGIS Pro Python environment. Note that the cloned ArcGIS environment is visible but does not have a name as is was created by the conda from ArcGIS. We have to activate it by its path instead using the following commands:
    ```
    conda activate C:\Users\lixi\AppData\Local\ESRI\conda\envs\arcgispro-py3-clone
    conda install fastparquet
    ```
  This way, the env just worked fine with both .gzip and snappy format. I don’t need to install python-snappy.

To make the env work for open source based tiling scripts we need to install geopandas. After installing geopandas, geopandas had issues with the rtree library, which was resolved by downgrading rtree version from 0.9.7 to 0.9.3 (using conda install rtree=0.9.3). Then there is a problem with fiona which somehow cannot use the gdal library which was installed from channel “[arcgispro]  esri”. After updated fiona from 1.8.4 to 1.8.18 using “conda update Fiona”, geopandas finally works!

To make the env work for open source based mapping scripts, we need to install rasterio. The installation went smoothly but when running the mapping scripts, I got the following messages:
```
Can't find requested entry point: GDALRegisterMe
Can't find requested entry point: GDALRegister_nitf
Can't find requested entry point: GDALRegisterMe
Can't find requested entry point: GDALRegister_nitf
```
I suspect that this is related to the gdal library which comes from ESRI channel “[arcgispro]  esri”. The scripts still ran and the results seem correct though

With the above experience and experiment, it seems unrealistic to build single python environment for both arcpy and open source versions by expending. The primary problem, I believe, is that **some of the python packages in arcgispro-py3 are from the ESRI channel “[arcgispro]  esri” (i.e., customized packages) which may not be compatible with the packages from other standard channels!**  

### The “geo3” environment:

This environment was created in the fall of 2020 when teaching GEOG560. It works fine for open-source based mapping scripts but not for tiling library. It had problem when reading .xlsx Excel files. Installing the xlrd library still didn’t work. It worked after installing openpyxl and specifying it as the engine in pd.read_excel(). It also had problem to re-order FSP dataframe columns (fspDf = fspDf[fspIdxColumnNames]). This env is not further improved and expended. Instead, a new env “fldpln” was created for the open-source based version.
