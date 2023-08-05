---
title: "Microprofile JWT: Claim tenantId is missing."
excerpt: "After authenticated, the claim 'tenantId' is not injected even though the payload do has claim."
last_modified_at: 2023-08-06T22:07:00-07:00
tags: 
  - quarkus
toc: true
---

## Quarkus
### Microprofile JWT
```
  @Inject JsonWebToken jwtToken;

  @Inject
  @Claim("tenantId")
  String tenantId;
```
I found the jwtToken and tenantId are null.

Now it failed at Module2 test since all Authentication stuffs is hard coded in module1 and NOT generated yet. 

### Explore Seucrity Jwt Quickstart
testing on https://quarkus.io/guides/security-jwt

## Task Lists
- [x] Fix GitHub Actions build-test
- [ ] Implement Oidc (Keycloak) GRPC Impl tests
- [ ] Implement Oidc (Keycloak) Dart tests