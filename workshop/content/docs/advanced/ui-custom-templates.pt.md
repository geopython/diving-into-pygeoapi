---
title: Personalização de UI e templating
---

# Personalização de UI e templating

A pygeoapi adotou o mecanismo de templating Jinja2 para estilizar a saída HTML. Cada elemento visualizado na saída HTML é
personalizável substituindo o template relevante. Os templates estão localizados na pasta [`pygeoapi/templates`](https://github.com/geopython/pygeoapi/tree/master/pygeoapi/templates).
É possível substituir qualquer template copiando-o para uma pasta separada e ajustá-lo às suas necessidades. Na configuração da pygeoapi
pode então indicar o caminho para a pasta de substituição. Note que para ficheiros que não são colocados
na pasta de substituição, o ficheiro original é usado.

!!! caution

    Para qualquer personalização, tenha em mente que com uma nova versão da pygeoapi as alterações nos templates padrão não estão automaticamente
    disponíveis nos ficheiros substituídos. As atualizações precisam de ser cuidadosamente testadas e validadas.

## Jinja2

[Jinja2](https://jinja.palletsprojects.com) é um conceito de templating comum na comunidade Python. Com um conhecimento mínimo
de HTML será capaz de fazer personalizações menores mas significativas. No núcleo da configuração de template da pygeoapi está o
template [`_base.html`](https://github.com/geopython/pygeoapi/blob/master/pygeoapi/templates/_base.html), que define o
cabeçalho e rodapé da página. O fragmento abaixo define o rodapé da página, note os parâmetros em chavetas,
que são substituídos por conteúdo dinâmico. 

``` {.html linenums="1"}
 <footer class="sticky">
    {% trans %}Powered by {% endtrans %} 
    <a title="pygeoapi" href="https://pygeoapi.io">
        <img src="{{ config['server']['url'] }}/assets/images/pygeoapi.png" title="pygeoapi logo" style="height:24px;vertical-align: middle;"/></a> 
    {{ version }}
</footer>
```

!!! help "Personalizar uma página HTML"

    Copie `_base.html` para uma pasta separada. Ajuste alguns elementos nessa página (por exemplo, imagem do logótipo). Depois, inclua uma referência à nova pasta na
    configuração da pygeoapi. Reinicie o serviço. Verifique o resultado.

## Personalizações CSS

A partir do template HTML personalizado pode referenciar um novo ficheiro de folha de estilo com personalizações ou adicionar diretamente as suas personalizações ao [/static/css/default.css](https://github.com/geopython/pygeoapi/blob/master/pygeoapi/static/css/default.css).

# Resumo

Parabéns! Adicionou um aspeto e comportamento personalizados à sua implementação pygeoapi.