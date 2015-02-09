PyBossa geo-demo application: Urban Parks in cities
===================================================

This demo application shows how you can use PyBossa for geo-coding problems. 

The application is a simple demo that uses OpenLayers JavaScript library to load a map,
center it and add a point to the map, in order to mark the position of the
urban park. 

Thanks to the use of OpenLayers the data can be exported in any standard GIS format:
KML, GeoJSON, etc. In this application the answers are saved as GeoJSON.

![alt screenshot](http://i.imgur.com/XMVyNKV.png)

The application has three files:

*  createTasks.py: for creating the application in PyBossa
*  template.html: the view for every task and deal with the data of the answers.
*  tutorial.html: a simple tutorial for the volunteers.

Testing the application:
=======================
You need to install the pybossa-client (use a virtualenv):

```bash
    $ pip install -r requirements.txt
```

*  Create an account in PyBossa
*  Copy under your account your API-KEY
*  Run python createTasks.py -u http://crowdcrafting.org -k API-KEY
*  Open with your browser the *Applications section* and choose the Urban Parks 
   application. This will open the presenter for this demo application.


Documentation
=============

We recommend that you read the section: [Build with PyBossa](http://docs.pybossa.com/en/latest/build_with_pybossa.html) and follow the [step by step tutorial](http://docs.pybossa.com/en/latest/user/tutorial.html).

**NOTE**: This application uses the [pybossa-client](https://pypi.python.org/pypi/pybossa-client) in order to simplify the development of the application and its usage. Check the [documentation](http://pythonhosted.org/pybossa-client/).

LICENSE
=======

Please, see the COPYING file.

Acknowledgments
===============
The thumbnail has been created using a [photo](http://www.flickr.com/photos/infomatique/5489548540/) from William Murphy (license CC¬BY-SA 2.0).
