---
published: false
title: Remix 프레임워크를 사용하여 서비스 만들기
summary: 
date: 2024-08-19 00:00:00.0+09:00
image: remix.png
imageAlt: remix
tags:
- remix
---

새로운 프레임워크를 사용하여 최대한 빠르게 코드를 실행하려면, 기본 구조에 익숙해져야 합니다.
프레임워크의 설치, 주요 기능들의 설정, 빌드 및 실행 방법 등을 파악해야 합니다.
이 때 세세한 기능까지 살펴보지는 않아도 됩니다. 필요할 때 고민해도 늦지 않습니다.


### 설치

`create-remix` 명령을 사용하여, 초기 프로젝트를 생성합니다.

```bash
$ npx create-remix@latest
```

프로젝트를 생성할 경로를 입력합니다.

```
dir   Where should we create your new project?
      ./first-remix
```

소소코드의 관리는 Git 으로 합니다.

```
git   Initialize a new git repository?
      Yes
```

의존성 있는 패키지를 추가 설치 합니다.

```
deps   Install dependencies with npm?
       Yes
```


### 실행

`dev` 명령을 사용하여, 서비스를 실행합니다.

```
$ npm run dev

  ➜  Local:   http://localhost:5173/
  ➜  Network: use --host to expose
  ➜  press h + enter to show help
```

![img](./remix-example.png)


## 참고

-   [Remix - Quick Start](https://remix.run/docs/en/main/start/quickstart)

