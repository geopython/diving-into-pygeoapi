---
title: Using pygeoapi in downstream applications
---

# Using pygeoapi in downstream applications

While pygeoapi is typically run as a standalone application, it is also designed
to enable direct usage via external Python applications in a number of different
design patterns.at multiple levels. From the [official documentation](https://docs.pygeoapi.io/en/latest/how-pygeoapi-works.html), the below
diagram provides an overview of how pygeoapi is designed and architected:

[![how pygeoapi works](https://docs.pygeoapi.io/en/latest/_images/how-pygeoapi-works.png)](https://docs.pygeoapi.io/en/latest/how-pygeoapi-works.html)

There are two main ways to create a downstream application:

- Using the core API
- Extending through the web interface of the frameworks supported out-of-the box

## Using the core API directly

The core pygeoapi Python API entrypoint is `pygeoapi.api.API`, which is initialized with the pygeoapi configuration
as a Python `dict`.

!!! note

    The pygeoapi core API enables the developer to manage pygeoapi configuration in any number of ways
    (file on disk, object storage, database driven, etc.)

From here, API objects provide a number of functions, most of which require a [`pygeoapi.api.APIRequest`](https://docs.pygeoapi.io/en/latest/api-documentation.html#pygeoapi.api.APIRequest) object
according to the web framework. Examples include:

- [Flask](https://flask.palletsprojects.com/en/latest/api/#incoming-request-data)
- [Starlette](https://www.starlette.io/requests)
- [FastAPI](https://fastapi.tiangolo.com/advanced/using-request-directly)
- [Django](https://docs.djangoproject.com/en/4.1/ref/request-response/#django.http.HttpRequest)

!!! note

    See the [official documentation](https://docs.pygeoapi.io/en/latest/api-documentation.html#pygeoapi.api.APIRequest)
    for more information about `pygeoapi.api.APIRequest` (you can even use your own custom request object as long as it
    satisfies the interface requirements of `pygeoapi.api.APIRequest`.

Let's take a look at what a bare bones API integration would look like, using Flask as an example:

```python

from flask import Flask, make_response, request

from pygeoapi.api import API
from pygeoapi.util import yaml_load

my_flask_app = Flask(__name__)

with open('my-pygeoapi-config.yml') as fh:
    my_pygeoapi_config = yaml_load(fh)

my_pygeoapi_api = API(my_pygeoapi_config)

@my_flask_app.route('/my-landing-page-route')
def my_def():

    headers, status, content = my_pygeoapi_api.landing_page(request)

    response = make_response(content, status)

    if headers: 
        response.headers = headers

    return response
```

!!! note

    See the [official documentation](https://docs.pygeoapi.io/en/latest/api-documentation.html#module-pygeoapi.api)
    for more information on the core Python API

## Extending through a web framework

pygeoapi can be installed and used at the web routing level as a dependency in your project. This is pretty much the easier way to leverage the flexibility and the modularity of its architecture.
Once the interfaces are available then the developer can use the preferred framework for serving the frontend application. In practice
the following modules:

- `pygeoapi.flask_app.py` for Flask blueprints
- `pygeoapi.starlette_app.py` for Starlette/FastAPI
- `pygeoapi.django_app.py` for Django (ongoing [PR](https://github.com/geopython/pygeoapi/pull/630))

Some examples are available below for developers.

### Examples

#### Flask blueprints

```python
from flask import Flask

from pygeoapi.flask_app import BLUEPRINT as pygeoapi_blueprint

my_flask_app = Flask(__name__, static_url_path='/static')
my_flask_app.url_map.strict_slashes = False

# mount all pygeoapi endpoints to /oapi
my_flask_app.register_blueprint(pygeoapi_blueprint, url_prefix='/oapi')


@my_flask_app.route('/')
def home():
    return '<p>home page</p>'
```

#### Starlette and FastAPI

```python

import uvicorn
from fastapi import FastAPI
from fastapi.exceptions import RequestValidationError
from starlette.exceptions import HTTPException as StarletteHTTPException
from starlette.middleware.cors import CORSMiddleware

from pygeoapi.starlette_app import app as pygeoapi_app


def create_app() -> FastAPI:
    """Handle application creation."""
    app = FastAPI(title="my_pygeoapi", root_path="", debug=True)

    # Set all CORS enabled origins
    app.add_middleware(
        CORSMiddleware,
        allow_origins=["*"],
        allow_credentials=True,
        allow_methods=["*"],
        allow_headers=["*"],
    )

    @app.exception_handler(StarletteHTTPException)
    async def custom_http_exception_handler(request, e):
        return await http_exception_handler(request, e)

    @app.exception_handler(RequestValidationError)
    async def custom_validation_exception_handler(request, e):
        return await request_validation_exception_handler(request, e)

    # mount all pygeoapi endpoints to /oapi
    app.mount(path="/oapi", app=pygeoapi_app)

    return app

app = create_app()

if __name__ == "__main__":
    uvicorn.run(app, port=5000)
```
