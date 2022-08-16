---
title: Security and access control
---

# Security and access control

Security is a very huge and complex topic and we do not pretend to cover it in this workshop. It affects the entire development lifecycle and it is recommended to follow security best practices during all the phases like design, development, deployment.
We want to focus on API security and access control rather than the wide  topic about application security risks.

!!! Note "Application Security"

    The Open Web Application Security Project (OWASP) [Top Ten document](https://owasp.org/www-project-top-ten/) is a very good tool to ensure the bare minimum against the security risks and manage critical treats that are most likely affecting your code.

API Security is the whole process to protect APIs from attacks. It is part of the more general security guidelines that are being treated in the OWASP Top Ten document. So those recommendations still apply.

Access control is another fundamental part of the Open Web Application Security Project and addresses the Identity and Access Management (IAM) of an API.
IAM consists of two different parts of a security flow:

- *Authentication* (AuthN) that verifies the user identity. The identity is checked to recognize the user in order to allowing or denying the access
- *Authorization* (AuthZ) that gives user's permissions to access a resource. The user grants of an identity are checked against the resource's policies to permit the consumption of a API.

These parts are usually managed by dedicated infrastructures and solutions which usually provide most of the security requirements out-of-the-box.

!!! Note "OpenAPI Security Specification"

    The OpenAPI specification has very well-defined elements for developers and adopters. The most relevant are:
        - [Security Scheme Object](https://swagger.io/specification/#security-scheme-object) defines the security schemes that can be used by the operations. Supported schemes are *HTTP Authentication*, an *API Key*, *OAuth2*'s flows and *OpenID Connect*.
        - [Security Requirement Object](https://swagger.io/specification/#security-requirement-object) defines the list of required security schemes to executean operation.

