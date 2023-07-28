---
title: "Wait for Module1 Failed. Again!!"
excerpt: "missing Quarkus OIDC parameters."
last_modified_at: 2023-07-19T04:37:00-07:00
tags: 
  - github actions
  - keycloak
  - quarkus
  - dart
toc: true
---

## Wait for Module1 Failed

### Add GitHub Action Steps 
It failed on the github but works find on local. So I add steps to always show module 1 logs and describe it after the module 1 waiting.
Also set the TIMEOUT env to 750s

### Quarkus Complained Missing parameters 
I forgot to set secrets ${{secrets.TESTSUITE_OIDC_CLIENT_ID}} and ${{secrets.TESTSUITE_OIDC_CLIENT_SECRET}} 
Also need to change the environment name to QUARKUS_OIDC_CREDENTIALS_SECRET


### Change Keycloak to Cluster IP instead of LoadBalancer
Comment keycloak manifest the LoadBalancer line. So it will be Cluster IP by default.

## Task Lists
- [x] Fix GitHub Actions build-test
- [ ] Implement Oidc (Keycloak) GRPC Impl tests
- [ ] Implement Oidc (Keycloak) Dart tests