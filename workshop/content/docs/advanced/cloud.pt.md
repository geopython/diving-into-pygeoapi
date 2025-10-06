---
title: Implementação na cloud
---

# Implementação na cloud

A implementação em infraestruturas cloud e conceitos como Microsserviços e [Twelve-Factor](https://12factor.net) apresentam requisitos específicos para
como o software é projetado e implementado. A pygeoapi suporta estes conceitos, tendo uma pegada baixa em CPU e memória, e não persiste estado
do utilizador, sendo portanto capaz de escalar sem riscos.

## pygeoapi e Docker

Uma [imagem Docker](https://hub.docker.com/r/geopython/pygeoapi) está disponível para a pygeoapi. Pode executar a imagem localmente como:

=== "Linux/Mac"

    <div class="termy">
    ```bash
    docker run -p 5000:80 geopython/pygeoapi:latest
    ```
    </div>

=== "Windows (PowerShell)"

    <div class="termy">
    ```bash
    docker run -p 5000:80 geopython/pygeoapi:latest
    ```
    </div>

!!! question "Rever o Dockerfile da pygeoapi"

    Note no [Dockerfile da pygeoapi](https://github.com/geopython/pygeoapi/Dockerfile) como o ficheiro open api é gerado como parte do script de arranque do Docker. 

Numa configuração típica, substituir-se-ia o ficheiro de configuração padrão da pygeoapi na imagem por um personalizado e incluir a pasta de dados:

!!! example "usar configuração personalizada"

    === "Linux/Mac"

        <div class="termy">
        ```bash
        docker run -p 5000:80 \ 
        -v $(pwd)/pygeoapi-config.yml:/pygeoapi/local.config.yml \
        -v $(pwd)/geodata:/geodata https://hub.docker.com/r/geopython/pygeoapi:latest
        ```
        </div>

    === "Windows (PowerShell)"

        <div class="termy">
        ```bash
        docker run -p 5000:80 -v ${pwd}/pygeoapi-config.yml:/pygeoapi/local.config.yml -v ${pwd}/geodata:/geodata https://hub.docker.com/r/geopython/pygeoapi:latest
        ```
        </div>


Alternativamente, pode construir uma nova imagem Docker incluindo tanto a configuração como os dados para o serviço. 

```
FROM geopython/pygeoapi:latest
COPY ./my.config.yml /pygeoapi/local.config.yml
```

Pode ter notado que o ficheiro de configuração da pygeoapi inclui uma referência ao endpoint no qual a pygeoapi é publicada. Esta configuração deve
corresponder ao endpoint público do serviço (domínio, caminho e porta).

Por defeito, a imagem Docker da pygeoapi executará a partir do caminho raiz `/`. Se precisar de executar a partir de um sub-caminho e ter todos os URLs internos corretos pode
definir a variável de ambiente `SCRIPT_NAME`.

=== "Linux/Mac"

    <div class="termy">
    ```bash
    docker run -p 5000:80 -e SCRIPT_NAME='/mypygeoapi' \
    -v $(pwd)/my.config.yml:/pygeoapi/local.config.yml -it geopython/pygeoapi
    # navegue para http://localhost:5000/mypygeoapi
    ```
    </div>

=== "Windows (PowerShell)"

    <div class="termy">
    ```bash
    docker run -p 5000:80 -e SCRIPT_NAME='/mypygeoapi' -v ${pwd}/my.config.yml:/pygeoapi/local.config.yml -it geopython/pygeoapi
    # navegue para http://localhost:5000/mypygeoapi
    ```
    </div>

# Resumo

Parabéns! Agora pode implementar a pygeoapi como um serviço cloud native.