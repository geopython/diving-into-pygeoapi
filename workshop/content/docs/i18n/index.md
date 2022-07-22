---
title: Multilingual support
---

# Multilingual support

pygeoapi supports multilinguality at two levels. 

- `Hardcoded` text strings returned by the API's as part of the json and html outputs can be translated to relevant languages using the [Babel conventions](https://github.com/python-babel/babel). 
- For each column in a dataset you can annotate its language. If a dataset contains columns in multiple languages, the API will try to return data responses in the requested language.

!!! note

  Error messages are not translated, to facilitate copy-paste of the error into [stackoverflow](https://stackoverflow.com/search?q=pygeoapi) and [github issues](https://github.com/geopython/pygeoapi/issues).

Language negotiation is triggered by the `Accept-Language` header sent by the client, but can be overridden with a `?lang=fr` attribute.

## Multilingual configuration

In the pygeoapi configuration file you can indicate the languages supported by the instance. The first language is the default language. For most of the textual configuration properties you can provide a translation in alternative languages.

```
lakes:
  type: collection
  title:
      en: Large Lakes
      de: Grands Lacs
  description:
      en: lakes of the world, public domain
      de: lacs du monde, domaine public
  keywords:
      fr:
          - lakes
          - water bodies
      fr:
          - lacs
          - plans d'eau
```

## Text strings within jinja2 templates

Most of the text strings exist within the jinja2 templates. Text strings to be translated are placed in a `trans` tag:

```
<title>{% trans %}Page title{% endtrans %}</title>
```

Babel provides a utility which extracts all keys to be translated from the templates into a `.pot` file. 

```
pybabel extract -F babel-mapping.ini -o locale/messages.pot ./
```

This `.pot` file is used to create or update existing `.po` files, which exist for every language and contain the actual translations.

```
pybabel init -d locale -l it -i locale/messages.pot
```

The `.po` files are stored on the source code github repository. You can create a Pull Request to add or update your favourite languages. `.po` files can also be added to translation software such as [transifex.com](https://transifex.com). 

!!! question "Edit a `.po` file"

    Open a `.po` file from the [locale](https://github.com/geopython/pygeoapi/tree/master/locale) folder in a text editor. Edit some values. Save the file and restart the service. Verify that the updated content is available. You can also try to add a new key to a template and translate it via the `.po` mechanism.

## Annotating the language of data columns

pygeoapi has some features to influence the api responses based on the requested language. If your service operates in a multilingual area, it makes sense to add textual columns in multiple languages. In the pygeoapi configuration file you can for example indicate which column can be used as the title field, for which language. 

!!! question "Publish a multilingual dataset"

    In the workshop data folder we've prepared a multilingual dataset of `free wifi hotspots in Florence`. Add the dataset to the pygeoapi configuration using the CSV provider. Add a title-field configuration with for each translated column the relevant language.

    ```
    data: ./data/free-wifi-florence.csv
    id_field: id
    title_field: 
      en: name-en
      it: name-it
      de: name-de
    ```

    Test the configuration by navigating to the items page of the collection and switching the language by appending `?lang=it`, `?lang=de` to the url.