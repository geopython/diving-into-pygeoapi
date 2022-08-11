# Diving into pygeoapi

Welcome to the Diving into pygeoapi workshop!

[pygeoapi](https://pygeoapi.io) is a Python server implementation of the [OGC API](https://ogcapi.ogc.org) suite of standards. The project emerged as part of the next generation OGC API efforts in 2018 and provides the capability for organizations to deploy a RESTful OGC API endpoint using OpenAPI, GeoJSON, and HTML. pygeoapi is open source and released under an MIT license.

## For users

Are you a workshop participant or want to dive-in individually?  Go to [dive.pygeoapi.io](https://dive.pygeoapi.io/) to follow the lessons and exercises.

## For authors

Below are guidelines for authoring and/or improving the workshop's content.

### Setting up the pygeoapi environment

This workshop uses Docker (Docker, Docker Compose) to ensure a consistent environment
to deploy pygeoapi and work through the various exercises. As with participants, follow
the [Workshop environment setup](https://dive.pygeoapi.io/setup).

### Building the workshop content locally

The workshop manual is powered by [MkDocs](https://www.mkdocs.org) which facilitates easy management
of content and publishing. Workshop content is written in Markdown.


### Setting up the manual environment locally

```bash
# build a virtual Python environment in isolation
python3 -m venv .
. bin/activate
# fork or clone from GitHub
git clone https://github.com/geopython/diving-into-pygeoapi.git
cd diving-into-pygeoapi/workshop/content
# install required dependencies
pip install -r requirements.txt
# build the website
mkdocs build
# serve locally
mkdocs serve  # website is made available on http://localhost:8000
```

## Contributing updates

To make contributions back to the workshop, fork the repository from GitHub.  Contributions and Pull Requests are always welcome!

Changes to the GitHub repository result in an automated build and deploy of the content to [dive.pygeoapi.io](https://dive.pygeoapi.io).

## Deploying to live site

Website updates are automatically published via GitHub Actions. To publish manually:

```bash
# NOTE: you require access privileges to the GitHub repository
# to publish live updates
mkdocs gh-deploy -m 'add new page on topic x'
```
