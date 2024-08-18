---
published: true
title: Git 저장소에서 특정 디렉토리만 복제하기
summary: 여러개의 Git 프로젝트로 구분해야 할 것으로 보이지만, 하나의 Git 저장소에서 하위 디렉토리로 구분하여 사용하는 경우가 있습니다. Git 에서 체크아웃할 때 특정 디렉토리만 선택하여 가져오는 방법을 살펴봅니다.
date: 2024-08-18 00:00:00.0+09:00
image: git.png
imageAlt: git
tags:
- git
- sparse-checkout
- tip
---

여러개의 Git 프로젝트로 구분해야 할 것으로 보이지만, 하나의 Git 저장소에서 하위 디렉토리로 구분하여 사용하는 경우가 있습니다.
`remix` 프로젝트에서는 다양한 예제를 각각의 디렉토리로 구분하여 관리합니다. <sup><a id="fnr.1" class="footref" href="#fn.1" role="doc-backlink">1</a></sup>

관리하는 입장에서는 이런 경우 다수의 git 프로젝트로 관리하는 것보다, 하나로 쓰는 것이 편리합니다.
사용자의 입장에서는 필요한 예시들만 가져와서 사용하는 것이 좋습니다.


## Sparse Checkout

원격 Git 저장소에서 특정 경로만 가져오는 것을 **sparse checkout** 이라 하며, Get 1.7 이상에서 지원합니다.
**sparse checkout** 을 하는 단계는 다음과 같습니다.


### 빈 Git 저장소를 만들고, 원격 주소를 추가

```bash
$ mkdir <repo>
$ cd <repo>
$ git init
$ git remote add -f origin <url>
```


### Git 설정에서 Sparse Checkout 기능 활성화

Git 설정에서 `core.sparseChekcout` 값을 `true` 로 변경하여 활성화 합니다.
`sparse-checkout` 명령은 Git 2.25 이상 버전에서 지원합니다.

```bash
$ git config core.sparseCheckout true
또는
$ git sparse-checkout init
```


### 체크아웃 할 파일 또는 디렉토리를 지정

`.git/info/sparse-checkout` 내에 체크아웃 할 파일 또는 디렉토리를 지정합니다.

```
$ echo "some/dir" >> .git/info/sparse-checkout
또는
$ git sparse-checkout set some/dir
```


### 원격과 같은 상태로 업데이트

원격과 같은 상태로 빈 저장소를 업데이트 합니다.

```
$ git pull origin master
```


### 참고

-   [How do I clone a subdirectory only of a Git repository?](https://stackoverflow.com/questions/600079/how-do-i-clone-a-subdirectory-only-of-a-git-repository)


## Footnotes

<sup><a id="fn.1" href="#fnr.1">1</a></sup> <https://github.com/remix-run/examples>
