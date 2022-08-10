---
title: Search Engine Optimization (SEO)
---

# Search Engine Optimization (SEO)

OGC API - Features adopted the Spatial Data on the Web [Best Practice 2: Make your spatial data indexable by search engines](https://www.w3.org/TR/sdw-bp/#indexable-by-search-engines) with the recommendation to [include HTML as an output format of any OGC API](http://docs.ogc.org/is/17-069r3/17-069r3.html#_requirements_class_html). It means that users can navigate an OGC API from within their browser and Search Engines are able to crawl the content.

An aspect to consider is that since the API becomes a webpage and common practices for web architecture and development become relevant:

- does the website have a clear navigation?
- is a company logo, branding, privacy statement, cookie warning included?
- is the webpage accessable [WCAG](https://www.w3.org/TR/WCAG21)?

!!! tip

    Notice that the pygeoapi configuration also has an option to disable HTML output. In that scenario, only the JSON output is available.
    TODO: Paul? validate functionality

On the Web, websites are typically visited by [web crawlers](https://en.wikipedia.org/wiki/Web_crawler) by popular search engines. These
are automated processes which aid in building the index of the search engine. Crawlers follow links on the Web to identify new or updated
content. Cross linking your API to other resources therefore increases the visibility (and ranking) of your API.

The British Geo6 wrote an extensive [best practice on SEO for data publishers](https://www.gov.uk/government/publications/search-engine-optimisation-for-publishers-best-practice-guide) which offers a good overview of SEO in the scope of data publications.

## Tweaking Web Crawler behaviour

This paragraph introduces you to some mechanisms which facilitate or block web crawlers to index your content.

If you are not interested in having your content indexed by search engines, you can provide a [robots.txt](https://en.wikipedia.org/wiki/Robots_exclusion_standard)
file in root of your website, specifying which folders should not be indexed. More drastically is the option to block access for crawlers or bots to your content
by filtering traffic to the website based on the HTTP [User-Agent header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/User-Agent). Such a rule can also
be added to firewall or web server configuration.

A `robots.txt` file can also include a link to a [Sitemap](https://en.wikipedia.org/wiki/Sitemaps). Many search engines provide the option to submit a sitemap
in order to speed up crawling and indexing. pygeoapi does not provide a sitemap of its content, but you can create your own sitemap (publishing to `/sitemap.xml`),
specifying URLs of your desired conrent to be indexed.

Search engines provide tooling to evaluate the search behaviour of your website. These tools can provide valuable insight to the findability of your website
and content (e.g. keywords used to locate your website).

## Schema.org/Dataset

Search engines cooperate in the [Schema.org](https://schema.org) initiative. Schema.org enables annotation your website using the `schema.org` ontology, in
order for search engines to index the content in a structured manner. Google was the first to use this mechanism to provide a [dedicated search engine for datasets](https://datasetsearch.research.google.com/). pygeoapi adds `schema.org/Dataset` annotations to collection pages, so collections are automagically included in Google's dataset search.

!!! question "Evaluate the schema.org annotations in collections"

    Google provides a [tool](https://search.google.com/test/rich-results) to evaluate Schema.org annotation in websites. Try evaluating a collection endpoint of pygeoapi
    in the tool. If you run pygeoapi locally, you can copy the source of the page as HTML into the `<code>` tab, otherwise you can paste the URL of the page in the `URL` tab.

!!! note

    A similar tool is made available by [Yandex](https://webmaster.yandex.com/tools/microtest) (note that registration is required)
