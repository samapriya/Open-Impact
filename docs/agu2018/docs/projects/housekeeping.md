# Housekeeping and Setup

For most users data usage often boils down to the software you use to analyze and manipulate images and how you are going to work with them. So here are going to do some housekeeping and setup depending on which tools and setup you are most comfortable with

#### 1) Python Setup and libraries
Depending on what type of usage you are interested and the libraries you want to use you can find Python installations [here](https://www.python.org/downloads/). And here are some of the libraries you can use with that including but not limited to GDAL, [Scipy](https://www.scipy.org/),[Numpy](http://www.numpy.org/), [Matplotlib](https://matplotlib.org), [Pandas](https://pandas.pydata.org/), [Fiona](https://pypi.python.org/pypi/Fiona) and [Shapely](https://pypi.python.org/pypi/Shapely). This list is not exhaustive and include anything else you might need like [scikit](http://scikit-learn.org/) or learn [opencv](https://pypi.python.org/pypi/opencv-python) All of [Pypi](https://pypi.org/) is your oyster

for most systems you can copy and paste this if you have pip on your native command line or terminal

<center>```pip install numpy scipy fiona shapely matplotlib pandas```</center>

For Ubuntu and Debian users this might work better and I borrowed it from the installation page at [Scipy](https://www.scipy.org/install.html)

<center>```sudo apt-get install python-numpy python-scipy python-matplotlib python-pandas```</center>

Note that GDAL involves a few additional steps for installation on windows machines [You can read more about it here](https://webcache.googleusercontent.com/search?q=cache:UZWc-pnCgwsJ:https://sandbox.idre.ucla.edu/sandbox/tutorials/installing-gdal-for-windows+&cd=3&hl=en&ct=clnk&gl=us)

Two other tools or setups which might be handy include

#### virtualenv
virtualenv allows the user to create and manage seperate package installations fo multiple projects. Think of this as your new project can have its own set of user libraries seperated from the native python libraries. It allows you to create isolated environments for projects and install packages into that virtual isolated environments.

A simple installation would be
<center>```pip install virtualenv```</center>

You can get more details about installation on different operation systems and activation of this environment by [reading their docs](https://packaging.python.org/guides/installing-using-pip-and-virtualenv/).


#### jupyter notebook
The jupyter hub defined jupyter notebook as

```
The Jupyter Notebook is an open-source web application that allows you to create
and share documents that contain live code, equations, visualizations and narrative text.
```

A lot of the tool chains and processes have been already built into jupyter notebooks for you to use,

#### 2) Planet and Google Earth Engine(GEE) Command Line Interface(CLI) Setup
You planet account comes with a brand new CLI and it allows you to perfrom basic functions such as search for ID[s] and for images in a specific location, export all image footprint in your area of interest and so on. Installation is pretty simple

<center>```pip install planet```</center>

You installation steps from earlier means you have managed to not only create the Google Earth Engine account but also installed its client. Incase you have missed it go to their main reference page for installation of their python client. Since you can consume Earth Engine using both Javascript(in browser) and Python(locally).

#### 3) Location to GEE datasets
For the purpose of this workshop, I have downloaded and ingested Planet imagery into three distinct colllections (PlanetScope 4 Band analytic, Planet 4 analytic surface analytic, RapidEye OrthoTile) If the data is maintained as open access license, you can add any of the datasets by simply adding the following lines. For now you access these here

``` js
var planetscope4b = ee.ImageCollection("projects/sat-io/terra2018/ps4b")
var planetscope4b_sr = ee.ImageCollection("projects/sat-io/terra2018/ps4bsr")
var rapideye = ee.ImageCollection("projects/sat-io/terra2018/reo")

//get size of collection
print("PlanetScope Size",planetscope4b.size())
print("PlanetScope SR",planetscope4b_sr.size())
print("RapidEye size",rapideye.size())

//Get the first element from each collection
print("PS Image",planetscope4b.first())
print("PSR Image",planetscope4b_sr.first())
print("REO Image",rapideye.first())
```

for those using python API you can still access a collection

``` py
import ee
ee.Initialize()
planetscope=ee.ImageCollection("projects/sat-io/terra2018/ps4b")

##you can even check how many images does the collection have
print(planetscope.size().getInfo())
```

#### 4) Adding additional Images
For a minute there imagine you want to work with more data apart from the few areas we talked about, the Education and Research account gives you 10,000 square kilometer and you can then upload it into GEE.

For the simplest users getting images into GEE begins with the Image upload tool located inside GEE. Once you have added the filename you can edit additional metadata such as start time, cloud cover information if you have that from the metadata file among other things. This tool does not have a way for you to ingest any metadata automatically so it has to be fed manually.

The image name is automatically filled in with the filename that you select when uploading.

![Image Upload](/images/upload_gee.gif)

Note you cannot select more than one image and upload as a single image if they overlap each other. To handle which we have the concept of image collections. Where you can upload many images. To import images into collections, you have to either import them manually as images first and then copy them into the collection one by one or for now use an external tool to help such as using the Google Earth Engine CLI.

For now you can use the tool I made to [batch upload collections along with their metadata into Google Earth Engine](https://github.com/samapriya/Planet-GEE-Pipeline-CLI). You can install this by simply typing

<center>```pip install ppipe```</center>

You can read about the tool, it's setup and it's operation at [this Planet Story](https://medium.com/planet-stories/planet-people-and-pixels-a-data-pipeline-to-link-planet-api-to-google-earth-engine-1166606445a8)

Incase you have a Google Cloud Storage bucket you can also push images automatically to be ingested into GEE. Though this requires interaction with gsutil and starting ingestion function for each image. The GEE guide for image ingestion can be [found here](https://developers.google.com/earth-engine/image_upload)

#### 5) Additional Tools and Toolboxes for Local Analysis
If you need to handle the data locally using Matlab, QGIS or ArcMap make sure you have these softwares installed. The images can then be downloaded and analyzed using multiple methods and toolsets. A lot of these softwares have additional capabilities to help you further use Planet data. You can find a better reference of external integration here

* [ENVI Integration](https://www.planet.com/pulse/explore-and-analyze-planet-imagery-with-harris-envi)
* [ESRI Integration](https://blogs.esri.com/esri/arcgis/2017/05/10/new-tools-for-managing-planet-imagery)
* [Cesium Integration](https://www.planet.com/pulse/planet-imagery-available-to-cesium-community)
* [Boundless](https://boundlessgeo.com/press_releases/boundless-announces-strategic-partnership-planet-expand-imagery-ecosystem)
* [PCI Geomatics](http://www.pcigeomatics.com/pressnews/2017_PCI_Planet_Ecosystem.pdf)
