---
title: "Mysterious death"
excerpt: "module1 deployment just stopped without clue."
last_modified_at: 2023-07-19T04:37:00-07:00
tags: 
  - kubernetes
  - gradle
  - quarkus
toc: true
---

## Kubernetes
### Wait For Module 1

>kubectl describe pod/module1-5989cb84b7-lqd2d
```
Name:             module1-5989cb84b7-lqd2d
Namespace:        default
Priority:         0
Service Account:  default
Node:             minikube/192.168.49.2
Start Time:       Wed, 19 Jul 2023 18:36:54 +0700
Labels:           app.kubernetes.io/managed-by=quarkus
                  app.kubernetes.io/name=module1
                  app.kubernetes.io/version=unspecified
                  pod-template-hash=5989cb84b7
Annotations:      app.quarkus.io/build-timestamp: 2023-07-19 - 10:59:28 +0000
                  app.quarkus.io/commit-id: b3b4b0bb0915f21a53ba08ddeea9dcc4b93a84d1
Status:           Running
IP:               10.244.0.8
IPs:
  IP:           10.244.0.8
Controlled By:  ReplicaSet/module1-5989cb84b7
Containers:
  module1:
    Container ID:   docker://b03cf95dba7627a54667342c03229a77a2afe3e27a5aa502301ee638bab0de39
    Image:          ghcr.io/drsarutobi8/testsuite-module1:latest
    Image ID:       docker-pullable://ghcr.io/drsarutobi8/testsuite-module1@sha256:99929be1bda1358e4b6a32cda527bce3fc4b0d6959cc541eb2c9e70c8fa7d711
    Ports:          8081/TCP, 8443/TCP, 9000/TCP, 9443/TCP, 9900/TCP
    Host Ports:     0/TCP, 0/TCP, 0/TCP, 0/TCP, 0/TCP
    State:          Running
      Started:      Wed, 19 Jul 2023 19:25:17 +0700
    Last State:     Terminated
      Reason:       Error
      Exit Code:    137
      Started:      Wed, 19 Jul 2023 19:24:01 +0700
      Finished:     Wed, 19 Jul 2023 19:25:06 +0700
    Ready:          False
    Restart Count:  17
    Limits:
      cpu:     500m
      memory:  640Mi
    Requests:
      cpu:      250m
      memory:   64Mi
    Liveness:   http-get http://:8081/q/health/live delay=180s timeout=45s period=30s #success=1 #failure=10
    Readiness:  http-get http://:8081/q/health/ready delay=180s timeout=45s period=30s #success=1 #failure=10
    Startup:    http-get http://:8081/q/health/started delay=5s timeout=10s period=10s #success=1 #failure=3
    Environment Variables from:
      testsuite-oidc-secret        Secret     Optional: false
      testsuite-kafka-configmap    ConfigMap  Optional: false
      testsuite-module1-db-secret  Secret     Optional: false
    Environment:
      KUBERNETES_NAMESPACE:                                      default (v1:metadata.namespace)
      QUARKUS_HTTP_PORT:                                         8081
      QUARKUS_GRPC_CLIENTS__DDLCODEGEN_CLIENT__HOST:             bizvelocity.biz
      QUARKUS_GRPC_SERVER_SSL_CERTIFICATE:                       /tls/tls.crt
      QUARKUS_GRPC_CLIENTS__DDLCODEGEN_CLIENT__SSL_TRUST_STORE:  /certs/ca.pem
      QUARKUS_GRPC_SERVER_SSL_KEY:                               /tls/tls.key
    Mounts:
      /certs from testsuite-module1-certs-vol (ro)
      /tls from testsuite-module1-tls-vol (ro)
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-k5l7g (ro)
  envoy:
    Container ID:  docker://1359887eea4bb4a0ec3faaa3b189059e85291fd508e09692b29da7602d93ef20
    Image:         envoyproxy/envoy:v1.24.1
    Image ID:      docker-pullable://envoyproxy/envoy@sha256:1f07971d83e470d6f87583044ddb2b7aefac9ba1f14239479ea936599166f638
    Ports:         9900/TCP, 9443/TCP, 8443/TCP
    Host Ports:    0/TCP, 0/TCP, 0/TCP
    Command:
      /bin/sh
    Args:
      -c
      envoy -c /etc/envoy/envoy.yaml
    State:          Running
      Started:      Wed, 19 Jul 2023 18:37:05 +0700
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /certs from testsuite-module1-certs-vol (ro)
      /etc/envoy from testsuite-module1-envoy-vol (ro)
      /proto from testsuite-module1-proto-vol (ro)
      /tls from testsuite-module1-tls-vol (ro)
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-k5l7g (ro)
Conditions:
  Type              Status
  Initialized       True 
  Ready             False 
  ContainersReady   False 
  PodScheduled      True 
Volumes:
  testsuite-module1-certs-vol:
    Type:      ConfigMap (a volume populated by a ConfigMap)
    Name:      testsuite-module1-certs-configmap
    Optional:  false
  testsuite-module1-envoy-vol:
    Type:      ConfigMap (a volume populated by a ConfigMap)
    Name:      testsuite-module1-envoy-configmap
    Optional:  false
  testsuite-module1-proto-vol:
    Type:      ConfigMap (a volume populated by a ConfigMap)
    Name:      testsuite-module1-proto-configmap
    Optional:  false
  testsuite-module1-tls-vol:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  testsuite-module1-tls-secret
    Optional:    false
  kube-api-access-k5l7g:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    ConfigMapOptional:       <nil>
    DownwardAPI:             true
QoS Class:                   Burstable
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type     Reason     Age                    From               Message
  ----     ------     ----                   ----               -------
  Normal   Scheduled  49m                    default-scheduler  Successfully assigned default/module1-5989cb84b7-lqd2d to minikube
  Normal   Pulled     49m                    kubelet            Successfully pulled image "ghcr.io/drsarutobi8/testsuite-module1:latest" in 1.750137428s (1.750152811s including waiting)
  Normal   Pulled     49m                    kubelet            Container image "envoyproxy/envoy:v1.24.1" already present on machine
  Normal   Created    49m                    kubelet            Created container envoy
  Normal   Started    49m                    kubelet            Started container envoy
  Normal   Pulled     48m                    kubelet            Successfully pulled image "ghcr.io/drsarutobi8/testsuite-module1:latest" in 1.807333881s (1.807671854s including waiting)
  Normal   Killing    47m (x2 over 48m)      kubelet            Container module1 failed startup probe, will be restarted
  Normal   Pulled     47m                    kubelet            Successfully pulled image "ghcr.io/drsarutobi8/testsuite-module1:latest" in 6.684462402s (6.684481326s including waiting)
  Normal   Created    47m (x3 over 49m)      kubelet            Created container module1
  Normal   Started    47m (x3 over 49m)      kubelet            Started container module1
  Normal   Pulling    24m (x11 over 49m)     kubelet            Pulling image "ghcr.io/drsarutobi8/testsuite-module1:latest"
  Warning  Unhealthy  9m27s (x45 over 49m)   kubelet            Startup probe failed: Get "http://10.244.0.8:8081/q/health/started": dial tcp 10.244.0.8:8081: connect: connection refused
  Warning  BackOff    4m29s (x141 over 43m)  kubelet            Back-off restarting failed container module1 in pod module1-5989cb84b7-lqd2d_default(dae6f0f3-c9a7-4030-ac22-d91b9246d4d3)
```

