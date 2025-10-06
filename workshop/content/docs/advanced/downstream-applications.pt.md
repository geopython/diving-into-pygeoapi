---
title: Usar a pygeoapi em aplicações downstream
---

# Usar a pygeoapi em aplicações downstream

Embora a pygeoapi seja tipicamente executada como uma aplicação autónoma, também foi projetada
para permitir uso direto através de aplicações Python externas em vários padrões
de design diferentes a múltiplos níveis. Da [documentação oficial](https://docs.pygeoapi.io/en/latest/how-pygeoapi-works.html), o diagrama abaixo
fornece uma visão geral de como a pygeoapi é projetada e arquitetada:

[![como a pygeoapi funciona](https://docs.pygeoapi.io/en/latest/_images/how-pygeoapi-works.png)](https://docs.pygeoapi.io/en/latest/how-pygeoapi-works.html)

Há duas formas principais de criar uma aplicação downstream:

- Usar a API principal
- Estender através da interface web das frameworks suportadas out-of-the box

## Usar a API principal diretamente

O ponto de entrada da API Python principal da pygeoapi é `pygeoapi.api.API`, que é inicializada com a configuração da pygeoapi
como um `dict` Python.

!!! note

    A API principal da pygeoapi permite ao programador gerir a configuração da pygeoapi de várias formas
    (ficheiro em disco, armazenamento de objetos, baseado em base de dados, etc.)

A partir daqui, os objetos API fornecem várias funções, a maioria das quais requer um objeto [`pygeoapi.api.APIRequest`](https://docs.pygeoapi.io/en/latest/api-documentation.html#pygeoapi.api.APIRequest)
de acordo com a framework web. Exemplos incluem:

- [Flask](https://flask.palletsprojects.com/en/latest/api/#incoming-request-data)
- [Starlette](https://www.starlette.io/requests)
- [FastAPI](https://fastapi.tiangolo.com/advanced/using-request-directly)
- [Django](https://docs.djangoproject.com/en/4.1/ref/request-response/#django.http.HttpRequest)

!!! note

    Consulte a [documentação oficial](https://docs.pygeoapi.io/en/latest/api-documentation.html#pygeoapi.api.APIRequest)
    para mais informações sobre `pygeoapi.api.APIRequest` (pode até usar o seu próprio objeto de pedido personalizado desde que
    satisfaça os requisitos de interface de `pygeoapi.api.APIRequest`.

Vamos ver como seria uma integração API básica, usando Flask como exemplo:

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

    Consulte a [documentação oficial](https://docs.pygeoapi.io/en/latest/api-documentation.html#module-pygeoapi.api)
    para mais informações sobre a API Python principal

## Estender através de uma framework web

A pygeoapi pode ser instalada e usada ao nível do roteamento web como uma dependência no seu projeto. Esta é praticamente a forma mais fácil de aproveitar a flexibilidade e a modularidade da sua arquitetura.
Uma vez que as interfaces estejam disponíveis, então o programador pode usar a framework preferida para servir a aplicação frontend. Na prática
os seguintes módulos:

- `pygeoapi.flask_app.py` para blueprints Flask
- `pygeoapi.starlette_app.py` para Starlette/FastAPI
- `pygeoapi.django_app.py` para Django (PR em curso [PR](https://github.com/geopython/pygeoapi/pull/630))

Alguns exemplos estão disponíveis abaixo para programadores.

### Exemplos

#### Blueprints Flask

```python
from flask import Flask

from pygeoapi.flask_app import BLUEPRINT as pygeoapi_blueprint

my_flask_app = Flask(__name__, static_url_path='/static')
my_flask_app.url_map.strict_slashes = False

# montar todos os endpoints da pygeoapi em /oapi
my_flask_app.register_blueprint(pygeoapi_blueprint, url_prefix='/oapi')


@my_flask_app.route('/')
def home():
    return '<p>home page</p>'
```

#### Starlette e FastAPI

```python

import uvicorn
from fastapi import FastAPI
from fastapi.exceptions import RequestValidationError
from starlette.exceptions import HTTPException as StarletteHTTPException
from starlette.middleware.cors import CORSMiddleware

from pygeoapi.starlette_app import app as pygeoapi_app


def create_app() -> FastAPI:
    """Gerir criação da aplicação."""
    app = FastAPI(title="my_pygeoapi", root_path="", debug=True)

    # Definir todas as origens CORS habilitadas
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

    # montar todos os endpoints da pygeoapi em /oapi
    app.mount(path="/oapi", app=pygeoapi_app)

    return app

app = create_app()

if __name__ == "__main__":
    uvicorn.run(app, port=5000)
```