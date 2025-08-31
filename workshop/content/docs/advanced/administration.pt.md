---
title: Administração
---

# Administração

## Visão geral

A pygeoapi fornece uma API de administração (consulte a [documentação](https://docs.pygeoapi.io/en/latest/admin-api.html) da pygeoapi para mais informações sobre como ativar, configurar e usar) em suporte à gestão da sua configuração. A API (não uma OGC API) é implementada como um serviço RESTful para ajudar a criar, atualizar, substituir ou eliminar vários elementos da configuração da pygeoapi. Uma UI simples apenas de leitura é implementada como parte da API de administração.

## Interface de utilizador

Por design, a pygeoapi não fornece uma verdadeira interface de utilizador para administrar a configuração. Dado que a API de administração existe, algumas opções podem ser consideradas para desenvolver uma UI de administração:

- autónoma
    - aplicação simples sem conectividade à API de administração da pygeoapi
    - construída a partir do [esquema](https://github.com/geopython/pygeoapi/blob/master/pygeoapi/schemas/config/pygeoapi-config-0.x.yml) de configuração da pygeoapi
    - permite copiar uma configuração já existente da pygeoapi
    - permite gerar configuração da pygeoapi para copiar/colar numa implementação da pygeoapi
    - pode ser implementada em qualquer lugar (por exemplo, GitHub Pages)
- integrada
    - aplicação conectada a uma implementação da pygeoapi
    - construída a partir do [esquema](https://github.com/geopython/pygeoapi/blob/master/pygeoapi/schemas/config/pygeoapi-config-0.x.yml) de configuração da pygeoapi
    - lê/escreve uma configuração da pygeoapi em tempo real através da API de administração da pygeoapi (controlo de acesso)
    - implementada como parte de uma aplicação Docker Compose

!!! note

    Tem a sua própria ideia para uma UI de administração da pygeoapi? Conecte-se com a [comunidade da pygeoapi](https://pygeoapi.io/community) para discutir a sua ideia!