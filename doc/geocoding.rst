===================================
Application Tutorial: Urban Parks
===================================

This tutorial is based in the demo application **Urban Parks** provided with
PyBossa. This demo application is a simple microtasking application where users have to
place a marker indicating the location of one urban park for a given city.

The demo application has two main components:

  * **Task Creator**: Python script to generate the tasks in PyBossa, and the
  * **Task Presenter**: HTML + Javascript to show the tasks to the users.

Both items use the PyBossa API.

Setting Things Up
=================

In order to run the tutorial, you will need to create an account in a PyBossa. 
The PyBossa server could be running in your computer or in a third party
server.

.. note:

   You can use http://pybossa.com for testing. 

When you create the account, you will have to access your profile, and copy the
**API-KEY** that has been generated for you. This **API-KEY** allows you to create the
application in PyBossa (only authenticated users can create applications and
tasks, while everyone can collaborate solving the tasks).

Creating the Application
========================

There two possible ways for creating an application:

  * Using the **web interface**: the top menu bar has a section called
    Applications, that will have an option for creating an application.
  * Using the **RestFUL API**: you can check the source code of the
    *createTasks.py* script for more details about creating an application using
    the API.

For this tutorial we are going to use the second option, the **RestFUL API** via
the *createTasks.py* script. The script will require a URL and an **API-KEY** to
authenticate you in the PyBossa server. The following section gives more
details about how to use the script.


Creating the Tasks and Application
==================================

The *createTasks.py* script has a full example about how it is possible to create
an application, and the associated tasks for the application. PyBossa will deliver the
tasks for the users (authenticated and anonymous) and store the submitted
results in the PyBossa data base.

The script gets a list of cities from a file named **cities** (you can see
the content of the file with any text editor) and creates one task per city. As
the file has 246 cities, there will be 246 Tasks in PyBossa.

In PyBossa the task information is stored in an **info** field of a JSON
object::

  {'info' : { 
             'city': 'Madrid Spain' 
            },
   [..]
  }

As you can see, the script only reads the city name, which is a line of the
file lines, and creates the JSON object info with only one field: **'city'**
populated with one line of the file **cities**.

In order to create the application and its tasks, run the following script::

  $ python createTasks.py -u http://PYBOSSA-SERVER -k API-KEY -c cities

.. note::
    You can create another list of cities and save them in a file and then pass
    it to the createTasks.py script. Use the command line argument -c or
    --cities. You can get more information about the script running the command
    line argument --help or -h.

Here is a list of all the Python methods that use the PyBossa API:

.. automodule:: createTasks 
   :members: create_app, update_app, delete_app

Providing more details about the application
============================================

If you check the source code, you will see that there is a file named
*long_description.html*. This file has a long description of the application,
explaining different aspects of it.

This information is not mandatory, however it will be very useful for the users
as they will get a bit more of information about the application goals.

The file can be composed using HTML or plain text. As PyBossa is using `Twitter
Bootstrap <http://twitter.github.com/bootstrap/>`_ you can use all the available 
CSS properties that this framework provides.

The long description is shown in the application home page::

 http://pybossa.com/app/urbanpark

If you want to modify the description you have two options:

 * Edit it via the web interface, or
 * modify locally the *long_description.html* file and run the command again
   with the **-u** option to update it.


Presenting the Tasks to the user
================================

In order to present the tasks to the user, you have to create an HTML template.
The template has only the skeleton for loading the tasks data (the map) and the 
questions and answers that users can provide for the given
task.

In this tutorial, Urban Parks uses a basic HTML skeleton and the `PyBossa.JS <https://github.com/PyBossa/pybossa.js>`_ library to load the data of the tasks 
into the HTML template, and take actions based on the users's answers.

.. note::
  When a task is submitted by an authenticated user, the task will save his
  user_id. For anonymous users the submitted task will only have the user IP
  address.

1. The HTML Skeleton
--------------------

The file_ **template.html** has the skeleton to show the tasks. The file has
three sections or <div>:

  * **<div> for the warnings actions**. When the user saves an answer, a success
    feedback message is shown to the user. There is also an error one for
    the failures.
  * **<div> for the map**. This div will be populated with the OpenLayers map.
  * **<div> for the Questions & Answer buttons**. There are two buttons: *Save
    these coordinates for the urban park* or *Try another city*.

At the end of the skeleton we load the Javascript: 

 * the PyBossa.JS library: <script src="/static/js/pybossa/pybossa.js" type="text/javascript"></script>
 * and the script to load the data, request new tasks, etc.: <script></script>

