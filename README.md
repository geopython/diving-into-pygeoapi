# Diving into pygeoapi
pygeoapi is an OGC Reference Implementation supporting numerous OGC API specifications. This workshop will cover publishing geospatial data to the Web using pygeoapi in support of the suite of OGC API standards.

## Building the manual

The workshop manual is powered by
by [MkDocs](https://www.mkdocs.org) which facilitates easy management
of content and publishing.

### Setting up the manual environment locally

```bash
# build a virtual Python environment in isolation
python3 -m venv .
. bin/activate
# download the website from GitHub
git clone https://github.com/geopython/diving-into-pygeoapi.git
cd diving-into-pygeoapi/workshop
# install required dependencies
pip install -r requirements.txt
# build the website
mkdocs build
# serve locally
mkdocs serve  # website is made available on http://localhost:8000/
```

### Publishing updates to the live site

Changes to the manual can be suggested via pull requests.

Website updates are automatically published via GitHub Actions, but just in case:

```bash
# NOTE: you require access privileges to the GitHub repository
# to publish live updates
mkdocs gh-deploy -m 'add new page on topic x'
```
