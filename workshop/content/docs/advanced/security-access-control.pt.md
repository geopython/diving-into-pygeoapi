---
title: Segurança e controlo de acessos
---

# Segurança e controlo de acessos

## Visão geral

A segurança em geral é um tópico amplo e complexo, afetando todo o ciclo de vida de desenvolvimento.
É recomendado seguir as melhores práticas de segurança durante todas as fases de desenvolvimento como design, codificação e implementação.
Nesta workshop focaremos apenas na segurança de API e controlo de acessos, em vez da gama completa de tópicos de segurança de aplicações.

## Segurança de API

A segurança de API é todo o processo para proteger APIs de ataques. É parte das diretrizes de segurança mais gerais que são tratadas no documento OWASP Top Ten. Portanto, essas recomendações ainda se aplicam.

!!! Note "Segurança de Aplicações"

    O documento [Top Ten](https://owasp.org/www-project-top-ten/) do Open Web Application Security Project (OWASP) é uma ferramenta muito boa para garantir o mínimo contra os riscos de segurança e gerir ameaças críticas que mais provavelmente afetam o seu código.

## Controlo de acessos

O controlo de acessos é outra parte fundamental do Open Web Application Security Project e aborda a Gestão de Identidade e Acesso (IAM) de uma API.
A IAM consiste em duas partes diferentes de um fluxo de segurança:

- *Autenticação* (AuthN) verifica a identidade do utilizador para permitir ou negar acesso subsequente (ver próximo)
- *Autorização* (AuthZ) valida permissões de um utilizador (identidade) para aceder a um recurso. As permissões dessa identidade são verificadas contra as políticas de um recurso para (des)permitir acesso a, por exemplo, (partes de) uma API.

Estas partes são geralmente geridas por infraestruturas e soluções dedicadas que geralmente fornecem a maioria dos requisitos de segurança out-of-the-box.

!!! Note "Especificação de Segurança OpenAPI"

    A especificação OpenAPI tem elementos muito bem definidos para programadores e adotantes. Os mais relevantes são:

    - [Security Scheme Object](https://swagger.io/specification/#security-scheme-object) define os esquemas de segurança que podem ser usados pelas operações. Os esquemas suportados são *Autenticação HTTP*, uma *Chave API*, fluxos *OAuth2* e *OpenID Connect*.
    - [Security Requirement Object](https://swagger.io/specification/#security-requirement-object) define a lista de esquemas de segurança necessários para executar uma operação.

## Considerações da pygeoapi

A pygeoapi ainda não suporta elementos de segurança OpenAPI. A implementação futura poderia incluir geração do documento OpenAPI da pygeoapi com uma configuração de segurança, ou gerar a partir de uma aplicação/solução de controlo de acessos conhecida (como [fastgeoapi](https://github.com/geobeyond/fastgeoapi) ou [pygeoapi-auth](https://github.com/cartologic/pygeoapi-auth)).

A implementação direta de controlo de acessos não está no âmbito da pygeoapi. A abordagem desejada aqui seria aproveitar uma solução existente e definir/integrar os endpoints seguros de acordo. Por exemplo, fastgeoapi ou pygeoapi-auth poderiam ser implementados downstream da pygeoapi, e governar o acesso a endpoints específicos (coleções, itens, etc.).


!!! Note

    A [documentação oficial da pygeoapi](https://docs.pygeoapi.io/en/latest/security.html) fornece o estado oficial do projeto sobre atualizações de implementação de segurança, e deve ser visitada para se manter atualizado com o estado mais recente da implementação de segurança no projeto.