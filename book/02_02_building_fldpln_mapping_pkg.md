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


# Build the fldpln Package

The fldpln Python package was built using the [Cookiecutter PyPackage](https://github.com/opengeos/cookiecutter-pypackage) and it's related [YouTube video](https://www.youtube.com/watch?v=Z2d1Kw1xSVY), both significantly reduced the time to create a professional Python package on Guthub and publish it on PyPI.

## Install Package Development Tools

Instead of every time pushing the package onto GitHub to build it, we can install the necessary tools on local computer and build the package locally. To use those tools, we need first to install them which are specified in the requirements_dev.txt.
```
pip install -r requirements_dev.txt
```
This will install the same tools that GitHub runs automatically after the changes are pushed to it. For example, instead of pushing it to GitHub and build it over there, we can open a commandline window, activate the Python environment, navigate to the project folder, and use the mkdoc tool locally to build the package web site locally by running the following command.
```
mkcdocs build
```
We can also check the spelling using the codespell tool:
```
codespell --skip="*.csv,*.geojson,*.json,*.js,*.html,*cff,./.git" --ignore-words-list="aci,hist,gage"
```

## Create the Project

## Setup the Workflow/Pipeline

Here we setup the workflows that the package needs to go through whenever we make changes or update it.

## Update and Make Changes on the Project

### Commit and synchronize changes

After making changes on the package, we can update the package to include those changes on GitHub and PyPI.
* In VSC, click on Source Control tab, type some description text about the changes made and hit Ctrl + Enter or the Commit button to commit the update
* Sychronize the changes to Github. This will activate the pipeline/workflow on GitHub (see yaml files under .github/workflows) which tests the package in several OSs (Windows, MacOS, and Linux/ubuntu) and this will take a while to finish.

### Manage version

After making sure the updating workflow on the package run successfully, we can update the version for the package if wanted. First we can do a dry run, which does nothing real but just shows the changes will be made. We need to open a Anaconda Prompt window, navigate to the fldpln package folder (for example, E:\fldpln\fldpln), and run the following command in Windows commandline window:
```
bump-my-version bump patch --dry-run --verbose
```
After making sure updates on the package are good, we will actually bump the version. You can check the version change by open __init__.py in VSC.
```
bump-my-version bump patch --verbose
```
Note that the version bump was done only locally after running the above command successfully. We still need to go back to VSC and click on the Synchronize button to synchronize this on GitHub, which will take a while.

### Push tags and changes to GitHub

After bumping the version, we can push the tags and changes to GitHub by running the following two commands:
```
git push --tags
git push
```

### Make a New Release on GitHub

Now we can make a new release of the package. On GitHub (on the right side) click Release and then on the release page, click on the Tags button at the top. On the tags page, the top release is the suggested new release tag. Click the "..." on the right side of the release and choose Create release.

Put a release title, typically is the new release tag such as v0.0.15, and add some description text in the Release notes box, and the click on the Publish release button at the bottom of the page.

When this is done, it will publish the package on PyPI automatically.

## Issues with importing library/package and requirements.txt

Tips:

* import the libraries in individual functions if they cannot be installed using pip.

## What's next?

•	How to hide __new__() from API reference?

