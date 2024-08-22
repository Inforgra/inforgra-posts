---
published: true
title: Dart/Flutter 에서 JSON 다루기
summary: 
date: 2024-08-22 00:00:00.0+09:00
image: 
imageAlt: 
tags:
- dart
- flutter
- json
---

어플리케이션을 개발하다 보면 데이터를 JSON 형식을 사용하는 경우가 자주 있습니다.
Dart / Flutter 에서는 3개 정도의 라이브러리를 제공하며, 각각 살펴보도록 하겠습니다.

-   [dart:convert](https://dart.dev/libraries/dart-convert)
-   [package:json<sub>serializable</sub>](https://pub.dev/packages/json_serializable)
-   [package:built<sub>value</sub>](https://pub.dev/packages/built_value)

먼저 몇 가지 용어를 정리해 보겠습니다.

-   encode : 원본 데이터를 특정 형태(또는 포맷)로 변환
-   decode : 특정 형태의 데이터를 원본 데이터로 변환
-   codec  : encode / decode 셋

예를 들어 JSON codec 이 있다고 가정해 보겠습니다.
JSON codec 은 JSON 형식을 가진 텍스트를 자료 변환하거나 역변환하는 방법을 제공합니다.
encode 는 자료구조를 json 형식으로 저장하며,
decode 는 반대로 json 형식을 가진 텍스트를 자료구조로 변환합니다.


## dart::convert

Dart 에서 제공하는 기본 라이브러리로 다양한 codec 이 있습니다.
JSON 을 위한 `JsonDecoder`, `JsonEncoder` 클래스를 제공합니다.
List, Map 자료 구조를 사용하기 다루는 방법이 매우 간단합니다.


### JsonEncoder class

```dart
const JsonEncoder encoder = JsonEncoder();
const data = {'text': 'foo', 'value': '2'};

final String jsonString = encoder.convert(data);
print(jsonString); // {"text":"foo","value":"2"}
```


### JsonDecoder class

```dart
const JsonDecoder decoder = JsonDecoder();

const String jsonString = '''
  {
    "data": [{"text": "foo", "value": 1 },
             {"text": "bar", "value": 2 }],
    "text": "Dart"
  }
''';

final Map<String, dynamic> object = decoder.convert(jsonString);

final item = object['data'][0];
print(item['text']); // foo
print(item['value']); // 1
print(object['text']); // Dart
```


## json<sub>serializable</sub>

JSON 처리를 위한 빌더를 제공합니다. 빌더는 자료구조로 변환하기 위한 코드를 자동으로 생성합니다.

Model class 를 자료구조로 사용하며, 빌더가 알 수 있도록 `@JsonSerializable` 어노테이션을 추가합니다.

`pub` 명령을 사용하여 패키지를 추가합니다.

```bash
$ flutter pub add json_serializable
```

```dart
import 'package:json_annotation/json_annotation.dart';
part 'example.g.dart';

@JsonSerializable()
class Person {
  /// The generated code assumes these values exist in JSON.
  final String firstName, lastName;

  /// The generated code below handles if the corresponding JSON value doesn't
  /// exist or is empty.
  final DateTime? dateOfBirth;

  Person({required this.firstName, required this.lastName, this.dateOfBirth});

  /// Connect the generated [_$PersonFromJson] function to the `fromJson`
  /// factory.
  factory Person.fromJson(Map<String, dynamic> json) => _$PersonFromJson(json);

  /// Connect the generated [_$PersonToJson] function to the `toJson` method.
  Map<String, dynamic> toJson() => _$PersonToJson(this);
}
```


### JSON


## 참고

<https://api.flutter.dev/flutter/dart-convert/JsonDecoder-class.html>

