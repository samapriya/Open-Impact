# Image Clasification
Guess what we have high resolution imagery at about [70-80 cm Panchromatic and 1m Mulispectral](https://assets.planet.com/marketing/PDF/SkySat_Imagery_Product_Specification.pdf) and when Google had acquired Terra Bella(now known as Skysat) they made couple of image collections open source. So you can perform supervised image classification to test out some of the more advanced feature and then match this with field data. The example imagery is over Port of Lázaro Cárdenas and the three types of classification namely, random forest, cart and svm type clasifications are performed.

<center>![ndvi](/images/skysat_class.gif)</center>

You can add the collection using the line below or type Skysat in the Image Catalog
```js
var skysat=ee.ImageCollection('SKYSAT/GEN-A/PUBLIC/ORTHO/MULTISPECTRAL')
```

You can get link to the overall [area code here](https://code.earthengine.google.com/8002be1166f4b814a90bfa8af3b18372)

To get to our specific area
```js
//Add the collection
var imageCollection = ee.ImageCollection("SKYSAT/GEN-A/PUBLIC/ORTHO/MULTISPECTRAL")

//Add a point geometry to use as filter & print size
var geometry = ee.Geometry.Point([-102.17748642229708, 17.942153910452422])
print(imageCollection.filterBounds(geometry).size())

//Add visualization
var vis = {"opacity":1,"bands":["N","G","R"],"min":132.7061786684951,"max":3655.0445459691864,"gamma":1};
Map.addLayer(imageCollection.median(),vis,'Skysat Median')

Map.setCenter(-102.16941833496094,17.95048268223829,14)
Map.setOptions('SATELLITE')
```
