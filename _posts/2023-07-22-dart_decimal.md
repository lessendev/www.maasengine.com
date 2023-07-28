---
title: "Decimal Solved"
excerpt: "Git Pull local to get the latest."
last_modified_at: 2023-07-19T04:37:00-07:00
tags: 
  - keycloak
  - quarkus
  - dart
toc: true
---

## Dart
### Dart Test Module 1 GRPC Failed
I have run the test locally and it works fine. No error.
So I suspect it might be the different version between the one in github actions and local.

```
  - name: Show content of generated decimal.pb.dart
    working-directory: ddlcodegen-test/module1/build/ddlcodegen-src/test/dart/lib/grpc_stub/google/type/
    run:  cat decimal.pb.dart
```
Let's compare 

File from GitHub
```
//
//  Generated code. Do not modify.
//  source: google/type/decimal.proto
//
// @dart = 2.12
// ignore_for_file: annotate_overrides, camel_case_types
// ignore_for_file: constant_identifier_names, library_prefixes
// ignore_for_file: non_constant_identifier_names, prefer_final_fields
// ignore_for_file: unnecessary_import, unnecessary_this, unused_import
import 'dart:core' as $core;
import 'package:protobuf/protobuf.dart' as $pb;

class Decimal extends $pb.GeneratedMessage {
  factory Decimal() => create();
  Decimal._() : super();
  factory Decimal.fromBuffer($core.List<$core.int> i,
          [$pb.ExtensionRegistry r = $pb.ExtensionRegistry.EMPTY]) =>
      create()..mergeFromBuffer(i, r);
  factory Decimal.fromJson($core.String i,
          [$pb.ExtensionRegistry r = $pb.ExtensionRegistry.EMPTY]) =>
      create()..mergeFromJson(i, r);
  static final $pb.BuilderInfo _i = $pb.BuilderInfo(
      _omitMessageNames ? '' : 'Decimal',
      package: const $pb.PackageName(_omitMessageNames ? '' : 'google.type'),
      createEmptyInstance: create)
    ..aOS(1, _omitFieldNames ? '' : 'value')
    ..hasRequiredFields = false;
  @$core.Deprecated('Using this can add significant overhead to your binary. '
      'Use [GeneratedMessageGenericExtensions.deepCopy] instead. '
      'Will be removed in next major version')
  Decimal clone() => Decimal()..mergeFromMessage(this);
  @$core.Deprecated('Using this can add significant overhead to your binary. '
      'Use [GeneratedMessageGenericExtensions.rebuild] instead. '
      'Will be removed in next major version')
  Decimal copyWith(void Function(Decimal) updates) =>
      super.copyWith((message) => updates(message as Decimal)) as Decimal;
  $pb.BuilderInfo get info_ => _i;
  @$core.pragma('dart2js:noInline')
  static Decimal create() => Decimal._();
  Decimal createEmptyInstance() => create();
  static $pb.PbList<Decimal> createRepeated() => $pb.PbList<Decimal>();
  @$core.pragma('dart2js:noInline')
  static Decimal getDefault() =>
      _defaultInstance ??= $pb.GeneratedMessage.$_defaultFor<Decimal>(create);
  static Decimal? _defaultInstance;
  @$pb.TagNumber(1)
  $core.String get value => $_getSZ(0);
  @$pb.TagNumber(1)
  set value($core.String v) {
    $_setString(0, v);
  }

  @$pb.TagNumber(1)
  $core.bool hasValue() => $_has(0);
  @$pb.TagNumber(1)
  void clearValue() => clearField(1);
}

const _omitFieldNames = $core.bool.fromEnvironment('protobuf.omit_field_names');
const _omitMessageNames =
    $core.bool.fromEnvironment('protobuf.omit_message_names');

```
and File from Local

