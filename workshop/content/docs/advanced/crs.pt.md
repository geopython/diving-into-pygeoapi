---
title: Suporte a Sistemas de Referência de Coordenadas (CRS)
---

# Suporte CRS

A partir da versão 0.15.0, a pygeoapi suporta totalmente [OGC API - Features - Part 2: Coordinate Reference Systems by Reference](https://docs.opengeospatial.org/is/18-058r1/18-058r1.html).
Isto permite a importação e exportação de quaisquer dados de acordo com projeções dedicadas.
Uma "projeção" é especificada com um identificador de Sistema de Referência de Coordenadas (CRS). Estes estão em formatos URI
como `http://www.opengis.net/def/crs/OGC/1.3/CRS84` (basicamente WGS84 em ordem de eixo longitude, latitude)
ou o formato "OpenGIS" como `http://www.opengis.net/def/crs/EPSG/0/4258`. Note que o formato "EPSG:" como `EPSG:4326`
está fora do âmbito da norma OGC.

Em particular, o suporte CRS permite o seguinte:

- especificar o CRS no qual os dados são armazenados, na pygeoapi a opção de configuração `storageCRS` 
- especificar a lista de CRSs nos quais os dados de Funcionalidades podem ser recuperados, na pygeoapi a opção de configuração `crs`
- publicar estes CRSs nos metadados da coleção
- o parâmetro de consulta `crs=` para uma coleção ou item de coleção
- o parâmetro de consulta `bbox-crs=` para indicar que o parâmetro `bbox=` está codificado nesse CRS
- o cabeçalho de resposta HTTP `Content-Crs` denota o CRS da(s) Funcionalidade(s) nos dados devolvidos

Então, embora o GeoJSON exija WGS84 em ordem longitude, latitude, o cliente e servidor podem ainda assim concordar
com outros CRSs.

Por baixo, a pygeoapi usa o bem conhecido wrapper Python [pyproj](https://pyproj4.github.io/pyproj/stable) para a biblioteca [PROJ](https://proj.org).
                                                                                               
Leia mais na documentação da pygeoapi no [Capítulo CRS](https://docs.pygeoapi.io/en/latest/crs.html).

# Exercício

Adicionar suporte CRS às coleções pygeoapi para o tipo `provider` `feature` é tão simples como
por exemplo estender a configuração do [Exercício 2](../publishing/ogcapi-features.md) com este fragmento:

```yaml
  crs:
      - http://www.opengis.net/def/crs/OGC/1.3/CRS84
      - http://www.opengis.net/def/crs/EPSG/0/4258
      - http://www.opengis.net/def/crs/EPSG/0/3857
      - http://www.opengis.net/def/crs/EPSG/0/4326
  storage_crs: http://www.opengis.net/def/crs/OGC/1.3/CRS84
```


!!! Ordem dos eixos

    A ordem dos eixos (são coordenadas em ordem longitude, latitude ou latitude, longitude?) nas projeções é frequentemente uma fonte de confusão. 
    No entanto, o formato URI é bastante claro sobre isto, pelo menos mais do que o formato `EPSG:`.
    Então http://www.opengis.net/def/crs/OGC/1.3/CRS84 é ordem longitude, latitude, enquanto
    http://www.opengis.net/def/crs/EPSG/0/4326 é ordem latitude, longitude.
    
 
Na configuração abaixo, indicamos basicamente que os dados são armazenados em WGS84 (ordem de eixo longitude, latitude) e podem ser recuperados
em CRSs como `http://www.opengis.net/def/crs/EPSG/0/4258` (ETRS89 ordem de eixo latitude, longitude) etc.

!!! question "Adicionar CRS a uma configuração pygeoapi"

    Abra o ficheiro de configuração da pygeoapi num editor de texto.
    Encontre a linha `# START - EXERCISE 2 - firenze-terrain`

    Atualize a secção do conjunto de dados com suporte CRS substituindo-a pelo fragmento abaixo:

    ``` {.yaml linenums="1"}
    firenze-terrains-vec:
        type: collection
        title: Limites administrativos antes de 2014
        description: Parcelas cadastrais (terrenos) do cadastro. Agência do Território; SIT e Redes de Informação;
        keywords:
            - Parcelas cadastrais
        links:
            - type: text/html
              rel: canonical
              title: Limites administrativos antes de 2014
              href: http://dati.cittametropolitana.fi.it/geonetwork/srv/metadata/cmfi:c539d359-4387-4f83-a6f4-cd546b3d8443
              hreflang: it
        extents:
            spatial:
                bbox: [11.23,43.75,11.28,43.78]
                crs: http://www.opengis.net/def/crs/OGC/1.3/CRS84
        providers:
            - type: feature
              name: SQLiteGPKG
              data: /data/firenze_terrains.gpkg # colocar caminho correto aqui
              id_field: fid
              crs:
                - http://www.opengis.net/def/crs/OGC/1.3/CRS84
                - http://www.opengis.net/def/crs/EPSG/0/4258
                - http://www.opengis.net/def/crs/EPSG/0/3857
                - http://www.opengis.net/def/crs/EPSG/0/4326
              storage_crs: http://www.opengis.net/def/crs/OGC/1.3/CRS84
              title_field: codbo
              table: firenze_terrains
    ```
 
Agora vamos inspecionar os metadados da coleção e recuperar Funcionalidades em vários CRSs.
Podemos até fazer isto na interface Swagger, mas usar o navegador é bastante rápido e claro.

## Metadados

!!! question "Metadados da Coleção"

    Abra o URL: 
    <http://localhost:5000/collections/firenze-terrains-vec>
    Os seus CRSs configurados são exibidos na parte inferior da página: "Reference Systems" e "Storage CRS".
    
    Veja estes em formato JSON, também na parte inferior: 
    <http://localhost:5000/collections/firenze-terrains-vec?f=json>
    ```yaml
       .
       .
       "crs":[
        "http://www.opengis.net/def/crs/OGC/1.3/CRS84",
        "http://www.opengis.net/def/crs/EPSG/0/4258",
        "http://www.opengis.net/def/crs/EPSG/0/3857",
        "http://www.opengis.net/def/crs/EPSG/0/4326"
       ],
       "storageCRS":"http://www.opengis.net/def/crs/OGC/1.3/CRS84"
     }
    ```

## Reprojetar Funcionalidades

!!! question "Usar o parâmetro de consulta CRS"

    Abra o URL: 
    <http://localhost:5000/collections/firenze-terrains-vec/items?f=json&crs=http://www.opengis.net/def/crs/EPSG/0/4258>

    Este é ETRS89, semelhante a WGS84, mas para o Continente Europeu (Datum) e em ordem lat,lon. Este é por exemplo usado no INSPIRE.

    Veja estes em formato JSON, também na parte inferior:

    ```json
    "type":"FeatureCollection",
      "features":[
          {
              "type":"Feature",
              "geometry":{
                  "type":"MultiPolygon",
                  "coordinates":[
                      [
                          [
                              [
                                  43.77805936835436,
                                  11.23486287997071
                              ],
                              [
                                  43.77809089595012,
                                  11.2348943159564
                              ],
                              [
                                  43.77810038978989,
                                  11.23491359066035
                              ],
                              [
                                  43.77705757917591,
                                  11.2368990806804
                              ],
       .
       .
       "crs":[
        "http://www.opengis.net/def/crs/OGC/1.3/CRS84",
        "http://www.opengis.net/def/crs/EPSG/0/4258",
        "http://www.opengis.net/def/crs/EPSG/0/3857",
        "http://www.opengis.net/def/crs/EPSG/0/4326"
       ],
       "storageCRS":"http://www.opengis.net/def/crs/OGC/1.3/CRS84"
     }
    ```

    Se abrir a consola de desenvolvimento do navegador, pode observar o cabeçalho de resposta HTTP:

    `Content-Crs: <http://www.opengis.net/def/crs/EPSG/0/4258>`

    (O URI CRS está sempre entre `<` `>`)