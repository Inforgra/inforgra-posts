---
published: false
title: Dart/Flutter 에서 JSON 다루기
summary: 
date: 2024-08-22 00:00:00.0+09:00
image: none.png
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

먼저 이 글에서 자주 사용하는 용어 몇 개를 살펴보겠습니다.

-   encode : 읽을 수 있는 데이터를 저장 가능한 형식으로 변환 (변환, Serialize)
-   decode : 저장 한 데이터를 읽을 수 있는 형식으로 변환 (역변환, Unserialize)
-   codec  : 데이터를 변환, 역변환 하기 위한 방법

예를 들어 JSON codec 이 있다고 가정해 보겠습니다.
JSON codec 은 임의의 데이터를 JSON 포맷의 텍스트로 변환하거나 역변환 하기 위한 방법을 제공합니다.
encode 는 데이터를 JSON 포맷의 텍스트로 변환합니다.
decode 는 반대로 JSON 포맷의 텍스트를 데이터로 변환합니다.


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

JSON 처리를 위한 빌더를 제공합니다. 이 빌더는 encode, decode 하기 위한 코드를 자동으로 생성합니다.
Model class 를 자료구조로 사용하며, 빌더가 알 수 있도록 `JsonSerializable()` 어노테이션을 추가합니다.

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

`*.g.dart` 파일이 없다면, 다음 명령을 실행하여 생성합니다.

```bash
dart run bulid_runner build
```

만약 `build_runner` 가 없다면, 다음 명령을 실행하여 의존성을 추가합니다.

```bash
dart pub add build_runner --dev
```


### JSON


## 참고

<https://api.flutter.dev/flutter/dart-convert/JsonDecoder-class.html>

