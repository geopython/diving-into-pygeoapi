---
title: UI customization and templating
---

# UI customization and templating

pygeoapi adopted the Jinja2 templating mechanism to style HTML output. Each element visualized on the HTML output is
customizable by overriding the relevant template. Templates are located in the [`pygeoapi/templates`](https://github.com/geopython/pygeoapi/tree/master/pygeoapi/templates) folder.
It is possible to override any template by copying it into a separate folder and adjust it to your needs. In the pygeoapi
configuration you can then indicate the path to the override folder. Notice that for files which are not placed
in the override folder, the original file is used.

!!! caution

    For any customization, mind that with a new version of pygeoapi changes on the default templates are not automatically
    available on the overriden files. Upgrades need to be carefully tested and validated.

## Jinja2

[Jinja2](https://jinja.palletsprojects.com) is a common templating concept in the Python community. With a minimal background
in HTML you will be able to make minor but meaningful customizations. At the core of pygeoapi's template setup is the
[`_base.html`](https://github.com/geopython/pygeoapi/blob/master/pygeoapi/templates/_base.html) template, which defines the
header and footer of the page. The fragment below defines the footer of the page, notice the parameters in curly braces,
which are replaced by dynamic content. 

``` {.html linenums="1"}
 <footer class="sticky">
    {% trans %}Powered by {% endtrans %} 
    <a title="pygeoapi" href="https://pygeoapi.io">
        <img src="{{ config['server']['url'] }}/static/img/pygeoapi.png" title="pygeoapi logo" style="height:24px;vertical-align: middle;"/></a> 
    {{ version }}
</footer>
```

!!! help "Customizing an HTML page"

    Copy `_base.html` to a separate folder. Adjust some elements on that page (e.g. logo image). Then, include a reference to the new folder in
    the pygeoapi configuration. Restart the service. Verify the result.

## CSS customizations

From the customized HTML template you can reference a new stylesheet file with customizations or directly add your customizations to [/static/css/default.css](https://github.com/geopython/pygeoapi/blob/master/pygeoapi/static/css/default.css).

# Summary

Congratulations! You've added a custom look and feel to your pygeoapi deployment.
