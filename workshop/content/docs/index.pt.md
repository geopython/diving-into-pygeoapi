---
title: Mergulhando na pygeoapi
---

# Bem-vind@s à workshop Mergulhando na pygeoapi!

Versão: 1.10.0

![pygeoapi logo](assets/images/pygeoapi-logo.png)

A [pygeoapi](https://pygeoapi.io) é uma implementação em Python de um servidor da suíte de normas [OGC API](https://ogcapi.ogc.org). O projeto surgiu como parte dos esforços da próxima geração da OGC API em 2018 e oferece a capacidade de as organizações implementarem um ponto de acesso (endpoint) RESTful OGC API usando OpenAPI, GeoJSON e HTML. A pygeoapi é de código aberto e disponibilizada sob a licença MIT.

**Mergulhando na pygeoapi** é uma workshop de meio dia, desenhada para que os utilizadores se familiarizem com a instalação, configuração, publicação de dados e extensão da pygeoapi. Esta workshop abordará a publicação de dados geoespaciais na Web usando a pygeoapi, em conformidade com a suíte de normas OGC API.

Esta workshop abrange uma vasta gama de tópicos (instalação/configuração, publicação, cloud, modelos, plugins, etc.). Consulte o menu de navegação à esquerda para aceder ao índice.

# A vossa equipa da workshop da [FOSS4G 2026](https://2026.foss4g.org)

<table>    
    <tr>
        <td><a href="https://github.com/tomkralidis"><img width="150" src="https://avatars.githubusercontent.com/u/910430?v=4"/></a></td>
        <td><a href="https://github.com/doublebyte1"><img width="150" src="https://avatars.githubusercontent.com/u/1038897?v=4"/></a></td>
        <td><a href="https://github.com/jorgejesus"><img width="150" src="https://avatars.githubusercontent.com/u/1275400?v=4"/></a></td>
        <td><a href="https://github.com/PascalLike"><img width="150" src="https://avatars.githubusercontent.com/u/1323093?v=4"/></a></td>
        <td><a href="https://github.com/webb-ben"><img width="150" src="https://avatars.githubusercontent.com/u/40066515?v=4"/></a></td>
        <td><a href="https://github.com/ldesousa"><img width="150" src="https://avatars.githubusercontent.com/u/1137878?v=4"/></a></td>
        <td><a href="https://github.com/lcalisto"><img width="150" src="https://avatars.githubusercontent.com/u/4139084?v=4"/></a></td>
    </tr>
</table>

# Sobre este tutorial

Este tutorial é uma combinação de explicações passo a passo de vários aspetos da pygeoapi, bem como uma série de exercícios para se familiarizar com o projeto.

Os exercícios são indicados da seguinte forma:

!!! question "Exemplo de exercício"

    Uma secção marcada desta forma indica que pode experimentar o exercício.

!!! example "Exemplo de exercício com separadores"

    Uma secção marcada desta forma indica que pode experimentar o exercício e escolher o seu ambiente (Linux/Mac ou Windows).

    === "Linux/Mac"

        ```bash
        docker run -p 5000:80 -v $(pwd)/default.config.yml:/pygeoapi/local.config.yml geopython/pygeoapi:latest
        ```

    === "Windows"

        ```bash
        docker run -p 5000:80 -v ${pwd}/default.config.yml:/pygeoapi/local.config.yml geopython/pygeoapi:latest
        ```

Também irá notar secções de dicas e notas no texto:

!!! tip

    As dicas oferecem ajuda adicional sobre a melhor forma de realizar tarefas

Os exemplos são indicados da seguinte forma:

Código
``` {.html linenums="1"}
<html>
    <head>
        <title>This is an HTML sample</title>
    </head>
</html>
```

Configuração
``` {.yaml linenums="1"}
my-collection:
    type: collection
    title: my cool collection title
    description: my cool collection description
```

Fragmentos de código (snippets) que precisam de ser digitados num terminal/consola são indicados como:

```bash
echo 'Hello world'
```

# Localização e materiais da workshop

Esta workshop é sempre disponibilizada ao vivo em [https://dive.pygeoapi.io](https://dive.pygeoapi.io).

Os conteúdos da workshop, a wiki e o sistema de registo de problemas (issue tracker) são geridos no GitHub em [https://github.com/geopython/diving-into-pygeoapi](https://github.com/geopython/diving-into-pygeoapi).

# Suporte

Existe um canal no [Gitter](https://app.gitter.im/#/room/#geopython_diving-into-pygeoapi:gitter.im) para discussão e suporte ao vivo por parte dos desenvolvedores da workshop e de outros participantes.

Para problemas/bugs/sugestões ou melhorias/contribuições, por favor, utilize o [issue tracker do GitHub](https://github.com/geopython/diving-into-pygeoapi/issues).

Todos os bugs, melhorias e problemas podem ser reportados no [GitHub](https://github.com/geopython/diving-into-pygeoapi/issues).

Como sempre, o suporte principal da pygeoapi e as informações da comunidade podem ser encontrados no [website](https://pygeoapi.io/community) da pygeoapi.

As contribuições são sempre incentivadas e bem-vindas!

## Agora, vamos à workshop. [Vamos começar!](introduction.pt.md)
