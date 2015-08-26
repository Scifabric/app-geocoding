PyBossa geo-demo project: Urban Parks in cities
===============================================

This demo project shows how you can use PyBossa for geo-coding problems.

The project is a simple demo that uses OpenLayers JavaScript library to load a map,
center it and add a point to the map, in order to mark the position of the
urban park. 

Thanks to the use of OpenLayers the data can be exported in any standard GIS format:
KML, GeoJSON, etc. In this project the answers are saved as GeoJSON.

![alt screenshot](http://i.imgur.com/XMVyNKV.png)

The project has five main files:

*  project.json: a JSON file that describes the project.
*  long_description.md: a Markdown file with a long description of the project.
*  cities.csv: a CSV file that contains sample tasks to be used by the project.
*  template.html: the view for every task and deal with the data of the answers.
*  tutorial.html: a simple tutorial for the volunteers.


Testing the project:
====================

You need to install pybossa-pbs. If you don't have a virtual environment,
we recommend you to create one, and activate it:

```bash
    $ virtualenv env
    $ source env/bin/activate
```

Then, you can install pybossa-pbs:

```bash
    $ pip install pybossa-pbs
```

Or if you prefer:

```bash
    $ pip install -r requirements.txt
```

## Creating an account in a PyBossa server

Now that you've all the requirements installed in your system, you need
a PyBossa account:

*  Create an account in your PyBossa server (use [Crowdcrafting](http://crowdcrafting.org) if you want).
*  Copy your API-KEY (you can find it in your profile page).

## Configure pybossa-pbs command line

PyBossa-pbs command line tool can be configured with a config file in order to
avoid typing the API-KEY and the server every time you want to take an action
on your project. For this reason, we recommend you to actually create the
config file. For creating the file, follow the next steps:

```bash
    $ cd ~
    $ editorofyourchoice .pybossa.cfg
```

That will create a file. Now paste the following:

```ini
[default]
server: http://yourpybossaserver.com
apikey: yourapikey
```

Save the file, and you are done! From now on, pybossa-pbs will always use the
default section to run your commands.

## Create the project

Now that we've everything in place, creating the project is as simple as
running this command:

```bash
    $ pbs create_project
```

## Using a CSV file for adding tasks

Now we can add some tasks. The project comes with a sample tasks file in CSV format
that you can use. The CSV file has two headers, **question** (for the question to be
displayed to volunteers about the task) and **city** (with the name of the city
and country it is located).

This is very simple too, thanks to pbs:

```bash
    $ pbs add_tasks --tasks-file cities.csv
```
You'll get a progress bar with the tasks being uploaded. Now your project has
some tasks in the server to be processed by the volunteers.

## Finally, add the task presenter, tutorial and long description

Now that we've some data to process, let's add to our project the required
templates to show a better description of our project, to present the tasks to
our users, and a small tutorial for the volunteers:

```bash
    $ pbs update_project
```

Done!

**NOTE**: we provide templates also for Bootstrap v2 in case your PyBossa
server is using Bootstrap2 instead of Bootstrap3. See the rest of the files.


Documentation
=============

We recommend that you read the section: [Build with PyBossa](http://docs.pybossa.com/en/latest/build_with_pybossa.html) and follow the [step by step tutorial](http://docs.pybossa.com/en/latest/user/tutorial.html).

**NOTE**: This project uses the [pybossa-pbs](https://pypi.python.org/pypi/pybossa-pbs) library in order to simplify the development of the project and its usage. Check the [documentation](https://github.com/PyBossa/pbs).

LICENSE
=======

Please, see the COPYING file.

Acknowledgments
===============
The thumbnail has been created using a [photo](http://www.flickr.com/photos/infomatique/5489548540/) from William Murphy (license CC¬BY-SA 2.0).