.. _file: https://github.com/PyBossa/app-geocoding/blob/master/app-geocoding/template.html

This template file will be used by the **createTasks.py** script to send the
template as part of the JSON object that will create the tasks. In PyBossa
every application has a **presenter** endpoint:

 * http://PYBOSSA-SERVER/app/SLUG/presenter

.. note::
   The **slug** is the short name for the application, in this case **urbanpark**. 

The presenter will load the skeleton and JavaScript for the application task.
The header and footer will be provided by PyBossa, so the template only has to
define the structure to load the data from the tasks and the action buttons to
retrieve and save the answer from the volunteers.

2. Adding an icon to the application
------------------------------------

It is possible also to add a nice icon for the application. By default PyBossa
will render a 100x100 pixels empty thumbnail for those applications that do not
provide it. If you want to add an icon you only have to upload the thumbnail of
size 100x100 pixels to a hosting service like Flickr, ImageShack, etc. and use
the URL image link to include it in the **info** field (check createTask.py
script as it has an example)::

  info = { 'thumbnail': http://hosting-service/thumbnail-name.png,
           'task_presenter': template.html file
         }

3. Updating the template for all the tasks
------------------------------------------


It is possible to update the template of the application without
having to re-create the application and its tasks. In order to update the
template, you only have to modify the file template.html and run the following
command::

  python createTasks.py -u http://PYBOSSA-SERVER -k API-KEY -t

This is the used method to update the template:

.. automodule:: createTasks 
   :members: update_template

4. Loading the Task data
------------------------

All the action takes place in the file_ **template.html** script section, after the pybossa.js library.

The script is very simple, it uses the PyBossa.JS library to get a new task and
to submit and save the answer in the server.

PyBossa.JS provides a method to get the data for a task that needs to be solved
by the volunteer:

  * pybossa.newTask( applicationName )

In this case, applicationName will be "urbanpark". The library will get
a task for the application and return a JSON object with the following format::

  { question: application.description,
    task: { 
            id: value,
            ...,
            info: { 
                    city: 
                  } 
          } 
  }

Therefore, if we want to load the data into the skeleton, we will only have to
do something like this::

  $("#question h1").text(data.question);
  $("#task-id").text(data.task.id);
  $("#photo-link").attr("href", data.task.info.link);
  $("#photo").attr("src",data.task.info.url);

and wrap it in the pybossa.newTask method::

  pybossa.newTask( "urbanpark").done(
    function( data ) {
      $("#question h1").text(data.question);
      $("#question h3").attr("href", data.task.info.city);
      $("#task-id").text(data.task.id);

      addCity(data.task.info.city);
    };
  );

As you can see, with every task the task data is loaded from the grabbed task,
and as this application tries to locate an urban park for a city, there is
a function called: **addCity** that will geocode the name of the city into its
coordinates (lon, lat) and center the map in this position. The function also
places a marker for the city, indicating its position in the map.

Every time that we want to load a new task, we will have to call the above
function, so it will be better if we create a specific function for this
purpose (check the **loadData** function in the script).

Once the data have been loaded, it is time to bind the action buttons to
actions that will save the answer from the user.

2.1 Loading the map
-------------------

This application uses the `OpenLayers <http://openlayers.org/>`_ library to load the map. This library
provides lots of features that can be very helpful to work with maps: layers, markers, drag & drop actions, etc. The community is very active and there a lots of examples about how to use this library or other plugins.

.. note:

   PyBossa can use any mapping library. This demo application uses OpenLayers
   because is open source and very powerful.

The script section for this application needs a function to set up the map, or
in other words to load the OpenLayers map::

    map = new OpenLayers.Map('map_canvas');

Then, we can add all the layers from external sources that we want to use. In
this example we are going to use as the default mapping layer, the Open Street
Map servers, and then Google Maps Satellite imagery and they Physical layer::

    // Layers
    // Open Street Map (default layer)
    map.addLayer(new OpenLayers.Layer.OSM("Open Street Map"));

    // Google Maps Satellite layer
    map.addLayer(new OpenLayers.Layer.Google(
        "Google Satellite",
        {type: google.maps.MapTypeId.SATELLITE}
    ));

    // Google Maps Physical layer
    map.addLayer(new OpenLayers.Layer.Google(
        "Google Physical",
        {type: google.maps.MapTypeId.TERRAIN}
    ));

