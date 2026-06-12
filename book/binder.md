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


# Binder and BinderHub

Binder or BinderHub makes code immediately reproducible by anyone, anywhere.

## How Binder works

* First to make the mapping scripts and notebooks public on github.
* Fetching your repo from GitHub
* Analyzing the contents
* Creating a Docker file based on your repo
* Launching that Docker image in the Cloud
* Connecting you to it via your browser

## BinderHub

* BinderHub is the primary technology behind Binder (i.e., myginder.org)
* Pangeo’s BinderHub is a BinderHub built by Pangeo which allows users to **perform scalable computations using Dask Gateway**. 
  * The Pangeo BinderHub deployed at binder.pangeo.io seems not working when trying Pangeo gallery applications.
  * The Pangeo BinderHub deployed at https://hub.aws-uswest2-binder.pangeo.io seems working.
* Can we use Pangeo’s BinderHub or can we build a BinderHub on Microsoft Planetary Computer to run flood mapping?


