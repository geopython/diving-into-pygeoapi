# Diving into pygeoapi

[![Gitter](https://img.shields.io/gitter/room/geopython/diving-into-pygeoapi)](https://matrix.to/#/#geopython_diving-into-pygeoapi:gitter.im)

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

The workshop manual is powered by [Zensical](https://zensical.org) which facilitates easy management
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
zensical build
# serve locally
zensical serve  # website is made available on http://localhost:8000
```

### Running from Github Codespaces

You can also use Github Codespaces after forking the repository:
<img width="964" height="269" alt="image" src="https://github.com/user-attachments/assets/e041edd6-c7cc-48ce-b5ea-f7bf7a1cbb7c" />

Then you can open port 5000 in the browser:
<img width="1402" height="728" alt="image" src="https://github.com/user-attachments/assets/2ce9fc13-fbc0-4e95-8da1-a2362907a0c6" />

You can use Terminal as usually. Navigate to exercises folder with ```cd workshop/exercises``` to proceed with the exercises. Docker container is already started from that folder. If you want to make changes to pygeoapi.config.yml, you can run ```docker compose down``` (from the same exercise folder), make your changes and restart it with ```docker compose up -d```.
<img width="1409" height="738" alt="image" src="https://github.com/user-attachments/assets/28090661-e9c8-4e94-b093-6171af4f4173" />

NOTE: Please consult [GitHub Codespaces documentation](https://docs.github.com/en/billing/concepts/product-billing/github-codespaces#free-and-billed-use-by-personal-accounts) for more information on personal GitHub accounts and free usage.

### Translating the workshop to a different language

Support to multiple languages is native in Zensical. To add an additional language to the workshop, create another toml file with the locale code as part of the filename. See an example [here](./workshop/content/zensical.pt.toml). 

- To enable the language switcher, update this block at the end of [the main configuration file](./workshop/content/zensical.toml):
    ```
    alternate =[
        { name = "English", link = "/", lang = "en" },
        { name = "Português", link = "/pt/", lang = "pt" }
    ] 
    ```
- foreach `.md` page in `workshop/content/docs`, add an equivalent page in the language with the locale code as part of the filename.  For example:
  - `ogcapi-records.md` -> `ogcapi-records.el.md`
- Update the [GitHub action](https://github.com/geopython/diving-into-pygeoapi/blob/main/.github/workflows/deploy.docs.yml), to also include a build for this language; example: `zensical build --strict --config-file zensical.pt.toml`

- commit to your fork and issue a GitHub Pull Request

NOTE: see [issue 217](https://github.com/geopython/diving-into-pygeoapi/issues/217) to track the implementation of auto-translation.

Note: Zensical can only serve one language at a time. Example:
 `zensical serve --config-file zensical.pt.toml`

If you want to test the language switcher: build the default English site, build the other language sites and spin up a basic local web server to view the combined output:

 ```
zensical build --config-file zensical.toml

zensical build --config-file zensical.pt.toml

python3 -m http.server --directory site
 ```

## Contributing updates

To make contributions back to the workshop, fork the repository from GitHub.  Contributions and Pull Requests are always welcome!

Changes to the GitHub repository result in an automated build and deploy of the content to [dive.pygeoapi.io](https://dive.pygeoapi.io).

## Deploying to live site

Website updates are automatically published via GitHub Actions.
