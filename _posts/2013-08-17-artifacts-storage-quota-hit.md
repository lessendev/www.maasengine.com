---
title: "Frustated with GitHub Actions: Artifact storage quota has been hit"
excerpt: "Post displaying the various ways of highlighting code in Markdown."
last_modified_at: 2018-01-03T09:45:06-05:00
categories:
  - diary
tags:
  - github actions
  - ddlcodegen-main
toc: true
---

## ddlcodgen-main
Today I tried to fix the failed github actions.  I added cleaar() method in IParserListener to clear all table groups after parsing each directory. Otherwise, it will report duplicated table REFDOCSTATUS which appears in both module1 and module2.

## GitHub Actions

### Artifact Storage Quota Has Been Hit
Also all upload artifact actions are failed with error "Artifact storage quota has been hit. Unable to upload any new artifacts". So I added a new Delete workflow runs in the beginning of action.

Ended up that I couldn't fix it. The quota is monthly limit. Since I hit it, it cannot be undo. Just have to wait another month. Duh...
Maybe I should start my self-hosted runner. Hmmmm.

### Create Module 1 DB Failed

```
Run kubectl exec -it pod/mariadb-sts-0 -- mysql -uroot -p*** -e "CREATE DATABASE ***;CREATE USER *** IDENTIFIED BY '***';GRANT ALL PRIVILEGES ON ***.* TO '***'@'%' ;"
  kubectl exec -it pod/mariadb-sts-0 -- mysql -uroot -p*** -e "CREATE DATABASE ***;CREATE USER *** IDENTIFIED BY '***';GRANT ALL PRIVILEGES ON ***.* TO '***'@'%' ;"
  shell: /usr/bin/bash -e {0}
  env:
    JAVA_HOME: /opt/hostedtoolcache/Java_Temurin-Hotspot_jdk/17.0.7-7/x64
    JAVA_HOME_17_X64: /opt/hostedtoolcache/Java_Temurin-Hotspot_jdk/17.0.7-7/x64
    GRADLE_BUILD_ACTION_SETUP_COMPLETED: true
    GRADLE_BUILD_ACTION_CACHE_RESTORED: true
    PUB_CACHE: /home/runner/.pub-cache
    RELEASE_VERSION: v23.29.1-alpha
Unable to use a TTY - input is not a terminal or the right kind of file
OCI runtime exec failed: exec failed: unable to start container process: exec: "mysql": executable file not found in $PATH: unknown
command terminated with exit code 126
Error: Process completed with exit code 126.
```
Let's fix it tomorrow.

## Task Lists

- [ ] Fix GitHub Actions build-test
- [ ] Implement Oidc (Keycloak) GRPC Impl tests
- [ ] Implement Oidc (Keycloak) Dart tests