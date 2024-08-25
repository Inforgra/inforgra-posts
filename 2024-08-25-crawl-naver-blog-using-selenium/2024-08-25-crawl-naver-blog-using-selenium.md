---
published: true
title: Selenium 을 사용하여 웹 페이지 수집하기
summary: Selenium + Python 조합을 사용하면, 40라인이 안되는 코드로 다양한 웹 페이지를 수집할 수 있습니다.
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
웹 페이지를 수집하기 위한 코드는 40라인이 되지 않을 정도로 라이브러리 구성이 잘 되어 있습니다.
웹 페이지 수집에 관심이 있다면, 한번 따라 해 보시길 바랍니다.


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

크롤을 위해 반드시 필요한 액션을 정의합니다.
단 6개의 액션으로도 다양한 웹 페이지에 접근하고, 클릭하고, 저장이 가능합니다.


### `sleep`

웹 페이지 클릭시 로드가 되기전에 다른 작업을 하게 될 수 있습니다.
이런 경우 충분히 쉬어 줄 수 있도록 `sleep` 함수를 정의합니다.

```python
def sleep(sec, driver):
    time.sleep(sec)
```


### `get`

브라우저 주소창에 주소를 입력합니다.

```python
def get(url, driver):
    driver.get(url)
```


### `click`

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

`click_index`

지정한 요소들을 선택하고, `index` 번째 요소를 클릭합니다.

```python
def click_index(index, selector, driver):
    driver.find_elements(By.CSS_SELECTOR, selector)[index].click()
```

`save_current_page`

현재 페이지를 저장합니다.

```python
def save_current_page(filename, driver):
    with open(filename, 'w') as fp:
        fp.write(driver.page_source)
```


## 빌드와 실행

이제 명령을 어떻게 구성해야 할 지 고민을 해 봐야합니다.
몇 가지 요구사항을 생각해 봅시다.

-   액션은 필요한 인수를 가지고 있어야 합니다
-   그룹을 지어야 할 수 있어야 합니다

그럼 명령을 다음과 같이 내릴 수 있습니다.

```python
[
  [
    [get, 'https://blog.naver.com'],
    [save_current_page, 'crawl-blog-naver-com/home-sec-all-page-1.html'],
    [click_index, 1, '.pagination a'],
    [sleep, 1],
    [save_current_page, 'crawl-blog-naver-com/home-all-page-2.html'],
  ],
]
```


### `build`

앞에서 정의한 명령을 실행 가능한 구조로 변경합니다.

```python
def build(cmds):
    return [ functools.partial(cmd[0], *cmd[1:]) for cmd in itertools.chain(*cmds) ]
```


### `run`

빌드한 명령들을 드라이버 (ex: Firefox, Edge, Chrome) 에 실행합니다.

```python
def run(functions, driver):
    retval = driver
    for f in functions:
        if retval is None:
            return None
        try:
            print("execute: {}".format(f))
            f(driver)
        except Exception as e:
            print(traceback.format_exc())
            retval = None
```


### `execute`

build, run 함수를 각각 실행하지 않고, 한번에 할 수 있도록 조합(compose)하였습니다.

```python
execute = lambda cmds: functools.partial(run, build(cmds))
```


## 실행 결과

웹 페이지를 수집(크롤) 하기 위한 예시입니다.
네이버 블로그 홈을 수집하고, 페이지 이동(1, 2, 3 페이지)하여 수집합니다.
두번째 섹션을 수집하고,  페이지 이동(1, 2, 3 페이지)하여 수집합니다.

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

명령을 실행합니다.

```python
driver = Firefox()
execute(cmds)(driver)
```

결과는 다음과 같습니다. 이를 실행하는 과정을 녹화해 보았습니다.

<center>
<iframe width='560' height='315' src='https://www.youtube.com/embed/dNI_v4dmgZw?si=FipAa52cd_Tvmbw6' title='YouTube video player' frameborder='0' allow='accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share' referrerpolicy='strict-origin-when-cross-origin' allowfullscreen></iframe>
</center>
<br/>

```bash
execute: functools.partial(<function get at 0x7f0c388cf640>, 'https://blog.naver.com')
execute: functools.partial(<function save_current_page at 0x7f0c388cc820>, 'crawl-blog-naver-com/home-sec-all-page-1.html')
execute: functools.partial(<function click_index at 0x7f0c388cd6c0>, 1, '.pagination a')
execute: functools.partial(<function sleep at 0x7f0c3887a320>, 1)
execute: functools.partial(<function save_current_page at 0x7f0c388cc820>, 'crawl-blog-naver-com/home-all-page-2.html')
execute: functools.partial(<function sleep at 0x7f0c3887a320>, 1)
execute: functools.partial(<function click_index at 0x7f0c388cd6c0>, 2, '.pagination a')
execute: functools.partial(<function sleep at 0x7f0c3887a320>, 1)
execute: functools.partial(<function save_current_page at 0x7f0c388cc820>, 'crawl-blog-naver-com/home-all-page-3.html')
execute: functools.partial(<function sleep at 0x7f0c3887a320>, 1)
execute: functools.partial(<function click_index at 0x7f0c388cd6c0>, 1, 'div[class*=navigator_category] a')
execute: functools.partial(<function sleep at 0x7f0c3887a320>, 1)
execute: functools.partial(<function save_current_page at 0x7f0c388cc820>, 'crawl-blog-naver-com/home-sec-1-page-1.html')
execute: functools.partial(<function sleep at 0x7f0c3887a320>, 1)
execute: functools.partial(<function click_index at 0x7f0c388cd6c0>, 1, '.pagination a')
execute: functools.partial(<function sleep at 0x7f0c3887a320>, 1)
execute: functools.partial(<function save_current_page at 0x7f0c388cc820>, 'crawl-blog-naver-com/home-sec-1-page-2.html')
execute: functools.partial(<function sleep at 0x7f0c3887a320>, 1)
execute: functools.partial(<function click_index at 0x7f0c388cd6c0>, 2, '.pagination a')
execute: functools.partial(<function sleep at 0x7f0c3887a320>, 1)
execute: functools.partial(<function save_current_page at 0x7f0c388cc820>, 'crawl-blog-naver-com/home-sec-1-page-3.html')
```

수집이 잘 되었는지 디렉토리를 살펴봅니다.

```bash
$ ls -al crawl-blog-naver-com
total 2024
drwxr-xr-x 2 kjkang kjkang   4096 Aug 25 20:38 ./
drwxr-xr-x 5 kjkang kjkang   4096 Aug 25 20:38 ../
-rw-r--r-- 1 kjkang kjkang 342271 Aug 25 20:40 home-all-page-2.html
-rw-r--r-- 1 kjkang kjkang 341998 Aug 25 20:40 home-all-page-3.html
-rw-r--r-- 1 kjkang kjkang 340022 Aug 25 20:40 home-sec-1-page-1.html
-rw-r--r-- 1 kjkang kjkang 340588 Aug 25 20:40 home-sec-1-page-2.html
-rw-r--r-- 1 kjkang kjkang 342351 Aug 25 20:40 home-sec-1-page-3.html
-rw-r--r-- 1 kjkang kjkang 340958 Aug 25 20:40 home-sec-all-page-1.html
```


## 참고

-   [Selenium Documentation](https://www.selenium.dev/documentation/)
-   [CSS Selector Reference](https://www.w3schools.com/cssref/css_selectors.php)
-   [Github Gist 전체코드](https://gist.github.com/Inforgra/ea3809b1b44b5c68e5bece6a0e1abcbe)