Now we can set up two layers for loading the city and the urban park markers.
Using two different layers is interesting as we will be able to hide and show
them, as well as add specific feature to each of them. For example, the urban
marker should be added to a layer where drag & drop events can occur, as the
volunteer may want to drag and drop the maker once he placed the marker.

In the script, we create the layers and associated markers like this::

    // Icon for the City Marker
    var styleMapCity = new OpenLayers.StyleMap({
        pointRadius: 15,
        externalGraphic: 'http://dl.dropbox.com/u/27667029/mapicons/you-are-here-2.png'
    });

    // Icon for the Urban Park Marker 
    var styleMapUrbanPark = new OpenLayers.StyleMap({
        pointRadius: 15,
        externalGraphic: 'http://dl.dropbox.com/u/27667029/mapicons/urbanpark.png'
    });

    // Layer for placing the city marker
    cityLayer = new OpenLayers.Layer.Vector("City marker", {
        styleMap: styleMapCity,
        attribution: 'Marker Icons by <a href="http://mapicons.nicolasmollet.com/">Nicolas Mollet</a>'
    });
    map.addLayer(cityLayer);

    // Layer for placing the urban park marker
    urbanParkLayer = new OpenLayers.Layer.Vector("Urban park marker", {
        styleMap: styleMapUrbanPark,
        attribution: 'Marker Icons by <a href="http://mapicons.nicolasmollet.com/">Nicolas Mollet</a>'
    });
    map.addLayer(urbanParkLayer);

As you can see, the markers will have their own customized icon. This is very
helpful as you will be able to use your own icon sets for your application.

Then, we need to add a set of controls that will allow the user to interact
with map (editing toolbar) as well as get information from the map (i.e. cursor position in Lon & Lat
coordinates and/or layer switcher)::

    // Controls: Layer switcher and Mouse Position
    map.addControl(new OpenLayers.Control.LayerSwitcher());
    map.addControl(new OpenLayers.Control.MousePosition({displayProjection: new OpenLayers.Projection("EPSG:4326")}));
    // This panel adds the control to add points into the map. The event featureAdded will trigger the disablePoint
    // function, that will disable the toolbar, so only one urban park marker can be added per city and per volunteer
    var panelControls = [
        new OpenLayers.Control.DrawFeature(urbanParkLayer,
            OpenLayers.Handler.Point,
            { 'displayClass': 'olControlDrawFeaturePoint',
              'featureAdded': disablePoint 
            })
    ];

    // Load the Editing Toolbar but only with the points tool
    toolbar = new OpenLayers.Control.Panel({
        displayClass: 'olControlEditingToolbar'
    });

    // Add all the previous controls to the map
    toolbar.addControls(panelControls);
    map.addControl(toolbar);

From the above code it is important to note that when a user add an urban park
marker, the **disablePoint** function will be called::

    // Function to allow only the addition of one urban park per city
    // The function gets the feature (point) and gets its location, transforms it to the right projection
    // loads the lon and at into the HTML skeleton and disables the toolbar, so no more points can be added
    disablePoint = function(feature) { 
        $("#lat").text(feature.geometry.y);
        var tmp = feature.geometry.clone();
        tmp.transform(
                map.getProjectionObject(), // from Spherical Mercator Projection
                new OpenLayers.Projection("EPSG:4326") // to transform from WGS 1984
        );
        $("#lon").text(tmp.x);       
        $("#lat").text(tmp.y);       
        toolbar.deactivate();
    }

This function will only allow to add one marker per action. This constraint has
been used to show how it is possible to interact with the toolbar actions in
the user interface of the map. The function will be triggered when the user
places the marker in the map, and then the toolbar will be disabled, not
allowing the user to add more points to the map.

Finally, we enable and activate the drag and drop feature to the urban park
layer, so the user can move the marker once he has placed it in the map::

    // Enable drag & drop in the urban park Layer
    var drag = new OpenLayers.Control.DragFeature(urbanParkLayer, {
        onComplete: function() {
            var urbanParkPoint = urbanParkLayer.features[0].geometry
            var tmp = urbanParkPoint.clone();
            tmp.transform(
                    map.getProjectionObject(), // from Spherical Mercator Projection
                    new OpenLayers.Projection("EPSG:4326") // to transform from WGS 1984
            );
            // When the marker has been dropped, update the lon & lat of the urban park
            $("#lon").text(tmp.x);       
            $("#lat").text(tmp.y);       
        }
    
    });
    // Add the drag & drop control into the map
    map.addControl(drag);
    // Activate drag & drop
    drag.activate();

