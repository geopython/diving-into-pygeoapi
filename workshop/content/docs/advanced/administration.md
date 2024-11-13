---
title: Administration
---

# Administration

## Overview

pygeoapi provides an administration API (see the pygeoapi [documentation](https://docs.pygeoapi.io/en/latest/admin-api.html) for more information on how to enable, configure and use) in support of managing its configuration.  The API (not an OGC API) is implementated as a RESTful service to help create, update, replace or delete various elements of pygeoapi configuration.

## User interface

By design, pygeoapi does not provide a user interface to administer the configuration.  Given that the admin API exists, a few options can be considered for developing an admin UI:

- standalone
    - simple application with no connectivity to the pygeoapi admin API
    - built off the pygeoapi configuration [schema](https://github.com/geopython/pygeoapi/blob/master/pygeoapi/schemas/config/pygeoapi-config-0.x.yml)
    - allows for paste of existing pygeoapi configuration
    - allows for generating pygeoapi configuration for copy/paste into a pygeoapi deployment
    - can be deployed anywhere (for example, GitHub Pages)
- integrated
    - connected application to a pygeoapi deployment
    - built off the pygeoapi configuration [schema](https://github.com/geopython/pygeoapi/blob/master/pygeoapi/schemas/config/pygeoapi-config-0.x.yml)
    - reads/writes a live pygeoapi configuration via the pygeoapi admin API (access controlled)
    - deployed as part of a Docker Compose application

!!! note

    Have your own idea for a pygeoapi admin UI?  Connect with the [pygeoapi community](https://pygeoapi.io/community) to discuss your idea!
