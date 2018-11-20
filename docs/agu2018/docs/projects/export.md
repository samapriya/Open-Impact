# Export Images in Earth Engine

When we are all said and done we still want to export the images. Google Earth Engine allows you to export images externally into two subsystems, a Google Cloud Storage Bucket(Free quota upto 5 GB) or Google Drive (This is tied to your overall quota). The method we are exploring right now is export to Google Drive, and then being able to import the analyzed image into any local tool or libraries. It is possible to export entire collections to drive using batch exports in the python API client. This avoids the need for you to click on the Run button everytime an export task has to be started.

<center>![export](/images/ee_export.JPG)</center>
<center>**Export Window: Export Image to Google Drive**</center>

For this setup we look at how we added PlanetScope Surface Reflectance data earlier , filtering it using date and using Cloud cover for the scene. The next step we are building an function to calculate monthly composites from Jan till end of June 2018. Note that this is another way of creating a function where we have inserted the map function and the collection inside the function so it can be run directly. We might be interested in sorting these collections using month and hence we set the month as a metadata for each image in the cloud free composite. The last step that is added is the Export to drive function, where we set up the image name, the image type, the scale and the region refers to areas over which we are exporting this imagery.

You can access the entire [code here](https://code.earthengine.google.com/1a994add2330a7587dd88d429b71571a) or copy and paste from below

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

//Export Imagery
Export.image.toDrive({
  image:ee.Image(ee.ImageCollection(multimonth).first())
  .select(['b1','b2','b3','b4']).toUint16(),
  description: "Median-Image-Export",
  folder: 'EE-Terra-Test',
  scale:3,
  region: aoi,
  maxPixels:10e12
})
```

Exporting video seems like the obvious thing to do after this. For this a couple of things to keep in mind, you can only export 3 band videos cast to a 8 bit image per frame. You can sort these images before export and that is the ordering of the frames.

<center>![export](/images/ee_video.gif)</center>

Complete link with export can be [found here](https://code.earthengine.google.com/54d76a7ee276e1b08ff66b91c44d4b06) or add to existing code

``` js
// Load and format the collection to export.
var frame = ee.ImageCollection(multimonth)
  .sort('year')
  .select(['b4', 'b3', 'b2'])
  .map(function(image) {
    return image.visualize({bands: ['b4', 'b3', 'b2'], "min":-433.8386769876429,"max":2822.7077530529555})
    .set('month',image.get('month'));
  });

// Export video
Export.video.toDrive({
  collection: frame,
  folder: 'EE-Terra-Test',
  description: 'EE-Terra-Video',
  dimensions: 1080,
  framesPerSecond: 1,
  region: aoi
});
```
