---
title: "Working on Authentication against keycloak"
excerpt: "Authentication through GRPC service which tunnels to keycloak"
last_modified_at: 2023-08-06T22:07:00-07:00
tags: 
  - dart
toc: true
---

## Dart

### lib/auth/client_authentication_service.dart
```
import 'dart:async';
import 'dart:io';
import 'package:grpc/grpc.dart';
import 'package:grpc/grpc_or_grpcweb.dart';
import 'package:dart/grpc_stub/authenticationService.pbgrpc.dart';

AuthenticationServiceClient? _client;

Future\<String> getAccessToken(
    bool adminMode, GrpcOrGrpcWebClientChannel clientChannel) async {
  _client = AuthenticationServiceClient(clientChannel);
  final AuthenticationRequest request = AuthenticationRequest.create()
    ..userId = (adminMode) ? "admin" : "alice"
    ..password = (adminMode) ? "admin" : "alice"
    ..tenantId = "quarkus";
  final response = await _client!.authenticate(request);
  return Future.value(response.accessToken);
}
```

### Dart Test Results
```
❌ test/0000_binaryObjectEntityNameServiceTest.dart: BinaryObjectEntityNameService read (failed)
  gRPC Error (code: 2, codeName: UNKNOWN, message: io.quarkus.security.AuthenticationFailedException - Failed to parse authentication data, details: [], rawResponse: null, trailers: {})
```

### Logs from module1
```
Caused by: org.jose4j.lang.UnresolvableKeyException: SRJWT07003: Failed to load a key from https://keycloak-service:7443/realms/quarkus/protocol/openid-connect/certs
	at io.smallrye.jwt.auth.principal.AbstractKeyLocationResolver.reportLoadKeyException(AbstractKeyLocationResolver.java:217)
	at io.smallrye.jwt.auth.principal.KeyLocationResolver.<init>(KeyLocationResolver.java:47)
	at io.smallrye.jwt.auth.principal.DefaultJWTTokenParser.getVerificationKeyResolver(DefaultJWTTokenParser.java:266)
	at io.smallrye.jwt.auth.principal.DefaultJWTTokenParser.parseClaims(DefaultJWTTokenParser.java:111)
	... 30 more
Caused by: javax.net.ssl.SSLHandshakeException: PKIX path building failed: sun.security.provider.certpath.SunCertPathBuilderException: unable to find valid certification path to requested target
```

### Add env QUARKUS_TLS_TRUST_ALL=true
kubectl create configmap testsuite-oidc-configmap --from-literal=QUARKUS_OIDC_AUTH_SERVER_URL=https://keycloak-service:7443/realms/quarkus --from-literal=MP_JWT_VERIFY_PUBLICKEY_LOCATION=https://keycloak-service:7443/realms/quarkus/protocol/openid-connect/certs --from-literal=MP_JWT_VERIFY_ISSUER=https://keycloak-service:7443/realms/quarkus --from-literal=QUARKUS_TLS_TRUST_ALL=true

## Task Lists
- [x] Implement Oidc (Keycloak) GRPC Impl tests
- [ ] Implement Oidc (Keycloak) Dart tests
- [ ] Generate Quarkus Configuration
