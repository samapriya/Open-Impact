# Reducers & Charts in Earth Engine

While single image operations are direct and can be applied to the imagery for a one to one result. Applying any kind of analysis on a single image means you have to call the image and run the analysis which is quick and easy. To be able to turn this same into a function that you can iterate over an entire collection requires us to convert a single analysis to a function. A function is then mapped or run over an entire collection. To avoid any errors make sure that the collection images are consistent and have same name and number of band and characteristics.

<center>![Imagery](/images/ee_reduce.png)</center>
<center>**Source: [Image Collection Reductions from Google Earth Engine](https://developers.google.com/earth-engine/reducers_image_collection)**</center>

For this setup we look at how we added PlanetScope Surface Reflectance data earlier , filtering it using date and Cloud cover for the scene. The next step we are building an function to calculate Normalized Difference Vegetation Index (NDVI) over the entire collection which takes an image collection and iteratively passes an image to the function. The resultant structure is also an image collection where we are returning a single band which is the NDVI. Note that we also renamed the band to NDVI since they are not autorenamed. Now the additional step we added here was running the produced collection through a median reducer. We can then print the median values.

<center>![ndvi](/images/ee_ndvi.gif)</center>

You can access the [code here](https://code.earthengine.google.com/454c8a49ea2f99ccd5102a833eb0e733) or copy and paste from below

``` js
//Add an AOI
var aoi=ee.FeatureCollection("projects/sat-io/terra2018/vector/aoi")

//Zoom to AOI
Map.centerObject(aoi,15)

//Add an image collection
var collection=ee.ImageCollection('projects/sat-io/terra2018/ps4bsr')

//Filtering an Image Collection
var filtered=collection.filterDate('2018-01-01','2018-07-01')//Filter for March
.filterMetadata('cloud_cover','less_than',0.2)//Cloud cover less than 20%

//print filtered collection properties
print("Filtered Collection",filtered)


/*==================================================*/
//Writing a function
var addNDVI = function(image) {
  var ndvi = image.normalizedDifference(['b4', 'b3']).rename('NDVI');
  return ndvi;
};

var ndvicoll=filtered.map(addNDVI)
print("NDVI Collection",ndvicoll)

//Let's add a palette for us to show the results
var ndvivis = {"opacity":1,"bands":["NDVI"],"min":-0.259,"max":0.57,"palette":["d49e8a","ffcfb4","ecffcf","94ff8d","3eff56","15cc23"]};

//Add the first of the result
Map.addLayer(ee.Image(ndvicoll.first()).clip(aoi),ndvivis,"NDVI")

//Add an reducer
var ndviav=ndvicoll.reduce(ee.Reducer.max());

Map.addLayer(ndviav.select('NDVI_max').rename('NDVI').clip(aoi),ndvivis,"NDVI Max")
print(ndviav)
```

