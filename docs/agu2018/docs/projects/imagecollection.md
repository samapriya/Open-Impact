# Image Collections in Earth Engine

While single images are great to do quick analytics, the true power of the Earth Engine environment comes with the possibility of looking at really large and heavy image collections and to be able to push analysis towards the data, rather than the need for the data to travel at all. In the GEE environment image collections have their own characteristic setup and are composted with single images that we discussed earlier. They can often have the same or different band structure but generally share a similar metadata structure for filtering and querying.

Large scale image collections such as Landsat and Sentinel image collections are ingested on the fly and are actively maintained till there imagery and processing pipelines feeds are maintained byt he agencies supplying the imagery. Images as well as image collections can be moved into GEE environment to allow you to use both your data and the GEE catalog data within the same platform.

For those who are concerned with access to datasets, this means that though Earth Engine allows an easier way to share datasets across users, private folder, collections and imagery are private and are not here the section from their [Terms and Conditions page](https://earthengine.google.com/terms/)

**```Intellectual Property Rights. Except as expressly set forth in this Agreement,
this Agreement does not grant either party any rights, implied or otherwise,
to the other’s content or any of the other’s intellectual property.
As between the parties, Customer owns all Intellectual Property Rights
in Customer Data, Customer Code, and Application(s), and Google owns
all Intellectual Property Rights in the Services and Software.```**

These image collection as well as individual imaegs again have defined data type,scales and projections along with some default properties such as an index and ID among other system properties. So we can query these properties, print them and add them

``` js
var filtered=ee.ImageCollection("projects/sat-io/terra2018/ps4bsr")
.filterDate('2018-03-01','2018-03-31')//Filter for March
print('Date Filter Only',filtered.size())

var filtered=ee.ImageCollection("projects/sat-io/terra2018/ps4bsr")
.filterDate('2018-03-01','2018-03-31')//Filter for March
.filterMetadata('cloud_cover','less_than',0.1)//Cloud cover less than 10%
print('Multi Filter',filtered.size())
```

To have a look at all of the raster catalog you can find them [listed here](https://code.earthengine.google.com/datasets) or you can try the [list I update every week](https://github.com/samapriya/Earth-Engine-Datasets-List)
