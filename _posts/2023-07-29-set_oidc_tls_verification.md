---
title: "Back to Keycloak with TLS"
excerpt: "QUARKUS_OIDC_TLS_VERIFICATION=none"
last_modified_at: 2023-07-30T04:37:00-07:00
tags: 
  - keycloak
  - quarkus
toc: true
---

## Keycloak
I change my mind. I switch back to run keycloak on https instead of http because it is more secure for production. But the problem is the certification is for bizvelocity.biz but the keycloak-service is the name of node in kubernetes cluster. Here is my solution from Chat-GPT. By adding the environment QUARKUS_OIDC_TLS_VERIFICATION=none (or quarkus.oidc.tls.verification in application.properties), it does not complain anymore. I set this value in testsuite-oidc-configmap. 


## Task Lists
- [x] Fix GitHub Actions build-test
- [ ] Implement Oidc (Keycloak) GRPC Impl tests
- [ ] Implement Oidc (Keycloak) Dart tests