---
title: "Rest Client Reactive"
excerpt: "cannot set homeVerifier"
last_modified_at: 2023-08-02T22:07:00-07:00
tags: 
  - quarkus
toc: true
---

## Quarkus
### Rest Client Reactive
#### HomeVerifier
Quarkus's rest client reactive cannot set homeVerifier (issue #111). We set quarkus.tls.trust-all=true for now

#### @Inject @RestClient
java.lang.RuntimeException - Error injecting com.test.common.security.keycloak.KeycloakRestClient com.test.common.security.keycloak.KeycloakService.keycloakClient

### AuthenticationResponse
```
// responses declaration
message AuthenticationResponse{
  string accessToken = 1;
  sint64 expiresIn = 2;
  sint64 refreshExpiresIn = 3;
  string refreshToken = 4;
  string tokenType = 5;
  bool notBeforePolicy = 6;
  string sessionState = 7;
  string scope = 8;
  string tenantId = 9;
}
```
  
## Task Lists
- [x] Fix GitHub Actions build-test
- [ ] Implement Oidc (Keycloak) GRPC Impl tests
- [ ] Implement Oidc (Keycloak) Dart tests