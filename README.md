# Diving into pygeoapi
pygeoapi is an OGC Reference Implementation supporting numerous OGC API specifications. This workshop will cover publishing geospatial data to the Web using pygeoapi in support of the suite of OGC API standards. 

**Are you a workshop participant or want to dive-in individually?**   
**Go to the website [dive.pygeoapi.io](https://dive.pygeoapi.io/) and find instructions there.**   
Below are guidelines for authoring and/or improving the workshop's content.

## Setting up pygeoapi environment

This workshop uses Docker-technology all-over (Docker, Docker Compose). As a workshop-author you need to 
have Docker running on your system, like the participants. For this you can follow the [Workshop Setup Guide](https://dive.pygeoapi.io/setup/).
Only for the workshop content you need to fork/clone this repo as to make contributions via PRs etc.

## Building the manual

The workshop manual is powered by
by [MkDocs](https://www.mkdocs.org) which facilitates easy management
of content and publishing. On push/commit changes under [workshop/content](workshop/content) the site [dive.pygeoapi.io](https://dive.pygeoapi.io/) is re-generated and published (in GH Pages).

### Setting up the manual environment locally

```bash
# build a virtual Python environment in isolation
python3 -m venv .
. bin/activate
# clone from GitHub
git clone https://github.com/geopython/diving-into-pygeoapi.git
cd diving-into-pygeoapi/workshop/content
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
