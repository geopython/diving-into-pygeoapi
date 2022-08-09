---
title: Search Engine Optimisation (SEO)
---

# Search Engine Optimisation (SEO)

OGC API - Features adopted the Spatial Data on the Web [Best Practice 2: Make your spatial data indexable by search engines](https://www.w3.org/TR/sdw-bp/#indexable-by-search-engines) with the recommendation to [include HTML as an output format of any OGC API](http://docs.ogc.org/is/17-069r3/17-069r3.html#_requirements_class_html). It means that users can navigate an OGC API from within their browser and Search Engines are able to crawl the content.

An aspect to consider is that the API becomes a webpage and common practices for web development become relevant: 

- Does the website have a clear navigation? 
- A company logo, branding, privacy statement, cookie warning, etc. should be included?
- Is the webpage accessable [WCAG](https://www.w3.org/TR/WCAG21/)? 

!!! tip

    Notice that the pygeoapi configuration also has an option to disable html output. In that scenario only the json output is available.

All websites on the web are incidentally visited by [web crawlers](https://en.wikipedia.org/wiki/Web_crawler) of the popular search engines. These are automated processes which build up the index of the search engine. Crawlers follow links on the web to identify new content. Cross linking your api to other resources therefore increases the visibility (and ranking) of your api.

The British Geo6 wrote an extensive [best practice on SEO for data publishers](https://www.gov.uk/government/publications/search-engine-optimisation-for-publishers-best-practice-guide) which offers a good overview of SEO in the scope of data publications.

## Tweak Web Crawler behaviour

This paragraph introduces you to some mechanisms to facilitate or block web crawlers to index your content.

If you're not interested to have your content indexed by search engines. You can indicate on a [robots.txt](https://en.wikipedia.org/wiki/Robots_exclusion_standard) file in the root folder of your website, which folders should not be indexed. More drastically is the option to block access for bots to your content by filtering traffic to the website based on the [user-agent header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/User-Agent). Such a rule can be added to firewall or webserver software.

Robots.txt can also include a link to a [sitemap.xml](https://en.wikipedia.org/wiki/Sitemaps). Many search engines provide the option to submit a sitemap to speed up the indexation process. pygeoapi does  not provide a sitemap of its content, but you can create a sitemap with the main topics manually. 

Search engines provide tooling to evaluate the search behaviour of your website. These tools usually give a very interesting insight to your findability. For example; what keywords do people use to locate your website?

## Schema.org/Dataset

Search engines cooperate in the [schema.org](https://schema.org) initiative. Schema.org enables annotation your website using the schema.org ontology, so search engines can index the content in a structured way. Google was the first to use this mechanism to provide a [dedicated search engine for datasets](https://datasetsearch.research.google.com/). pygeoapi adds schema.org/Dataset annotations to collection pages, so collections are automatilly included in the dataset search.

!!! question "Evaluate the schema.org annotations in collections"

    Google provides a [tool to evaluate schema.org annotations](https://search.google.com/test/rich-results) in websites. Evalutate a collection page of pygeoapi in the tool. If you run pygeoapi locally, you can copy the `source of the page` as html into the `<code>` tab, else paste the url of the page in the `URL` tab. (A similar tool is made available by [Yandex](https://webmaster.yandex.com/tools/microtest), registration required)
