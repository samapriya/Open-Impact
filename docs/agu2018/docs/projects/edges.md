# Edge Detection

You can also run powerful functions such as edge detection using Hough and Canny transforms for example for single images as well as on collections to do edge counts, connectivity measures among a few other applications.

<center>![Edges](/images/edge_detection.gif)</center>

Similar to earlier example you can access the [full script here](https://code.earthengine.google.com/56fc8a053dd328c14166fc88fed8e8da) or copy and past the same code into [code.earthengine.google.com](https://code.earthengine.google.com)

``` js
//Add an AOI
var aoi=ee.FeatureCollection("projects/sat-io/terra2018/vector/aoi")

//Zoom to AOI
Map.centerObject(aoi,15)

//Add image and visualization
var image=ee.Image("projects/sat-io/terra2018/ps4bsr/20180115_175153_0e14_3B_AnalyticMS_SR")
var vis = {"opacity":1,"bands":["b4","b3","b2"],"min":-433.8386769876429,"max":2822.7077530529555,"gamma":1};

var ndvi = image.normalizedDifference(['b4', 'b3']);

// Apply a Canny edge detector.
var canny = ee.Algorithms.CannyEdgeDetector({
  image: ndvi,
  threshold: 0.2
}).multiply(255);

// Apply the Hough transform.
var h = ee.Algorithms.HoughTransform({
  image: canny,
  gridSize: 256,
  inputThreshold: 80,
  lineThreshold: 80
});

// Display.
Map.addLayer(image, vis, 'source_image');
Map.addLayer(ndvi,{min: -0.05, max: 0.5}, 'NDVI',false)
Map.addLayer(canny.updateMask(canny), {min: 0, max: 1, palette: 'blue'}, 'canny');
Map.addLayer(h.updateMask(h), {min: 0, max: 1, palette: 'red'}, 'hough');
Map.setOptions('SATELLITE')
```