The next sub-section explains how the **addCity function** loads the city
location into the map.

2.2. Loading the city coordinates into the map
----------------------------------------------

Every task in this application has an associated city location. Thus, we are
going to use a function called **addCity( cityname )** to load into the map its
position.

The function basically uses the `Nominatim geocoder server <http://wiki.openstreetmap.org/wiki/Nominatim>`_ to geocode every city name. The function will get the position, and transform it to a map point::

    // Geocode the city using Nominatim OSM service
    $.getJSON('http://nominatim.openstreetmap.org/search/' + city + '?format=json', function(output) {
        if (output.length >= 1) {
            //console.log("Lon: "+ output[0].lon + " Lat: " + output[0].lat);
            // Clean previous markers
            urbanParkLayer.removeAllFeatures();
            cityLayer.removeAllFeatures();
            // Activate the toolbar
            toolbar.activate();
            //console.log("Map cleaned!");
            // Create a LonLat object to load the city marker
            var lonLat = new OpenLayers.LonLat(output[0].lon, output[0].lat)
                .transform(
                    new OpenLayers.Projection("EPSG:4326"), // transform from WGS 1984
                    map.getProjectionObject() // to Spherical Mercator Projection
                );
            // Set the marker position
            point = new OpenLayers.Geometry.Point(lonLat.lon, lonLat.lat);
            cityLayer.addFeatures([new OpenLayers.Feature.Vector(point)]);
            // Center the map
            map.setCenter(lonLat,13);
            [...]

The function also cleans previous markers from the map, as the map is only
loaded once. Thus, it is important to remove all previous markers every time
this function is called, as well as enable the toolbar, so users can place
markers in the map.

Now, we only have to save the urban park marker position.

3. Saving the answer
--------------------

Once the task has been presented, the users can click on the answer buttons: **Save these coordinates for the urban park** or **Try another city**.

**Save these coordinates** save the answer in the DB (check **/api/taskrun**) with information about the task and the answer,
while the button **Try another city** simply loads another task as sometimes the
city may not have an urban park or the user does not find it.

In order to submit and save the answer from the user, we will use again the
PyBossa.JS library. In this case::

  pybossa.saveTask( taskid, answer )

The **pybossa.saveTask** method saves an answer for a given task. In the
previous section we saved in the DOM the task-id that we have loaded, so we can
retrieve this value and use it for saving the volunteer's answer.

The method allows us to give a successful pop-up feedback for the user, so we
will use the following structure to warn the user and tell him that his answer
has been saved, and load a new Task::

  pybossa.saveTask( taskid, answer ).done(
    function( data ) {
        // Show the feedback div
        $("#success").fadeIn(); 
        // Fade out the pop-up after a 1000 miliseconds
        setTimeout(function() { $("#success").fadeOut() }, 1000);
        // Finally, load a new task
        pybossa.newTask("flickrperson").done( function( data ){ loadData( data ) });
    };
  );

Now we only have to bind the action to the Save button so the above
snippet is called. In order to bind it, we will use the onclick event to call a new and
simple function for both buttons:

 * <button class="btn btn-success" onclick="submitTask()">Save these coordinates for the urban park</button>

The function submitTask will get the task-id from the DOM, and the answer will
be obtained from the map, thanks to the OpenLayers library::

    // Convert the feature location, the urban park marker position, into the GeoJSON format
    geojson = new OpenLayers.Format.GeoJSON({
        'internalProjection': map.baseLayer.projection,
        'externalProjection': new OpenLayers.Projection("EPSG:4326")
        });
    urbanPark = JSON.parse(geojson.write(urbanParkLayer.features[0]));
    pybossa.saveTask( task_id, {'city': city, 'urbanpark': urbanPark}).done(function(data) {});

Finally, the **Try another city** will use the same event, onclick, to request
a new task and load a new city into the map:

 * <button class="btn" onclick="pybossa.newTask('urbanpark').done( function( data ) { loadData( data ) });">Try another city</button>

For more details about the code, please, check the `template file
<https://github.com/PyBossa/app-geocoding/blob/master/app-geocoding/template.html>`_
for more details about all the steps.

4. Test the task presenter
--------------------------

In order to test the application task presenter, go to the following URL:

 * http://PYBOSSA-SERVER/app/SLUG/presenter

The presenter will load one task, and you will be able to submit and save one
answer for one task.
