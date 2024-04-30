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

사용하고자 하는 Node.js 의 버전이 고정되어 있는 경우 좋은 선택이 될 수 있다. 설치는 PPA 저장소의 URL 을 등록하고, apt 를 사용하여 설치한다. 다른 버전이 필요한 경우 `setup-20.x` 에서 원하는 버전으로 변경한다.

```bash
$ curl -fsSL https://deb.nodesource.com/setup_20.x | sudo bash -
$ sudo apt-get install -y nodejs
```

## NVM 설치 방법

NVM (Node Version Manager) 은 다양한 버전의 Node.js 를 설치하고, 관리하는 도구이다. 다른 버전의 Node.js 를 사용하는 프로젝트를 관리할 때 유용하다. 설치하는 사용자에게만 적용되기 때문에 다수의 사용자를 쓰는 환경에서는 PPA 설치를 하는 것을 권장한다.


아래 명령을 실행하여, NVM 설치를 할 수 있다. 설치 경로는 `~/.nvm` 이며, 환경 설정은 `~/.nvm/nvm.sh` 에서 한다.

```bash
$ curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
```

설치한 NVM 버전을 확인한다. 만약 `nvm` 명령이 실행되지 않는다면, 다시 로그인 하거나 `source ~/.bashrc` 명령을 실행한다.

```bash
$ nvm -v
0.39.7
```

설치 가능한 Node.js 버전을 확인한다.

```bash
$ nvm ls-remote
...
       v20.9.0   (LTS: Iron)
       v20.10.0   (LTS: Iron)
       v20.11.0   (LTS: Iron)
       v20.11.1   (LTS: Iron)
       v20.12.0   (LTS: Iron)
       v20.12.1   (LTS: Iron)
->     v20.12.2   (Latest LTS: Iron)
...
```

'install' 옵션을 사용하여 필요한 Node.js 를 설치한다.

```bash
$ nvm install 20
```

'use' 옵션을 사용하여 특정 버전의 Node.js 로 변경한다.

```bash
$ nvm use 21
```

'alias' 옵션을 사용하여 기본 버전을 지정한다.

```bash
$ nvm alias default 20
```

## 참고

- [Nodesource Node.js](http://deb.nodesource.com/)
- [Download Node.js](https://nodejs.org/en/download/package-manager)