> kubectl logs -f pod/module1-5989cb84b7-lqd2d
```
2023-07-19 11:45:50,562 INFO  [io.quarkus] (main) Profile prod activated. 
2023-07-19 11:45:50,562 INFO  [io.quarkus] (main) Installed features: [cdi, grpc-server, hibernate-orm, hibernate-reactive, hibernate-reactive-panache, hibernate-validator, kafka-client, kubernetes, oidc, oidc-client, oidc-client-reactive-filter, reactive-mysql-client, reactive-routes, rest-client-reactive, resteasy-reactive, security, smallrye-context-propagation, smallrye-health, smallrye-jwt, smallrye-reactive-messaging, smallrye-reactive-messaging-kafka, vertx]
2023-07-19 11:45:53,559 WARN  [org.apa.kaf.cli.con.int.ConsumerCoordinator] (smallrye-kafka-consumer-thread-1) [Consumer clientId=kafka-consumer-warehouse-in, groupId=module1] Close timed out with 2 pending requests to coordinator, terminating client connections
2023-07-19 11:45:55,564 WARN  [org.apa.kaf.cli.con.int.ConsumerCoordinator] (smallrye-kafka-consumer-thread-2) [Consumer clientId=kafka-consumer-binaryObject-in, groupId=module1] Close timed out with 2 pending requests to coordinator, terminating client connections
```

Afer I commented out all probes, it runs fine. **It seems that the startup probe is not working.**

```
startupProbe:
            failureThreshold: 3
            httpGet:
              path: /q/health/started
              port: 8081
              scheme: HTTP
            initialDelaySeconds: 5
            periodSeconds: 10
            successThreshold: 1
            timeoutSeconds: 10
```

## Quarkus
### QuarkusDev is not running.
At the beginner, I could not run as it showed compile errors. The classes in build/ddlcodegen-src were missing from the compile classpath somehow. So I modified the build.gradle.kts from 

```
tasks.withType<JavaCompile> {
    dependsOn("ddlcodegen")
    options.encoding = "UTF-8"
    options.compilerArgs.add("-parameters")
    options.compilerArgs.add("-Xlint:deprecation")
    options.isFork = true
}

tasks.compileJava {
  dependsOn("compileQuarkusGeneratedSourcesJava")
}
```
to

```
tasks.compileJava {
  dependsOn("ddlcodegen")
  dependsOn("compileQuarkusGeneratedSourcesJava")
  options.encoding = "UTF-8"
  options.compilerArgs.add("-parameters")
  options.compilerArgs.add("-Xlint:deprecation")
  options.isFork = true
}
```

It seems to work every other time I ran gradle. Haha
So, I was wrong.

## Gradle
### No-daemon
And the real culprit is the gradle daemon. The fix is simply running with --no-daemon
> gradle --no-daemon :module1:clean :module1:build
> gradle --no-daemon :module1:clean :module1:quarkusDev

and the best way is to put this in gradle.properties
```
org.gradle.daemon=false
```
which I turned it on before.

## Misc
### Move GHCR_TOKEN
GHCR_TOKEN was hardcoded in several files. Yep, I finally moved it to .env file which is ignored by git. It was specified in .gitignore file.
### Move Keycloak port from 8443 to 7443
When I tried to debug locally I found that the port 8443 is conflicted as I already specified port for testsuite-moduleX.

## Task Lists

- [ ] Fix GitHub Actions build-test
- [ ] Implement Oidc (Keycloak) GRPC Impl tests
- [ ] Implement Oidc (Keycloak) Dart tests