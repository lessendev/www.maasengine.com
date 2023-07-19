---
title: "'quarkus.oidc.credentials.secret' and 'quarkus.oidc.credentials.client-secret' properties are mutually exclusive"
excerpt: "Secret must be secret."
last_modified_at: 2023-07-18T09:45:06-07:00
tags: 
  - github actions
  - kubernetes
toc: true
---

## GitHub Actions
### Wait For Module 1
Yesterday after I added these in application.properties

```
%prod.quarkus.oidc.auth-server-url=https://bizvelocity.biz:8443/realms/quarkus
%prod.quarkus.oidc.client-id=backend-service
%prod.quarkus.oidc.credentials.client-secret.value=secret
```

The quarkus logs complained
> 'quarkus.oidc.credentials.secret' and 'quarkus.oidc.credentials.client-secret' properties are mutually exclusive

So I guess this cannot be specified explicitly and have to be in environment variables.

## Kubernetes
Add resources' limit and request to all scripts and application.properties for Envoy side car.


## Task Lists

- [ ] Fix GitHub Actions build-test
- [ ] Implement Oidc (Keycloak) GRPC Impl tests
- [ ] Implement Oidc (Keycloak) Dart tests