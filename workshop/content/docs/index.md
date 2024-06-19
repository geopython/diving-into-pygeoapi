---
title: Diving into pygeoapi
---

# Welcome to the Diving into pygeoapi workshop!

Version: 1.1

![pygeoapi logo](assets/images/pygeoapi-logo.png)

[pygeoapi](https://pygeoapi.io) is a Python server implementation of the [OGC API](https://ogcapi.ogc.org) suite of standards. The project emerged as part of the next generation OGC API efforts in 2018 and provides the capability for organizations to deploy a RESTful OGC API endpoint using OpenAPI, GeoJSON, and HTML. pygeoapi is open source and released under an MIT license.

**Diving into pygeoapi** is a half day workshop designed for users to become familiar with installing, configuring, publishing data to and extending pygeoapi. This workshop will cover publishing geospatial data to the Web using pygeoapi in support of the suite of OGC API standards.


This workshop covers a wide range of topics (install/setup/configuration, publishing, cloud, templating, plugins, etc.). Please see the left hand navigation for the table of contents.

# Your [FOSS4G Europe 2024](https://2024.europe.foss4g.org) workshop team

<table>
    <tr>
        <td><a href="https://twitter.com/tomkralidis"><img width="150" src="https://avatars.githubusercontent.com/u/910430?v=4"/></a></td>
        <td><a href="https://twitter.com/doublebyte"><img width="150" src="https://avatars.githubusercontent.com/u/1038897?v=4"/></a></td>
        <td><a href="https://twitter.com/pvangenuchten"><img width="150" src="https://avatars.githubusercontent.com/u/299829?v=4"/></a></td>
        <td><a href="https://twitter.com/justb4"><img width="150" src="https://avatars.githubusercontent.com/u/582630?v=4"/></a></td>
        <td><a href="https://twitter.com/krishnaglodha"><img width="150" src="https://avatars.githubusercontent.com/u/47075664?v=4"/></a></td>
    </tr>
    <tr>
        <td><a href="https://github.com/tomkralidis">Tom Kralidis</a></td>
        <td><a href="https://github.com/doublebyte1">Joana Simoes</a></td>
        <td><a href="https://github.com/pvangenuchten">Paul van Genuchten</a></td>
        <td><a href="https://github.com/justb4">Just van den Broecke</a></td>
        <td><a href="https://github.com/krishnaglodha">Krishna Lodha</a></td>
    </tr>
</table>

# About this tutorial

This tutorial is a combination of step-by-step explanations of various aspects of pygeoapi as well as a series of exercises to familiarize yourself with the project.

Exercises are indicated as follows:

!!! question "Example exercise"

    A section marked like this indicates that you can try out the exercise.

!!! example "Example exercise with tabs"

    A section marked like this indicates that you can try out the exercise and choose your environment (Linux/Mac or Windows).

    === "Linux/Mac"
        <div class="termy">
        ```bash
        docker run -p 5000:80 -v $(pwd)/default.config.yml:/pygeoapi/local.config.yml geopython/pygeoapi:latest
        ```
        </div>
    === "Windows"
        <div class="termy">
        ```bash
        docker run -p 5000:80 -v ${pwd}/default.config.yml:/pygeoapi/local.config.yml geopython/pygeoapi:latest
        ```
        </div>

Also you will notice tips and notes sections within the text:

!!! tip

    Tips share additional help on how to best achieve tasks

Examples are indicated as follows:

Code
``` {.html linenums="1"}
<html>
    <head>
        <title>This is an HTML sample</title>
    </head>
</html>
```

Configuration
``` {.yaml linenums="1"}
my-collection:
    type: collection
    title: my cool collection title
    description: my cool collection description
```

Snippets which need to be typed in a on a terminal/console are indicated as:

<div class="termy">
```bash
echo 'Hello world'
```
</div>

# Workshop location and materials

This workshop is always provided live at [https://dive.pygeoapi.io](https://dive.pygeoapi.io).

The workshop contents, wiki and issue tracker are managed on GitHub at [https://github.com/geopython/diving-into-pygeoapi](https://github.com/geopython/diving-into-pygeoapi).

# Support

A [Gitter](https://app.gitter.im/#/room/#geopython_diving-into-pygeoapi:gitter.im) channel exists for
discussion and live support from the developers of the workshop and other workshop participants.

For issues/bugs/suggestions or improvements/contributions, please use the [GitHub issue tracker](https://github.com/geopython/diving-into-pygeoapi/issues).

All bugs, enhancements and issues can be reported on [GitHub](https://github.com/geopython/diving-into-pygeoapi/issues).

As always, core pygeoapi support and community information can be found on the pygeoapi [website](https://pygeoapi.io/community).

Contributions are always enncouraged and welcome!


## Now, on to the workshop.  Let's go!
