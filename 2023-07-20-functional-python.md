---
title: Python 에서 함수형(functional) 스타일로 코드를 작성하기
description: 명령형 언어인 Python 은 `lambda`를 통하여 함수형 스타일의 코딩을 지원한다. 어떤 경우에 사용하면 좋을지 알아 보도록 하자.
author: Kukjin Kang
date: 2023-07-25
tags: ["Python", "함수형스타일", "lambda", "currying"]
image:
---

함수형 언어(functional language)는 명령형 언어(imperative language)와
다른 특징을 가지고 있다. 순수함수(pure function),
투명성(transparency), 불변성(immutability), 1급 객체(first class
oject), 고차함수(high order function) 등의 용어로 특징을 정의한다.

최근 만들어지거나 발전되는 언어들은 고차함수, 1급객체 등의 특징을
포함하는 경우가 많다. Python, Javascript, Typescript 등 한번씩은
경험해 본 언어들에는 대부분은 포함되어 있으며, 나도 모르게 쓰고 있을
수도 있다.

이런 특징들은 높은 추상화를 제공하며, 함수단위의 재사용성이 수월하며,
가독성이 높고, 결과를 예측하기가 쉬워진다. 다만 이렇게 코드를 작성하기
위해서는 많은 노력(특히 사고방식을 바꿔야 한다.)이 필요하다.

Python 에서 함수형 스타일로 개발하기 위해 필요한 몇 가지 개념들과
방법들을 설명한다. 보다 쉽게 접근 할 수 있도록 예제 중심으로 진행해
보겠다.

## 함수 (function)

Python 에서 함수를 정의하는 방법들을 살펴보고, 커링(Currying)에 대해
알아보도록 한다.

### 함수의 정의 방법

Python 에서 함수는 `def` 또는 `lambda` 를 사용하여 정의할 수
있다. 인수 `x`, `y`를 받아서 더하는 `add` 함수를 정의해 보자.

```python
In [1]: def add(x, y):
   ...:     return x + y
   ...:

In [2]: add(1, 2)
Out[2]: 3
```

`lambda` 를 사용하여 이름이 없는 함수(anonymouse function)으로
정의해보자. `lambda` 키워드와 `:` 사이에는 인수를 정의하고, `:` 뒤에는
필요한 계산식을 적는다.

```python
In [3]: lambda x, y: x + y
Out[3]: <function __main__.<lambda>(x, y)>

In [4]: (lambda x, y: x + y)(1, 2)
Out[4]: 3
```

`lambda` 함수로 정의한 것을 볼 수 있으며, 인수를 넣어 필요한 값을
계산도 할 수 있다. 계산식에는 `a = 1`과 같이 side-effect 를 발생시키는
문법은 쓸 수 없다. 이런 제약이 기존 명령어 언어들을 쓰는 사람들이
어려워 하는 부분이다.

앞에서 이름이 없다고 했지만, 하나의 변수에 배정하여 사용할 수 있다.

```python
In [5]: add = lambda x, y: x + y

In [6]: add(1, 2)
Out[6]: 3
```


### 커링(Currying)은 무었인가

