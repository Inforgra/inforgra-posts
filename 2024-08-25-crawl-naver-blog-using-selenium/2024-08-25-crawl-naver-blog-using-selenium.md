---
published: true
title: Selenium 을 사용하여 웹 페이지 크롤 하기
summary: 웹 페이지가 동적으로 구성하는 경우 HTTP/S 방식으로 웹 페이지 크롤은 매우 어렵습니다. 이런 경우 Selenium 을 사용하여 수집하는 것이 좋은 대안이 될 수 있습니다.
date: 2024-08-25 00:00:00.0+09:00
image: selenium.png
imageAlt: selenium
tags:
- selenium
- crawl
---

네이버 블로거 들의 평균 방문자 수가 어떤지 궁금했습니다.
네이버 블로그에서 알려주는 데이터는 너무 러프하여 직관적으로 파악하기 어려웠습니다.
그래서 데이터를 수집해 보기로 하였습니다.

네이버 블로그 홈은 페이지를 동적으로 구성합니다.
각 주제에 대한 목록을 미리 불러 오지 않습니다.
사용자가 주제를 클릭할 때 서버로부터 목록을 받아 노출합니다.
또한 네이버 블로그 글은 iframe 으로 감싸져 있습니다.
이런 동적인 요소가 포함되어 있을 때, 단순한  HTTP/S 요청으로 페이지를 수집하는 것은 매우 어렵습니다.

이런 경우 [Selenium](https://www.selenium.dev/) 을 사용하는 것이 좋은 대안이 될 수 있습니다.
웹 브라우저를 사용하기 때문에 동적인 요소를 고민할 필요가 없습니다.


## 사용하는 패키지

이 예제 코드에서 사용하는 패키지는 다음과 같습니다.
코드의 구조화를 위해 `functools`, `itertools` 패키지를 사용합니다.
이 코드를 Ubutnu 환경에서 테스트 하였기 때문에 `Firefox` 를 사용합니다.
Windows, OSX 에서는 Edge, Chrome 사용이 가능합니다.

```python
import functools
import itertools
import time
import traceback
from selenium.webdriver import Firefox
from selenium.webdriver.common.by import By
```


## 기본 함수

크롤을 위해 반드시 필요한 함수를 정의합니다.
6개의 함수만으로도 다양한 웹 페이지에 접근하고, 클릭하고, 저장이 가능합니다.


### sleep

웹 페이지 클릭시 로드가 되기전에 다른 작업을 하게 될 수 있습니다.
이런 경우 충분히 쉬어 줄 수 있도록 `sleep` 함수를 정의합니다.

```python
def sleep(sec, driver):
    time.sleep(sec)
```


### get

브라우저 주소창에 주소를 입력합니다.

```python
def get(url, driver):
    driver.get(url)
```


### click

지정한 요소들을 선택하고, 주어진 텍스트를 가진 요소를 클릭합니다.
요소를 선택하는 방법은 `css selector` 를 사용합니다.
문법은 [CSS Selector Reference](https://www.w3schools.com/cssref/css_selectors.php) 를 참고합니다.

```python
def click(text, selector, driver):
    for x in driver.find_elements(By.CSS_SELECTOR, selector):
        if x.text == text:
            x.click()
            break
```


### click<sub>index</sub>

지정한 요소들을 선택하고, `index` 번째 요소를 클릭합니다.

```python
def click_index(index, selector, driver):
    driver.find_elements(By.CSS_SELECTOR, selector)[index].click()
```


### save<sub>current</sub><sub>page</sub>

현재 페이지를 저장합니다.

```python
def save_current_page(filename, driver):
    with open(filename, 'w') as fp:
        fp.write(driver.page_source)
```


## 자료구조, 빌드 그리고 실행

기본 함수를 실행하는 방법에 대해 고민이 필요합니다.

```python
cmds = [
    [
        [get, 'https://blog.naver.com'],
        [save_current_page, 'crawl-blog-naver-com/home-sec-all-page-1.html'],
        [click_index, 1, '.pagination a'],
        [sleep, 1],
        [save_current_page, 'crawl-blog-naver-com/home-all-page-2.html'],
        [sleep, 1],
        [click_index, 2, '.pagination a'],
        [sleep, 1],
        [save_current_page, 'crawl-blog-naver-com/home-all-page-3.html'],
        [sleep, 1],
    ],
    [
        [click_index, 1, 'div[class*=navigator_category] a'],
        [sleep, 1],
        [save_current_page, 'crawl-blog-naver-com/home-sec-1-page-1.html'],
        [sleep, 1],
        [click_index, 1, '.pagination a'],
        [sleep, 1],
        [save_current_page, 'crawl-blog-naver-com/home-sec-1-page-2.html'],
        [sleep, 1],
        [click_index, 2, '.pagination a'],
        [sleep, 1],
        [save_current_page, 'crawl-blog-naver-com/home-sec-1-page-3.html'],
    ]
]
```


## 빌드, 실행


### build

