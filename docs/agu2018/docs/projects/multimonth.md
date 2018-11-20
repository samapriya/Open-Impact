# Monthly Cloud Free Composites in Earth Engine

One of the most sought after functions in Earth Engine is the possibility of using deep time stack imagery to create cloud free composites. One of the simplest way of thinking about this is to use reducers that we talked about earlier, where we look at an entire stack of pixels and choose the median value of the distribution of pixel across stack and we end up getting a cloud free composite over the given time period. Depending on the number of images, the actual number of cloud free images in the overall stack your results may need more fine tune adjustments.

<center>![composite](/images/ee_composite.gif)</center>
<center>**Multi Month Composite: A single month is added FCC**</center>

For this setup we look at how we added PlanetScope Surface Reflectance data earlier , filtering it using date and Cloud cover for the scene. The next step we are building an function to calculate monthly composites from January to June 2018. Note that this is another way of creating a function where we have inserted the map function and the collection inside the function so it can be run directly. We might be interested in sorting these collections using month and hence we set the month as a metadata for each image in the cloud free composite.

You can get the [complete code here](https://code.earthengine.google.com/bd8654c0635e9ae2b8f6329b64e70234) or copy and paste it from below

``` js
//Add an AOI
var aoi=ee.FeatureCollection("projects/sat-io/terra2018/vector/aoi")

//Zoom to AOI
Map.centerObject(aoi,15)

//Add an image collection
var collection=ee.ImageCollection('projects/sat-io/terra2018/ps4bsr')

//print collection properties
print("Collection",collection)

//Create Monthly Composite from PlanetScope Surface Reflectance
var months = ee.List.sequence(1, 6)

var multimonth = ee.ImageCollection(months
  .map(function(m) {
    var start = ee.Date.fromYMD(2018, m, 1)
    var end = start.advance(1, 'month');
    var image = collection.filterDate(start, end).median();
    return image.set('month', m)
}))

print(multimonth);

//Add a visualization
var vis = {"opacity":1,"bands":["b4","b3","b2"],"min":-433.8386769876429,"max":2822.7077530529555,"gamma":1};

//Add the Image
Map.addLayer(ee.Image(ee.ImageCollection(multimonth).first()).clip(aoi),vis,ee.String('2018-').cat(ee.String(ee.Image(ee.ImageCollection(multimonth).first()).get('month'))).slice(0,6).getInfo())

Map.setOptions('SATELLITE')
```