```
///
//  Generated code. Do not modify.
//  source: google/type/decimal.proto
//
// @dart = 2.12
// ignore_for_file: annotate_overrides,camel_case_types,constant_identifier_names,directives_ordering,library_prefixes,non_constant_identifier_names,prefer_final_fields,return_of_invalid_type,unnecessary_const,unnecessary_import,unnecessary_this,unused_import,unused_shown_name



import 'dart:core' as $core;
import 'package:protobuf/protobuf.dart' as $pb;
class Decimal extends $pb.GeneratedMessage {
  static final $pb.BuilderInfo _i = $pb.BuilderInfo(const $core.bool.fromEnvironment('protobuf.omit_message_names') ? '' : 'Decimal', package: const $pb.PackageName(const $core.bool.fromEnvironment('protobuf.omit_message_names') ? '' : 'google.type'), createEmptyInstance: create)
    ..aOS(1, const $core.bool.fromEnvironment('protobuf.omit_field_names') ? '' : 'value')
    ..hasRequiredFields = false
  ;
  Decimal._() : super();
  factory Decimal({
    $core.String? value,
  }) {
    final _result = create();
    if (value != null) {
      _result.value = value;
    }
    return _result;
  }
  factory Decimal.fromBuffer($core.List<$core.int> i, [$pb.ExtensionRegistry r = $pb.ExtensionRegistry.EMPTY]) => create()..mergeFromBuffer(i, r);
  factory Decimal.fromJson($core.String i, [$pb.ExtensionRegistry r = $pb.ExtensionRegistry.EMPTY]) => create()..mergeFromJson(i, r);
  @$core.Deprecated(
  'Using this can add significant overhead to your binary. '
  'Use [GeneratedMessageGenericExtensions.deepCopy] instead. '
  'Will be removed in next major version')
  Decimal clone() => Decimal()..mergeFromMessage(this);
  @$core.Deprecated(
  'Using this can add significant overhead to your binary. '
  'Use [GeneratedMessageGenericExtensions.rebuild] instead. '
  'Will be removed in next major version')
  Decimal copyWith(void Function(Decimal) updates) => super.copyWith((message) => updates(message as Decimal)) as Decimal; // ignore: deprecated_member_use
  $pb.BuilderInfo get info_ => _i;
  @$core.pragma('dart2js:noInline')
  static Decimal create() => Decimal._();
  Decimal createEmptyInstance() => create();
  static $pb.PbList<Decimal> createRepeated() => $pb.PbList<Decimal>();
  @$core.pragma('dart2js:noInline')
  static Decimal getDefault() => _defaultInstance ??= $pb.GeneratedMessage.$_defaultFor<Decimal>(create);
  static Decimal? _defaultInstance;

  @$pb.TagNumber(1)
  $core.String get value => $_getSZ(0);
  @$pb.TagNumber(1)
  set value($core.String v) { $_setString(0, v); }
  @$pb.TagNumber(1)
  $core.bool hasValue() => $_has(0);
  @$pb.TagNumber(1)
  void clearValue() => clearField(1);
}
```
I re-git clone the googleapi on local again and run protocMore again the file in local is updated as GitHub. After consulting Chat-GPT and trying on local, I have to use (Decimal()..value = \"1\") to represent BigDecimal. So I modified ddlcodegen-main/src/main/resources/st4/dart/service/javaType.stg as:

```
mapJavaInit(javaType) ::= <%<javaTypeInitMap.(javaType)>%>

javaTypeInitMap ::= [
	"int":"1",
	"long":"1",
	"float":"1.0f",
	"double":"1.0",
	"boolean":"false",
	"byte":"1",
	"short":"1",
	"char":"1",
    "java.math.BigDecimal":"(Decimal()..value = \"1\")",
    "java.sql.Date":"Timestamp.create()",
    "java.sql.Timestamp":"Timestamp.create()"
]
```

## Quarkus
### Wait for Module2
Error shown in log:

```
INFO: HHH10005004: Stopping BeanContainer : %s
'quarkus.oidc.auth-server-url' property must be configured
```

so I forgot to add these lines in application.properties:
```
quarkus.kubernetes.env.configmaps=testsuite-kafka-configmap,testsuite-oidc-configmap
quarkus.kubernetes.env.secrets=testsuite-oidc-secret,testsuite-module2-db-secret
```

## Task Lists

- [x] Fix GitHub Actions build-test
- [ ] Implement Oidc (Keycloak) GRPC Impl tests
- [ ] Implement Oidc (Keycloak) Dart tests