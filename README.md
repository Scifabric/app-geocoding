PyBossa geo-demo application: Urban Parks in cities

This demo application shows how you can use PyBossa in tasks that involves maps. The application is a simple demo
about how you can use PyBossa and OpenLayers to get the longitude and latitude for an specific point. In this case
the task consist in placing in a map one urban park for a city. Thanks to the use of OpenLayers the data can be 
exported in any format, in this demo it is saved as GeoJSON.

This application has three files:

*  createTasks.py: for creating the application in PyBossa
*  template.html: the view for every task and deal with the data of the answers.

Testing the application:

*  Create an account in PyBossa
*  Copy under your account your API-KEY
*  Run python createTasks.py -u http://pybossa.com -k API-KEY
*  Open with your browser the Applications section and choose the Urban Parks application. This will open the presenter for this demo application.

Please, check the full documentation here:

http://app-geocoding.rtfd.org
