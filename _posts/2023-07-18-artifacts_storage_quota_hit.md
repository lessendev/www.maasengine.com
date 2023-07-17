---
title: "Frustated with GitHub Actions: Artifact storage quota has been hit"
categories:
  - diary
tags:
  - github actions
  - ddlcodegen-main
---

Today I tried to fix the failed github actions.  I added cleaar() method in IParserListener to clear all table groups after parsing each directory. Otherwise, it will report duplicated table REFDOCSTATUS which appears in both module1 and module2.

Also all upload artifact actions are failed with error "Artifact storage quota has been hit. Unable to upload any new artifacts". So I added a new Delete workflow runs in the beginning of action.

Ended up that I couldn't fix it. The quota is monthly limit. Since I hit it, it cannot be undo. Just have to wait another month. Duh...

Maybe I should start my self-hosted runner. Hmmmm.

### Task Lists

- [x] Fix GitHub Actions build-test
- [ ] Implement Oidc (Keycloak) GRPC Impl tests
- [ ] Implement Oidc (Keycloak) Dart tests