---
title: Otimização para Motores de Busca (SEO)
---

# Otimização para Motores de Busca (SEO)

A OGC API - Features adotou a [Boa Prática 2 de Dados Espaciais na Web: Tornar os seus dados espaciais indexáveis por motores de busca](https://www.w3.org/TR/sdw-bp/#indexable-by-search-engines) com a recomendação de [incluir HTML como formato de saída de qualquer OGC API](http://docs.ogc.org/is/17-069r3/17-069r3.html#_requirements_class_html). Isto significa que os utilizadores podem navegar numa OGC API a partir do seu navegador e os Motores de Busca conseguem rastrear o conteúdo.

Um aspeto a considerar é que, uma vez que a API se torna uma página web, as práticas comuns para arquitetura web e desenvolvimento tornam-se relevantes:

- o website tem uma navegação clara?
- está incluído um logótipo da empresa, marca, declaração de privacidade, aviso de cookies?
- a página web é [WCAG](https://www.w3.org/TR/WCAG21) acessível?

!!! tip

    Note que a configuração da pygeoapi também tem uma opção para desativar a saída HTML. Nesse cenário, apenas a saída JSON está disponível.

Na Web, os websites são tipicamente visitados por [web crawlers](https://en.wikipedia.org/wiki/Web_crawler) de motores de busca populares. Os crawlers
são processos automatizados que ajudam a construir o índice do motor de busca. Os crawlers seguem ligações na Web para identificar conteúdo novo ou atualizado
. A ligação cruzada da sua API a outros recursos aumenta portanto a visibilidade (e classificação) da sua API.

O British Geo6 escreveu uma extensa [boa prática sobre SEO para editores de dados](https://www.gov.uk/government/publications/search-engine-optimisation-for-publishers-best-practice-guide) que oferece uma boa visão geral do SEO no âmbito das publicações de dados.

## Ajustar o comportamento dos Web Crawlers

Este parágrafo apresenta alguns mecanismos que facilitam ou bloqueiam web crawlers de indexar o seu conteúdo.

Se não está interessado em ter o seu conteúdo indexado por motores de busca, pode fornecer um ficheiro [robots.txt](https://en.wikipedia.org/wiki/Robots_exclusion_standard)
na raiz do seu website, especificando que pastas não devem ser indexadas. Mais drasticamente é a opção de bloquear o acesso para crawlers ou bots ao seu conteúdo
filtrando tráfego para o website baseado no cabeçalho HTTP [User-Agent](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/User-Agent). Tal regra pode
ser adicionada a uma configuração de firewall ou servidor web.

Um ficheiro `robots.txt` também pode incluir uma ligação para um [Sitemap](https://en.wikipedia.org/wiki/Sitemaps). Muitos motores de busca fornecem a opção de submeter um sitemap
para acelerar o rastreamento e indexação. A pygeoapi não fornece um sitemap do seu conteúdo, mas pode criar o seu próprio sitemap (publicar como `/sitemap.xml`),
especificando URLs do seu conteúdo desejado para ser indexado.

Os motores de busca fornecem ferramentas para avaliar o comportamento de pesquisa do seu website. Estas ferramentas podem fornecer informações valiosas sobre a capacidade de encontrar o seu website
e conteúdo (por exemplo, palavras-chave usadas para localizar o seu website).

## Schema.org/Dataset

Os motores de busca cooperam na iniciativa [Schema.org](https://schema.org). O Schema.org permite-lhe anotar o seu website usando o vocabulário `schema.org`,
para que os motores de busca indexem o conteúdo de forma estruturada. O Google foi o primeiro a empregar estas anotações para fornecer um [motor de busca dedicado para conjuntos de dados](https://datasetsearch.research.google.com/). A pygeoapi adiciona anotações `schema.org/Dataset` às páginas de coleção, para que as coleções sejam automaticamente incluídas na pesquisa de conjuntos de dados do Google.

!!! question "Avaliar as anotações schema.org em coleções"

    O Google fornece uma [ferramenta](https://search.google.com/test/rich-results) para avaliar anotações `Schema.org` em websites. Tente avaliar um endpoint de coleção da pygeoapi
    na ferramenta. Se executar a pygeoapi localmente (não acessível ao google), pode copiar o código fonte de uma página como HTML no separador `<code>`, caso contrário pode colar o URL da página no separador `URL`.

!!! note

    Uma ferramenta semelhante é disponibilizada pelo [Yandex](https://webmaster.yandex.com/tools/microtest) (note que é necessário registo).