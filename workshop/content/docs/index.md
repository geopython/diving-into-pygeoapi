---
title: Diving into pygeoapi
---

# Diving into pygeoapi

![pygeoapi logo](assets/images/pygeoapi-logo.png)

[pygeoapi](https://pygeoapi.io) is a Python server implementation of the [OGC API](https://ogcapi.ogc.org) suite of standards. The project emerged as part of the next generation OGC API efforts in 2018 and provides the capability for organizations to deploy a RESTful OGC API endpoint using OpenAPI, GeoJSON, and HTML. pygeoapi is open source and released under an MIT license.

**Diving into pygeoapi** is a half day workshop designed for user to become familiar with installing, configuring, publishing data to and extending pygeoapi. This workshop will cover publishing geospatial data to the Web using pygeoapi in support of the suite of OGC API standards.


This workshop covers a wide range of topics (install/setup/configuration, publishing, cloud, templating, plugins, etc.). Please see the left hand navigation for the table of contents.

# Your [FOSS4G 2022](https://2022.foss4g.org) workshop team

<table>
    <tr>
        <td><a href="https://twitter.com/tomkralidis"><img width="150" src="https://avatars.githubusercontent.com/u/910430?v=4"/></a></td>
        <td><a href="https://twitter.com/justb4"><img width="150" src="https://avatars.githubusercontent.com/u/582630?v=4"/></a></td>
        <td><a href="https://twitter.com/pvangenuchten"><img width="150" src="https://avatars.githubusercontent.com/u/299829?v=4"/></a></td>
        <td><a href="https://twitter.com/doublebyte"><img width="150" src="https://avatars.githubusercontent.com/u/1038897?v=4"/></a></td>
        <td><a href="https://twitter.com/francbartoli"><img width="150" src="https://avatars.githubusercontent.com/u/560676?v=4"/></a></td>
        <td><a href="https://twitter.com/tzotsos"><img width="150" src="https://avatars.githubusercontent.com/u/383944?v=4"/></a></td>
        <td><a href="https://twitter.com/PascalLike"><img width="150" src="https://avatars.githubusercontent.com/u/1323093?v=4"/></a></td>
    </tr>
    <tr>
        <td>Tom Kralidis</td>
        <td>Just van den Broecke</td>
        <td>Paul van Genuchten</td>
        <td>Joana Simoes</td>
        <td>Francesco Bartoli</td>
        <td>Angelos Tzotsos</td>
        <td>Antonio Cerciello</td>
    </tr>
</table>

# About this tutorial

This tutorial is a combination of step-by-step explanations of various aspects of pygeoapi as well as a series of exercises to familiarize yourself with the project.

Exercises are indicated as follows:

!!! question "Example exercise"

    A section marked like this indicates that you can try out the exercise.

Also you will notice tips and notes setions within the text:

!!! tip

    Tips share additional help on how to best achieve tasks

Examples are indicated as follows:

Code
``` {.html linenums="1"}
<html>
    <head>
        <title>This is a html sample</title>
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
$ echo 'Hello world'
```
</div>

# Workshop location and materials

This workshop is always provided live at [https://dive.pygeoapi.io](https://dive.pygeoapi.io).

The workshop contents, wiki and issue tracker are managed on GitHub at [https://github.com/geopython/diving-into-pygeoapi](https://github.com/geopython/diving-into-pygeoapi).

# Support

A [Gitter](https://gitter.im/geopython/diving-into-pygeoapi) channel exists for
discussion and live support from the developers of the workshop and other workshop participants.

For issues/bugs/suggestions or improvements/contributions, use the [GitHub issue tracker](https://github.com/geopython/diving-into-pygeoapi/issues).

All bugs, enhancements and issues can be reported on [GitHub](https://github.com/geopython/diving-into-pygeoapi/issues).

As always, core pygeoapi support and community information can be found on the pygeoapi [website](https://pygeoapi.io/community).

Contributions are always enncouraged and welcome!


## Now, on to the workshop.  Let's go!
