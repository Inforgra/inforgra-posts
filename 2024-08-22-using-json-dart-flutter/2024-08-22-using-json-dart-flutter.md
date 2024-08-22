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
Dart / Flutter 에서는 대략 3가지 방식으로 JSON 을 다루는 방법이 있으며, 각각 살펴보도록 하겠습니다.

먼저 몇 가지 용어를 정리해 보겠습니다.

-   encode : 원본 데이터를 특정 형태(또는 포맷)로 변환
-   decode : 특정 형태의 데이터를 원본 데이터로 변환
-   codec  : encode / decode 셋

예를 들어 JSON codec 이 있다고 가정해 보겠습니다.
encode 는 자료구조를 json 형식으로 저장하는 역활을 합니다.
decode 는 반대로 json 형식을 가진 텍스트를 자료구조로 변환합니다.


## dart::convert

Dart 에서 제공하는 기본 라이브러리로 다양한 codec 이 있습니다.
JSON 을 위한 `JsonDecoder`, `JsonEncoder` 클래스를 제공합니다.
List, Map 자료 구조를 사용하기 때문에 쉽게 적용할 수 있습니다.


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


## 참고

-   [dart:convert](https://dart.dev/libraries/dart-convert)
-   

<https://api.flutter.dev/flutter/dart-convert/JsonDecoder-class.html>

