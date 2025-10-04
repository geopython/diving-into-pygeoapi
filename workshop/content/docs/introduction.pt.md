---
title: Introdução à pygeoapi
---

# Introdução à pygeoapi

A equipa de desenvolvimento da pygeoapi (sim, em minúsculas) tem o prazer de lhe dar as boas-vindas a esta workshop!

Nesta workshop de meio dia[^1], fazemos uma introdução à pygeoapi, e à publicação de dados geoespaciais, fornecendo 
também recursos e dicas para leitura e referência futuras (ou seja, para onde ir quando não souber!).

[^1]: A sessão apresentada na [FSL 2025](https://festa2025.softwarelivre.eu/) é uma sessão simplificada de 2h30m.

Embora a pygeoapi seja escrita em Python e seja personalizável e extensível (através de _plugins_) por 
programadores de Python, não são necessários conhecimentos de Python para instalar, configurar e publicar os seus 
dados geoespaciais como parte desta workshop. Tudo o que precisa para a workshop é o seu editor de texto favorito 
e o Docker (forneceremos mais informações na [secção de configuração](setup.md)).

## Leitura de base

O [website](https://pygeoapi.io) da pygeoapi é o ponto de entrada principal para utilizadores finais e programadores. 
Aí pode encontrar:

* [Documentação oficial](https://docs.pygeoapi.io)
* Várias [apresentações do projeto](https://pygeoapi.io/presentations)
* P código-fonte no [GitHub](https://github.com/geopython/pygeoapi)
* As imagens Docker [disponíveis no Docker Hub](https://hub.docker.com/r/geopython/pygeoapi)
* As versões da pygeoapi no [Python Package Index (PyPI)](https://pypi.org/project/pygeoapi)
* Dado que a pygeoapi implementa uma série de normas OGC API, também poderá querer ler sobre estas 
  em [ogcapi.ogc.org](https://ogcapi.ogc.org).

## Instalações de referência

Várias organizações implementaram a pygeoapi nas suas operações. Para ter uma ideia de como a pygeoapi é usada na 
prática, consulte a nossa página atualizada de 
[implementações ativas](https://github.com/geopython/pygeoapi/wiki/LiveDeployments). 

O projeto pygeoapi também mantém uma instalação de demonstração, disponível em:

<https://demo.pygeoapi.io>

Esta instalação é operada pela equipa de desenvolvimento. Consulte a 
[instância principal](https://demo.pygeoapi.io/master), que executa sempre a versão mais recente do GitHub.

Interessado na configuração do próprio site de demonstração? O [demo.pygeoapi.io](https://demo.pygeoapi.io) é 
desenvolvido num [repositório GitHub](https://github.com/geopython/demo.pygeoapi.io) utilizando um fluxo de trabalho 
de disponibilização contínua (CD). Implementações GitOps ainda mais recentes foram desenvolvidas para 
a [Geonovum](https://github.com/Geonovum/ogc-api-testbed) e para o [Centro Comum de Investigação da Comissão Europeia](https://github.com/justb4/ogc-api-jrc).

Os exemplos acima podem servir como ponto de partida para a sua própria configuração e implementação da pygeoapi, 
por isso, sinta-se à vontade para os estudar e utilizar!

## História

Iniciada em 2018, a pygeoapi surgiu como parte dos esforços iniciais para o desenvolvimento das normas OGC API. 
As *code sprints* da OGC API foram fundamentais para o desenvolvimento ágil e para lançar as bases do projeto.

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
em código aberto, normas OGC API, baixa barreira de entrada, modularidade e extensibilidade. O resto, como se 
costuma dizer, é história.

Em Fevereiro de 2024 foi realizado o primeiro encontro de desenvolvedores da pygeoapi, em Évora

A pygeoapi é um [Projeto da OSGeo](https://www.osgeo.org/projects/pygeoapi) e uma Implementação de Referência da OGC.
