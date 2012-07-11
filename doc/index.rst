=====================================================
Welcome to the Geocoding demo application for PyBossa
=====================================================

PyBossa is an open source platform for crowd-sourcing online (volunteer)
assistance to perform tasks that require human cognition, knowledge or
intelligence (e.g. image classification, transcription, information location
etc). 

Geocoding is a demo application that asks volunteers to locate in a map an urban park for a city.

This demo application creates several tasks using a list of files (check the
cities file) and then loads a map using OpenLayers where the volunteers will be
able to place a marker, indicating the position of the urban park for the city.

The results are stored in the PyBossa instance using the GeoJSON format, but it
can be used any of the supported formats by OpenLayers: KML, GeoRsss, etc.

The goals of this application are to show how you can create your own geocoding
applications using PyBossa.

User Guide
==========

This section covers a demo application about how to use PyBossa from the perspective of a PyBossa
end-user such as:

  * Creators and managers of a specific PyBossa microtasking application:
    Geocoding locations in a map.
    
We suggest starting by taking a quick look at the overview of the PyBossa
system as this will introduce you to a few pieces of terminology and help you
understand how things fit together.

Useful Links
------------

* Mailing list: http://lists.okfn.org/mailman/listinfo/open-science-dev
* Source code: https://github.com/citizen-cyberscience-centre/pybossa
* User stories: http://science.okfnpad.org/pybossa-userstories
* General etherpad: http://science.okfnpad.org/pybossa

.. toctree::
   :maxdepth: 2
   
   quickstart

.. toctree::
   :maxdepth: 2

   geocoding
   advance

Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`

