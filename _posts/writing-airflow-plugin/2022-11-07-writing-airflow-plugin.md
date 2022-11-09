---
title: Writing Airflow Plugin
tags: [Airflow, Guide, HowTo]
date: 2022-11-07 16:50:47 +05:30
description: A step by step guide to get started with Airflow Plugin Development. 
image: "/wirting-airflow-plugin/plugin.jpeg"
comments: true
---
<figure>
<img src="plugin.jpg" alt="plugin">
<figcaption style="color: grey !important;"> 
	Photo by <a href="https://unsplash.com/@purzlbaum" style="color: grey !important;" target="_blank">Claudio Schwarz
</a> 
</figcaption>
</figure>

I recently pulished [Aerofoil](https://github.com/anu2602/aerofoil), an Airflow Plugin. In the process, I reallized there isn't a good Guide on writing Airflow Pluins, so here is I am writing a simple, practical and hopefully useful guide. 

### Airflow Plugins
Airflow has a simple plugin manager built-in that can integrate external features to its core by either dropping files in your `$AIRFLOW_HOME/plugins` folder or installing python modulues that implement plugin's interface. Pluings are useful for extending and customizing Airflow as per the needs. Plugin can be used for:
- Creating new Operatos, Sensors, Hooks
- Creating macros 
- Modifying Airflow UI
- Custom timetables
- Event Listeners 

### Why Write Airflow Plugin?
- To provide connectivity to your data product: You provide a data service and chances are your customers are using Airflow to connect to your system. They all are writing same code, instead if you could provide a pluin, that can make life easy for them.
- To customize your Airflow Installation: You may want to add Menu Links, change UI for your Airflow installation e.g you want to provide link to your documentation from Airflow or you want build add a simple data exploration capability. 
- To add custom or customized Operator/Hooks/Sensors: Though there are provider packages for almost any meaningful system you want to connect via Airflow, you may sometime need to write your own operator or modify operator.
-  and so on ..

### Plugin Packaging
There are two simple way to package plugin. First is simply dropping files into Airflow  `plugins` folder. `plugins` Folder is defined in `airflow.cfg`, airflow loads the code in this folder. This is simplest way of writing plugin, and if you are doing this you can probably skip this guide and user Airflow documentation. This guide is more suited to someone who intend to publish a reusable plugin. 

### Plugin src structure:
I recommend following structure for your plugin:
```
src/
  <mymodule>/
  operators (optional)
  sensors (optional)
  views (optional)
  static (optional)
  templates (optional)
  __init__.py
  <myplugin>.py  
setup.py
README.md
MANIFEST.in
```

### Development Environment 
First of all you need to have a running Airflow in your development environment in order for you to be able to test, which is fairly straight forward. After that you make want to find  your `plugins` folder in `airflow.cfg`, this is usually `$AIRFLOW_HOM/plugins`.

To be able to test, I recommend you create a symlink to `<mymodule>` in `plugins` folder.
```
cd $AIRFLOW_HOME/plugins
ln -s /path/to/plugin-code/src/<mymodule> . 
```

Also set `reload_on_plugin_change` to `true` in `[webserver]` section of `airflow.cfg`. This is specially helpful if you are developing new UI features and you want to preview your changes without restarting webservers. Take it with a pinch of salt, as some of the changes in the template do not reflect and you need to restart your webserver. 

### Entry Point
Entry point to the plugin is the plugin file. The file should define a class exending `airflow.plugins_manager.AirflowPlugin`
#### Example
```
from airflow.plugins_manager import AirflowPlugin

from flask import Blueprint
from flask_appbuilder import expose, BaseView as AppBuilderBaseView
from airflow.hooks.base import BaseHook

#Import your custome classes
from <mymodule>.operators import MyHook
rom <mymodule>.views import MyView

def my_macro():
    pass

my_view = MyView()

my_bp = Blueprint(
	'my_plugin', __name__,
	 template_folder='templates',
	 static_folder='<mymodule>/static',
	 static_url_path='/static/<myurlplath>'
)

my_pkg = {
	'name' : 'My View',
	'category' : 'My New Menu',
	'view': my_view
}

my_mitem = {
    "name": "Google",
    "href": "https://www.google.com",
    "category": "My New Menu'",
}

class MyAirflowPlugin(AirflowPlugin):
    name = "my_plugin"
    hooks = [MyHook]
    macros = [my_macro]
    flask_blueprints = [my_bp]
    appbuilder_views = [my_pkg]
    appbuilder_menu_items = [my_mitem]
```
#### Understanding the Plugin Interface
```from airflow.plugins_manager import AirflowPlugin```
Airflow plugin needs to extend the AirflowPlugin class as displayed above and below items needs to be defined in your subclass as per your need. 

<div class="datatable-begin"></div>

| Plugin Property | Usage |
| --------------- | ----- |
| `name` | This is the name of your Plugin | 
| `hooks` | List of your custom Hook classes go here |
| `macro` | A macro can be defined as a function, list of macros go here | 
| `flask_blueprints` | A list of [blueprints](https://stackoverflow.com/questions/24420857/what-are-flask-blueprints-exactly){:target="_blank"} created for Flask, usually you will have only one. | 
| `appbuilder_views` | These are Menu Items for your view (UI Screens). You can either add to existing Menu Group (Category) or create a new one.|
| `appbuilder_menu_items` | Additional external links to be added to Airflow Menu | 

<div class="datatable-end"></div>

#### Entry Point
Class implementing plugin interface is the entrypoint for your plugin. If you drop your pluggin into the Airflow's `plugins` folder, creating a AirflowPlugin implmentation and keeping is sufficient. But if you are creating a python package and plugin is loaded via setuptools entrypoint mechanism. In your setup.py configure entrypoint as mentioned below:
```
entry_points={
        'apache_airflow_provider': [
            'provider_info=<mymoduler>.__init__:get_provider_info'
        ],
        'airflow.plugins': [
            'aerofoil=<mymodule>.my_plugin:MyPlugin'
        ]
    },
```
Where MyPlugin is implemenation of AirflowPlugin as defined in `mymodule/my_plugin.py`.

### References
1. [Airflow Plugin Documentation](https://airflow.apache.org/docs/apache-airflow/stable/plugins.html){:target="_blank"}
2. [Aerofoil code](https://github.com/anu2602/aerofoil){:target="_blank"}
3. [Airflow Plugins in Github](https://github.com/airflow-plugins){:target="_blank"}
4. [What is Flask Blueprint](https://stackoverflow.com/questions/24420857/what-are-flask-blueprints-exactly){:target="_blank"}
5. [Flask-AppBuilder configuration reference](https://flask-appbuilder.readthedocs.io/en/latest/config.html){:target="_blank"}
