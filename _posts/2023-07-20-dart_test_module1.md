---
title: "Dart Test Module 1 GRPC Failed"
excerpt: "Decimal() is not the same."
last_modified_at: 2023-07-19T04:37:00-07:00
tags: 
  - keycloak
  - quarkus
  - dart
toc: true
---

## Keycloak
### Move Keycloak port from 8443 to 7443
The github actions is dead at Waiting Keycloak. So I found out I forgot to change the port in keycloak manifest file. This env has to be added to start keycloak https port at 7443.

```
- name: KC_HTTPS_PORT
  value: "7443"
```

## Quarkus
### Missing quarkus.oidc.auth-server-url
```
INFO: HHH10005004: Stopping BeanContainer : %s
'quarkus.oidc.auth-server-url' property must be configured
```
It seems that I didn't run my local build properly. Somehow my kubernetes manifest doesn't have a configMapRef.
```
- configMapRef:
    name: testsuite-oidc-configmap
```
After rerun, it didn't report this error anymore.

### Add startup-probe in application.properties
After run docker manually, locally, it seems the module1 running just fine. So it must be the startup probe that didn't work properly. I added this:

```
quarkus.kubernetes.startup-probe.failure-threshold=3
quarkus.kubernetes.startup-probe.initial-delay=240s
quarkus.kubernetes.startup-probe.period=30s
quarkus.kubernetes.startup-probe.timeout=45s
```
Here is the cure of mysterious death. I tested on local kubernetes and it started now.

## Dart
### Dart Test Module 1 GRPC Failed
Now the github actions failed at "Dart Test Module 1 GRPC".

```
Failed to load "test/0000_purchaseRequestEntityNameServiceTest.dart":
  test/0000_purchaseRequestEntityNameServiceTest.dart:63:28: Error: No named parameter with the name 'value'.
        ..quantity = Decimal(value:"1")
                             ^^^^^
  lib/grpc_stub/google/type/decimal.pb.dart:17:11: Context: Found this candidate, but the arguments don't match.
    factory Decimal() => create();
            ^
  package:test_core/src/runner/vm/platform.dart 252:7   VMPlatform._compileToKernel
  ===== asynchronous gap ===========================
  package:test_core/src/runner/vm/platform.dart 231:13  VMPlatform._spawnIsolate
  ===== asynchronous gap ===========================
  package:test_core/src/runner/vm/platform.dart 76:19   VMPlatform.load
  ===== asynchronous gap ===========================
  package:test_core/src/runner/loader.dart 232:27       Loader.loadFile.<fn>
  ===== asynchronous gap ===========================
  package:test_core/src/runner/load_suite.dart 98:19    new LoadSuite.<fn>.<fn>
```
and 
```
Failed to load "test/9999_orderEntityNameServiceTest.dart":
  test/9999_orderEntityNameServiceTest.dart:51:31: Error: No named parameter with the name 'value'.
        ..totalAmount = Decimal(value:"1");
                                ^^^^^
  lib/grpc_stub/google/type/decimal.pb.dart:17:11: Context: Found this candidate, but the arguments don't match.
    factory Decimal() => create();
            ^
  test/9999_orderEntityNameServiceTest.dart:64:28: Error: No named parameter with the name 'value'.
        ..quantity = Decimal(value:"1")
                             ^^^^^
  lib/grpc_stub/google/type/decimal.pb.dart:17:11: Context: Found this candidate, but the arguments don't match.
    factory Decimal() => create();
            ^
  test/9999_orderEntityNameServiceTest.dart:65:26: Error: No named parameter with the name 'value'.
        ..amount = Decimal(value:"1")
                           ^^^^^
  lib/grpc_stub/google/type/decimal.pb.dart:17:11: Context: Found this candidate, but the arguments don't match.
    factory Decimal() => create();
            ^
  package:test_core/src/runner/vm/platform.dart 252:7   VMPlatform._compileToKernel
  ===== asynchronous gap ===========================
  package:test_core/src/runner/vm/platform.dart 231:13  VMPlatform._spawnIsolate
  ===== asynchronous gap ===========================
  package:test_core/src/runner/vm/platform.dart 76:19   VMPlatform.load
  ===== asynchronous gap ===========================
  package:test_core/src/runner/loader.dart 232:27       Loader.loadFile.<fn>
  ===== asynchronous gap ===========================
  package:test_core/src/runner/load_suite.dart 98:19    new LoadSuite.<fn>.<fn>
  ```
## Task Lists

- [ ] Fix GitHub Actions build-test
- [ ] Implement Oidc (Keycloak) GRPC Impl tests
- [ ] Implement Oidc (Keycloak) Dart tests