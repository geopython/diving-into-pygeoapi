---
title: Security and access control
---

# Security and access control

## Overview

Security in general is a broad and complex topic, affecting the entire development lifecycle.
It is recommended to follow security best practices during all development phases like design, coding and deployment.
In this workshop we will focus only on API security and access control, rather than the full range of application security topics.

## API security

API security is the whole process to protect APIs from attacks. It is part of the more general security guidelines that are being treated in the OWASP Top Ten document. So those recommendations still apply.

!!! Note "Application Security"

    The Open Web Application Security Project (OWASP) [Top Ten document](https://owasp.org/www-project-top-ten/) is a very good tool to ensure the bare minimum against the security risks and manage critical treats that are most likely affecting your code.

## Access control

Access control is another fundamental part of the Open Web Application Security Project and addresses the Identity and Access Management (IAM) of an API.
IAM consists of two different parts of a security flow:

- *Authentication* (AuthN) verifies the user's identity in order to allow or deny subsequent access (see next)
- *Authorization* (AuthZ) validates permissions of a user (identity) to access a resource. The permissions of that identity are checked against a resource's policies to (dis)allow access to, for example, (parts of) an API.

These parts are usually managed by dedicated infrastructures and solutions which usually provide most of the security requirements out-of-the-box.

!!! Note "OpenAPI Security Specification"

    The OpenAPI specification has very well-defined elements for developers and adopters. The most relevant are:

    - [Security Scheme Object](https://swagger.io/specification/#security-scheme-object) defines the security schemes that can be used by the operations. Supported schemes are *HTTP Authentication*, an *API Key*, *OAuth2*'s flows and *OpenID Connect*.
    - [Security Requirement Object](https://swagger.io/specification/#security-requirement-object) defines the list of required security schemes to execute an operation.

## pygeoapi considerations

pygeoapi does not yet support OpenAPI security elements.  Future implementation could include generation of pygeoapi's OpenAPI document with a security configuration, or to generate from a known access control solution/application (such as [fastgeoapi](https://github.com/geobeyond/fastgeoapi) or [pygeoapi-auth](https://github.com/cartologic/pygeoapi-auth)).

Direct access control implementation is not in scope for pygeoapi.  The desired approach here would be to leverage an existing solution and define/integrate the secured endpoints accordingly.  For example, fastgeoapi or pygeoapi-auth could be deployed downstream of pygeoapi, and govern access to specific endpoints (collections, items, etc.).


!!! Note

    The [pygeoapi official documentation](https://docs.pygeoapi.io/en/latest/security.html) provides the project's official status on security implementation updates, and should be visited to keep up to date with the latest status on security implementation in the project.
