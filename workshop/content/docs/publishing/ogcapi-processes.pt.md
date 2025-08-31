---
title: Exercício 8 - Funções via OGC API - Processes
---

# Exercício 8 - Funções via OGC API - Processes

[OGC API - Processes](https://ogcapi.ogc.org/processes) suporta o encapsulamento de tarefas computacionais em
processos executáveis que podem ser oferecidos por um servidor através de uma Web API e ser invocados por uma aplicação cliente.

* [OGC API - Processes: Part 1: Core](https://docs.ogc.org/is/18-062r2/18-062r2.html)

A OGC API - Processes utiliza a OGC API - Common como bloco de construção, permitindo assim implementação
e integração simplificadas para clientes e utilizadores.

## Suporte na pygeoapi

A pygeoapi suporta a especificação OGC API - Processes, com a capacidade de publicar código Python (independentemente de quão
simples ou complexo) como uma definição de Processo OGC API. A pygeoapi também suporta processamento síncrono ou assíncrono,
com a capacidade de armazenar e recuperar o estado/resultados de 'tarefas'.

!!! note

    Consulte [a documentação oficial](https://docs.pygeoapi.io/en/latest/data-publishing/ogcapi-processes.html) para mais informações sobre publicação de processos na pygeoapi


## Publicar código Python como um processo na pygeoapi

Com a pygeoapi vamos configurar OGC API - Processes usando código Python que implementa o `BaseProcessor` da pygeoapi, que é uma classe
abstrata base fundamental da pygeoapi. Neste exercício vamos implementar uma função "squared" como um processo usando o código Python de exemplo em
`workshop/exercises/plugins/process/squared.py`. O processo já está definido para fazer parte do ambiente e configuração da pygeoapi.

!!! question "Atualizar a configuração da pygeoapi"

    Abra o ficheiro de configuração da pygeoapi num editor de texto. Adicione uma nova secção de processo da seguinte forma:

``` {.yaml linenums="1"}
    squared:
        type: process
        processor:
            name: pygeoapi.process.squared.SquaredProcessor
```

!!! question "Atualizar código Python"

    Abra o código Python em `workshop/exercises/plugins/process/squared.py`. Encontre a função `execute` e atualize o código Python
    para calcular o valor de entrada ao quadrado.


Guarde a configuração e reinicie o Docker Compose. Navegue para <http://localhost:5000/processes> para avaliar se o novo processo foi
publicado. Inspecione os metadados detalhados do processo navegando para <http://localhost:5000/processes/squared> para inspecionar como os metadados do processo
definidos no código/ficheiro Python são disponibilizados em JSON.

## Acesso cliente

### Swagger

A forma mais fácil de testar o novo processo é usando a interface Swagger incorporada da pygeoapi. Navegue para <http://localhost:5000/openapi> e experimente
o processo na interface Swagger.

![Execução do processo função squared](../assets/images/oaproc-squared-1.png){ width=120% }

![Execução do processo função squared](../assets/images/oaproc-squared-2.png){ width=120% }

![Execução do processo função squared](../assets/images/oaproc-squared-3.png){ width=120% }


# Resumo

Parabéns! Agora pode publicar código Python como um processo na pygeoapi.