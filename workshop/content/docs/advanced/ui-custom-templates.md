---
title: Customising layout with jinja2 templates 
---

# Customising layout

pygeoapi adopted the jinja2 templating mechanism to style the html output. Each element visualised on the html output is customisable by overriding the relevant template. Templates are located in the [/pygeoapi/templates](https://github.com/geopython/pygeoapi/tree/master/pygeoapi/templates) folder. It is possible to override any template by copying it into a separate folder and adjust it to your needs. In the pygeoapi-config.yml file you indicate the path to the override folder. Notice that for files which are not placed in the override folder, the original file is used.

For any customisation, mind that with a new version of pygeoapi changes on the default templates are not automatically available on the overriden files. Upgrades need to be carefully tested.

## jinja2

The [jinja2](https://jinja.palletsprojects.com/en/2.9.x/intro/) is a common templating concept in the Python community. With a minimal background on HTML you'll be able to make small customisations. Most likely to override is the [_base.html](https://github.com/geopython/pygeoapi/blob/master/pygeoapi/templates/_base.html) template, which defines the header and footer of the page. The fragment below defines the footer of the page, notice the parameters in curly braces, which are replaced by dynamic content. 

``` {.html linenums="1"}
 <footer class="sticky">
    {% trans %}Powered by {% endtrans %} 
    <a title="pygeoapi" href="https://pygeoapi.io">
        <img src="{{ config['server']['url'] }}/static/img/pygeoapi.png" title="pygeoapi logo" style="height:24px;vertical-align: middle;"/></a> 
    {{ version }}
</footer>
    
```

!!! help "Customising a html page"

    Copy _base.html to a separate folder. Adjust some elements on that page, for example the logo image. Now include a reference to the new folder in the pygeoapi-config file. Restart the service. Verify the result.

## CSS customisations

From the customised html template you can reference a new stylesheet file with customisations or directly add your customisations to [/static/css/default.css](https://github.com/geopython/pygeoapi/blob/master/pygeoapi/static/css/default.css).