우선 커링(Currying)이라는 용어는 함수형 프로그래밍 언어에 많은 공헌을
한 수학자인 [Haskell Curry](https://ko.wikipedia.org/wiki/%ED%95%B4%EC%8A%A4%EC%BC%88_%EC%BB%A4%EB%A6%AC)의
이름에서 가져왔다.

Currying 은 여러 인수가 있는 함수를 하나의 인수를 가지는 함수의
시퀀스로 변환하는 것이다. 예를 들어 `f(a, b, c, ...)` 와 같은 함수를
`f(a)(b)(c)(...)` 와 같이 변환한다. 이렇게 사용하면 하나의 함수에서
다른 함수를 만들어 낼 수 있다.

인수가 두개인 함수에 하나의 인자를 주면 아래와 같은 에러가 발생한다.

```python
In [7]: add = lambda x, y: x + y

In [8]: add(1)
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
Cell In[8], line 1
----> 1 add(1)

TypeError: <lambda>() missing 1 required positional argument: 'y'
```

`lambda`를 두번 사용하여, 함수를 정의할 수 있다.

```python
In [9]: add = lambda x: lambda y: x + y

In [10]: add(1)
Out[10]: <function __main__.<lambda>.<locals>.<lambda>(y)>

In [11]: add1 = add(1)

In [12]: add1(2)
Out[12]: 3
```

아래와 같이 `functools` 패키지를 사용할 수 있다.

```python
In [14]: import functools

In [15]: def add(x, y):
	...:     return x + y
	...:

In [16]: add1 = functools.partial(add, 1)

In [17]: add1(2)
Out[17]: 3
```

### 고차 함수 (High Order Function)

Python 뿐만 아니라 많은 프로그래밍 언어들에서 채용하여 사용하는
고차함수에 대해 알아보자. 이 함수들은 프로그램 내에서 `for` 나
`while`같이 루프(Loop)를 사용하는 경우 대체하여 사용할 수 있다.

## map

`map` 은 두개의 파라메터를 받는 함수이다. 첫번째 파라메터는 값을
맵핑(변환)하는 함수이고, 두번째는 값을 가지고 있는 리스트이다.

주어진 리스트에 각각 1을 더하는 함수를 적용해보자.

```python
In [18]: add = lambda x: lambda y: x + y

In [19]: add1 = add(1)

In [20]: map(add1, [1, 2, 3, 4, 5])
Out[20]: <map at 0x7f81f2adb730>

In [21]: list(map(add1, [1, 2, 3, 4, 5]))
Out[21]: [2, 3, 4, 5, 6]
```

## filter

`filter`는 두개의 파라메터를 받는다. 첫번째 파라메터는 값을 확인하여
가/부(True/False)를 반환하는 함수이며, 두번째는 값을 가지고 있는
리스트이다.

주어진 리스트에서 짝수만 가져오는 방법이다.

```python
In [22]: even = lambda x: x % 2 == 0

In [23]: filter(even, [1, 2, 3, 4, 5])
Out[23]: <filter at 0x7f81f2c79870>

In [24]: list(filter(even, [1, 2, 3, 4, 5]))
Out[24]: [2, 4]
```

## reduce

`reduce`는 2개 또는 3개의 파라메터를 받을 수 있다. 첫번째 파라메터는
값을 받아 계산하는 함수를 정의하고, 두번째는 값을 가지고 있는
리스트이다 세번째는 초기값을 지정한다.

```python
In [29]: add = lambda x, y: x + y

In [30]: functools.reduce(add, [1, 2, 3, 4, 5])
Out[30]: 15
```

어떻게 계산하는지 살펴보자. `add(add(add(add(1,2), 3), 4), 5)`와 같은
형태로 왼쪽으로 폴딩하여 계산한다.

```python
In [33]: def add(x, y):
	...:     print("add({}, {}) => {}".format(x, y, x + y))
	...:     return x + y
	...:

In [34]: functools.reduce(add, [1, 2, 3, 4, 5])
add(1, 2) => 3
add(3, 3) => 6
add(6, 4) => 10
add(10, 5) => 15
Out[34]: 15
```

초기값을 주면 빈 리스트인 경우를 한번 더 계산한다.

```python
In [35]: functools.reduce(add, [1, 2, 3, 4, 5], 0)
add(0, 1) => 1
add(1, 2) => 3
add(3, 3) => 6
add(6, 4) => 10
add(10, 5) => 15
Out[35]: 15
```

## 함수형 스타일

`map`, `filter`, `reduce` 의 개념만 잘 이해해도 실제 개발에 많은
적용이 가능하다.

## compose

두 개의 함수를 합성해서 사용할 수 있게 한다. 예전 수학시간에 보았던
`합성함수`와 개념이 같다. 우선 함수를 정의해 보자. 함수 f, g 두개를
받고, 값을 인자로 하고, 합성한 결과 `g(f(x))`를 반환하다.

```python
compose = lambda f, g: lambda x: g(f(x))
```

`2(x + 1)` 의 표현은 다음과 같이 할 수 있다. `add`, `product` 함수만
정의하였으며, 이를 재사용하여 표현이 가능하다.

```python
In [26]: product = lambda x: lambda y: x * y

In [27]: compose = lambda f, g: lambda x: g(f(x))

In [28]: add = lambda x: lambda y: x + y

In [29]: add1 = add(1)

In [30]: product = lambda x: lambda y: x * y

In [31]: product2 = product(2)

In [33]: compose(add1, product2)
Out[33]: <function __main__.<lambda>.<locals>.<lambda>(x)>

In [34]: compose(add1, product2)(1)
Out[34]: 4
```

## 예시1

`sqlite` 에서 select를 한 다음 필드명, 값을 가지는 dict 형으로
반환하는 함수를 작성해보자.

최선을 다해 작성하였다. 첫번째 루프에서는 필드명을 가져온다. 두번째
루프에서는 row를 하나씩 빼서, 필드명과 결합하여 dict로
변환한다. headers, rows 는 각각 초기화하는 명령과, new_row라는 변수도
추가되어 있다.

```python
In [62]: def iterative_sytle(cursor):
	...:     cursor.execute(" SELECT * FROM sites ")
	...:     headers = []
	...:     for (desc, *other) in cursor.description:
	...:         headers.append(desc)
	...:     rows = []
	...:     for row in cursor.fetchall():
	...:         new_row = dict(zip(headers, row))
	...:         rows.append(dict(new_row))
	...:     return rows
```

이제 함수형 스타일로 변경해 보자.

`header`를 정의할 때 사용하는 `lambda`는 주어진 값의 첫번째를 가져오는
함수이다. 여기서 `list`를 사용하는 이유는 결과 값이 `iterator` 형태로
저장되어 있기 때문에 한번 풀어주기 위해 사용한다. 만약 이것을 사용하지
않으면 두번째 부터는 header를 사용할 수 없다.

`rows`를 정의할 때 사용하는 내용은 `iterative_style` 함수를 정의할
때와 동일하다.

```python
In [108]: def functional_style(cursor):
	 ...:     cursor.execute(" SELECT * FROM sites ")
	 ...:     header = list(map(lambda x: x[0], cursor.description))
	 ...:     rows = map(lambda row: dict(zip(header, row)), cursor.fetchall())
	 ...:     return rows
```


만약 `map`을 알고 있다면, 코드를 더욱 간결하고, 이해하기 쉽게 작성할
수 있다.

## 예시2

암호를 변경할 때, 이 암호가 적당한 포맷을 가지고 있는지 검증해보는
코드를 작성해 보자. 영문자(대/소문자)와 숫자로만 구성되어 있어야 하며,
적어도 1개 이상의 대문자를 가져야한다.

첫번째 조건에서는 문자가 조건에 포함되지 않으면 False를
반환한다. 두번째 조건에서는 대문자의 개수를 센다. 루프가 끝나면
대문자의 개수를 확인해 True 또는 False를 반환한다.

```python
In [139]: num = list(map(lambda n: chr(n), range(48, 57)))

In [140]: alphaU = list(map(lambda n: chr(n), range(65, 91)))

In [141]: alphaL = list(map(lambda n: chr(n), range(97, 123)))

In [142]: def imperative_style(passwd):
	 ...:     uppercase = 0
	 ...:     for ch in passwd:
	 ...:         if not ch in num + alphaU + alphaL:
	 ...:             return False
	 ...:         if ch in alphaU:
	 ...:             uppercase = uppercase + 1
	 ...:     if uppercase == 0:
	 ...:         return False
	 ...:     return True
```

`isAlNum` 등과 같은 함수들은 side-effect 가 전혀 없는
함수들이기 때문에 전역적으로 선언해도 문제가 없다. 기본적인 계산에
필요한 함수들을 정의하고, 이것을 재사용하여 완성하였다.

```python
In [187]: isAlNum = lambda ch: ch in num + alphaU + alphaL

In [188]: isAlphaU = lambda ch: 1 if ch in alphaU else 0

In [189]: isAllAlNum = lambda xs: map(isAlNum, xs)

In [190]: logicalAndForList = lambda xs: functools.reduce(logicalAnd, xs)

In [191]: logicalAnd = lambda x, y: x and y

In [192]: def functional_style(passwd):
	 ...:     return compose(isAllAlNum, logicalAndForList)(passwd) and sum(map(isAlphaU, passwd)) > 0
	 ...:

In [193]: functional_style('abcde123a')
Out[193]: False

In [194]: functional_style('abcde123A')
Out[194]: True
```

만약 여기에 특수문자 기능을 추가해야하는 상황이 오면 어떨까?
imperative_style 에서는 기존 코드에 변수를 추가하고, 조건문을 추가하는
등 뜯어 고치는 형태로 진행되지만, functional_style 에서는 기본
함수들을 추가하고, 재조합을 하는 형태로 진행된다.

## 참고

- [functools](https://docs.python.org/3/library/functools.html)
- [함수형 프로그래밍이란](https://jongminfire.dev/%ED%95%A8%EC%88%98%ED%98%95-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D%EC%9D%B4%EB%9E%80)
