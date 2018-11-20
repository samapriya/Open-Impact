# Getting for a Google Earth Engine Account

If you don’t have a developer account [sign up for one here](https://signup.earthengine.google.com/) and make sure you follow the [instructions](https://developers.google.com/earth-engine/python_install_manual) to install the python CLI. The API and the CLI gets updated frequently and as does the install process as needed so you can read the latest instructions at the page.

For the simplest users getting images into GEE begins with the Image upload tool located inside GEE. Once you have added the filename you can edit additional metadata such as start time, cloud cover information if you have that from the metadata file among other things. This tool does not have a way for you to ingest any metadata automatically so it has to be fed manually.

The image name is automatically filled in with the filename that you select when uploading.

![Image Upload](https://i.imgur.com/UheUme1.gif)

Note you cannot select more than one image and upload as a single image if they overlap each other. To handle which we have the concept of image collections. Where you can upload many images. To import images into collections, you have to either import them manually as images first and then copy them into the collection one by one or for now use an external tool to help.

For now you can use the tool I made to batch upload collections along with their metadata into Google Earth Engine. You can read about the tool, it's setup and it's operation at [this Planet Story](https://medium.com/planet-stories/planet-people-and-pixels-a-data-pipeline-to-link-planet-api-to-google-earth-engine-1166606445a8)

Incase you have a Google Cloud Storage bucket you can also push images automatically to be ingested into GEE. Though this requires interaction with gsutil and starting ingestion function for each image. The GEE guide for image ingestion can be [found here](https://developers.google.com/earth-engine/image_upload)

