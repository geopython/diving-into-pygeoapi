---
title: Introdução à pygeoapi
---

# Introdução à pygeoapi

A equipa de desenvolvimento da pygeoapi (sim, escrita em minúsculas) tem o prazer de lhe dar as boas-vindas a esta workshop!

Nesta workshop de meio dia, fazemos uma introdução à pygeoapi e à publicação de dados, e fornecemos também recursos e dicas para leitura e referência futuras (ou seja, para onde ir quando não souber!).

Embora a pygeoapi seja escrita em Python e seja personalizável e extensível (através de _plugins_) por
programadores de Python, não são necessários conhecimentos de Python para instalar, configurar e publicar os seus
dados geoespaciais como parte desta workshop. Tudo o que precisa para a workshop é o seu editor de texto favorito
e o Docker (forneceremos mais informações na [secção de configuração](setup.pt.md)).

## Leitura de base

O [website](https://pygeoapi.io) da pygeoapi é o ponto de entrada principal para utilizadores finais e programadores.
Aí pode encontrar:

* [documentação oficial](https://docs.pygeoapi.io)
* [arquivo de apresentações](https://pygeoapi.io/presentations)
* [documentação](https://pygeoapi.io/documentation)
* código-fonte no [GitHub](https://github.com/geopython/pygeoapi)
* imagens Docker [disponíveis no Docker Hub](https://hub.docker.com/r/geopython/pygeoapi)
* versões da pygeoapi no [Python Package Index (PyPI)](https://pypi.org/project/pygeoapi)

Dado que a pygeoapi implementa uma série de normas OGC API, também poderá querer ler sobre estas
em [ogcapi.ogc.org](https://ogcapi.ogc.org).

## Instalações existentes

Várias organizações implementaram a pygeoapi nas suas operações. Para ter uma ideia de como a pygeoapi é usada na
prática, consulte a nossa página atualizada de
[implementações ativas](https://github.com/geopython/pygeoapi/wiki/LiveDeployments).

Por omissão, a instalação de demonstração pública da pygeoapi em [demo.pygeoapi.io](https://demo.pygeoapi.io) é
sempre mantida e disponibilizada pela equipa de desenvolvimento. Consulte a
[instância principal](https://demo.pygeoapi.io/master), que executa sempre a versão mais recente do GitHub.

Interessado na configuração do próprio site de demonstração? A [demo.pygeoapi.io](https://demo.pygeoapi.io) é
desenvolvida num [repositório GitHub](https://github.com/geopython/demo.pygeoapi.io) utilizando um fluxo de trabalho
de disponibilização contínua (CD). Implementações GitOps ainda mais recentes foram desenvolvidas para
a [Geonovum](https://github.com/Geonovum/ogc-api-testbed) e para o [Centro Comum de Investigação da Comissão Europeia](https://github.com/justb4/ogc-api-jrc).

Os exemplos acima podem servir como ponto de partida para a sua própria configuração e implementação da pygeoapi,
por isso, sinta-se à vontade para os estudar e utilizar!

## História

Iniciada em 2018, a pygeoapi surgiu como parte dos esforços iniciais para o desenvolvimento das normas OGC API.
Os *code sprints* da OGC API foram fundamentais para o desenvolvimento ágil e para lançar as bases do projeto.

Os princípios centrais de design são os seguintes:

* Simplicidade / baixa barreira de entrada
* Sustentabilidade a longo prazo
* Modularidade
* Extensibilidade
* Construção sobre um vasto ecossistema de componentes de Código Aberto e da OSGeo, como GDAL, rasterio, Shapely,
  Pandas, Elasticsearch, PostGIS e muitos outros

O projeto foi iniciado por [Tom Kralidis](https://github.com/tomkralidis). Em poucas semanas, vários programadores
talentosos juntaram-se ao projeto, o que levou à formação de uma equipa principal e de
um [Comité de Direção do Projeto (PSC)](https://pygeoapi.io/community/psc). As contribuições continuaram também por
parte de outros programadores e utilizadores que, de bom grado, forneceram novas funcionalidades, correções de erros
e atualizações da documentação. Como resultado, rapidamente emergiu uma comunidade saudável com um interesse comum
em código aberto, normas OGC API, baixa barreira de entrada, modularidade e extensibilidade. O resto, como se diz,
é história.

A pygeoapi é um [projeto da OSGeo](https://www.osgeo.org/projects/pygeoapi) e uma implementação de referência da OGC.
