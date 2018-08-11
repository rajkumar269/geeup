# geeup: Simple CLI for Earth Engine Uploads

This came of the simple need to handle batch uploads of both image assets to collections but also thanks to the new table feature the possibility of batch uploading shapefiles into a folder. Though a lot of these tools including batch image uploader is part of my other project [geeadd]() which also includes additional features to add to the python CLI, this tool was designed to be minimal so as to allow the user to simply query thier quota, upload images or tables and also to query ongoing tasks and delete assets. I am hoping this tool with a simple objective proves useful to a few users of Google Earth Engine.

## Table of contents
* [Installation](#installation)
* [Getting started](#getting-started)
* [geeup Simple CLI for Earth Engine Uploads](#geeup-simple-cli-for-earth-engine-uploads)
	* [gee Quota](#gee-quota)
    * [gee Zipshape](#gee-zipshape)
    * [gee upload](#gee-upload)
    * [gee table upload](#gee-table-upload)
    * [gee tasks](#gee-tasks)
    * [gee delete](#gee-delete)

## Installation
This assumes that you have native python & pip installed in your system, you can test this by going to the terminal (or windows command prompt) and trying

```python``` and then ```pip list```

If you get no errors and you have python 2.7.14 or higher you should be good to go. Please note that I have tested this only on python 2.7.15 but it should run on python 3.

**This also needs earthengine cli to be [installed and authenticated on your system](https://developers.google.com/earth-engine/python_install_manual) and earthengine to be callable in your command line or terminal**

To install **geeup: Simple CLI for Earth Engine Uploads** you can install using two methods

```pip install geeup```

or you can also try

```
git clone https://github.com/samapriya/geeup.git
cd geeup
python setup.py install
```
For linux use sudo.

Installation is an optional step; the application can be also run directly by executing geeup.py script. The advantage of having it installed is being able to execute ppipe as any command line tool. I recommend installation within virtual environment. If you don't want to install, browse into the geeup folder and try ```python geeup.py``` to get to the same result.


## Getting started

As usual, to print help:

```
usage: geeup [-h] {quota,zipshape,upload,tabup,tasks,delete} ...

Simple Client for Earth Engine Uploads

positional arguments:
  {quota,zipshape,upload,tabup,tasks,delete}
    quota               Print Earth Engine total quota and used quota
    zipshape            Zipped all shapefiles and subsidary files into
                        individual zip files
    upload              Batch Asset Uploader.
    tabup               Batch Table Uploader.
    tasks               Queries current task status
                        [completed,running,ready,failed,cancelled]
    delete              Deletes collection and all items inside. Supports
                        Unix-like wildcards.

optional arguments:
  -h, --help            show this help message and exit
```

To obtain help for a specific functionality, simply call it with _help_ switch, e.g.: `geeup dropinfo -h`. If you didn't install geeup, then you can run it just by going to *geeup* directory and running `python geeup.py [arguments go here]`

## geeup Simple CLI for Earth Engine Uploads
The tool is designed to handle batch uploading of images and tables(shapefiles). While there are image collection where you can batch upload imagery,for vector or shapefiles you have to batch upload them to a folder.

### gee Quota
Just a simple tool to print your earth engine quota quickly.

```
usage: geeup quota [-h]

optional arguments:
  -h, --help  show this help message and exit
```

### gee Zipshape
So here's how table upload in Google Earth Engine works, you can either upload the component files shp, shx, prj and dbf or you can zip these files together and upload it as a single file. The pros for this is that it reduces the overall size of the shapefile after zipping them together, this tool looks for the shp file and finds the subsidary files and zips them ready for upload. It also helps when your have limited upload bandwith. Cons you have to create a replicate structure of the file system, but it saves on bandwidth and auto arranges your files so you don't have to look for each additional file.

```
usage: geeup zipshape [-h] --input INPUT --output OUTPUT

optional arguments:
  -h, --help       show this help message and exit

Required named arguments.:
  --input INPUT    Path to the input directory with all shape files
  --output OUTPUT  Destination folder Full path where shp, shx, prj and dbf
                   files if present in input will be zipped and stored
```

### gee upload
The script creates an Image Collection from GeoTIFFs in your local directory. By default, the collection name is the same as the local directory name; with optional parameter you can provide a different name.

```
usage: geeup upload [-h] --source SOURCE --dest DEST [-m METADATA]
                       [--large] [--nodata NODATA] [--bands BANDS] [-u USER]
                       [-b BUCKET]

optional arguments:
  -h, --help            show this help message and exit

Required named arguments.:
  --source SOURCE       Path to the directory with images for upload.
  --dest DEST           Destination. Full path for upload to Google Earth
                        Engine, e.g. users/pinkiepie/myponycollection
  -u USER, --user USER  Google account name (gmail address).

Optional named arguments:
  -m METADATA, --metadata METADATA
                        Path to CSV with metadata.
  --large               (Advanced) Use multipart upload. Might help if upload
                        of large files is failing on some systems. Might cause
                        other issues.
  --nodata NODATA       The value to burn into the raster as NoData (missing
                        data)
  --bands BANDS         Comma-separated list of names to use for the image
                        bands. Spacesor other special characters are not
                        allowed.
  -b BUCKET, --bucket BUCKET
                        Google Cloud Storage bucket name.

```

### gee table upload
This tool allows you to batch download tables/shapefiles to a folder. It uses a modified version of the image upload and a wrapper around the earthengine upload cli to achieve this while creating folders if they don't exist and reporting on assets and checking on uploads. This only requires a source, destination and your ee authenticated email address.

```
usage: geeup tabup [-h] --source SOURCE --dest DEST [-u USER]

optional arguments:
  -h, --help            show this help message and exit

Required named arguments.:
  --source SOURCE       Path to the directory with zipped folder for upload.
  --dest DEST           Destination. Full path for upload to Google Earth
                        Engine, e.g. users/pinkiepie/myponycollection
  -u USER, --user USER  Google account name (gmail address).
```

### gee tasks
This script counts all currently running,ready,completed,failed and cancelled tasks along with failed tasks. This is linked to the account you initialized with your google earth engine account. This takes no argument.

```
usage: geeup tasks [-h]

optional arguments:
  -h, --help  show this help message and exit
```

### gee delete
The delete is recursive, meaning it will delete also all children assets: images, collections and folders. Use with caution!

```
usage: geeup delete [-h] id

positional arguments:
  id          Full path to asset for deletion. Recursively removes all
              folders, collections and images.

optional arguments:
  -h, --help  show this help message and exit
```
