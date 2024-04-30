---
published: true
title: 우분투 20.04 에서 Node.js 설치하기
date: 2024-04-14 09:00:00.0+09:00
image: nodejs.png
imageAlt: Node.js - Run JavaScript Everywhere
summary: 우분투 20.04 에서 최신 버전의 Node.js 를 설치하는 방법을 살펴봅니다.
tags:
  - nodejs
  - ubuntu
  - ubuntu 20.04
---

[Node.js](https://node.js) 는 Javascript 를 브라우저 밖에서 실행할 수 있도록 하는 런타임 환경이다. Next.js, Remix, Vue 등의 프레임워크를 개발하거나 실행할 때 반드시 설치가 필요하다.

우분투 환경에서 Node.js 의 설치는 apt 를 사용하여 설치할 수 있다. 이 경우 구버전(10.19)을 설치하며, 최신 버전이 필요한 경우 빌드나 서비스 할 때 오류가 발생한다. 최신 버전을 설치하기 위해서는 별도의 방법이 필요하다.

## PPA 설치 방법

사용하고자 하는 Node.js 의 버전이 고정되어 있는 경우 좋은 선택이 될 수 있다. 설치는 PPA 저장소의 URL 을 등록하고, apt 를 사용하여 설치한다. 다른 버전이 필요한 경우 `setup-20.x` 에서 원하는 버전으로 변경하면 된다.

```bash
$ curl -fsSL https://deb.nodesource.com/setup_20.x | sudo bash -
$ sudo apt-get install -y nodejs
```

## NVM 설치 방법

사용하고자 하는 Node.js 의 버전을 다양하게 쓸 때 사용한다. 설치하는 사용자에게만 적용되기 때문에 다수의 사용자를 쓰는 환경에서는 PPA 설치를 하는 것을 권장한다.

```bash
# installs NVM (Node Version Manager)
$ curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash

# download and install Node.js
$ nvm install 20

# verifies the right Node.js version is in the environment
$ node -v # should print `v20.12.2`

# verifies the right NPM version is in the environment
$ npm -v # should print `10.5.0`
```

## 참고

- [Nodesource Node.js](http://deb.nodesource.com/)
- [Download Node.js](https://nodejs.org/en/download/package-manager)
