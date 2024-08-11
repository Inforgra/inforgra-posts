---
published: true
title: 우분투 20.04 에서 Node.js 설치하기
date: 2024-04-14 09:00:00.0+09:00
image: nodejs.png
imageAlt: Node.js - Run JavaScript Everywhere
summary: 우분투에서 Node.js 를 설치하는 방법은 여러 가지가 있습니다. NVM 통해 Node.js 를 설치하면 다양한 버전의 Node.js 환경을 사용할 수 있습니다.
tags:
  - nodejs
  - ubuntu
  - ubuntu 20.04
---

[Node.js](https://node.js) 는 Javascript 를 브라우저 밖에서 실행할 수 있도록 하는 런타임 환경입니다. Next.js, Remix, Vue 등의 프레임워크를 개발하거나 실행할 때 반드시 설치가 필요합니다.

우분투에서 Node.js 의 설치는 apt 를 사용하여 설치할 수 있습니다. 이 경우 구버전(10.19)을 설치하며, 최신 버전이 필요한 경우 빌드나 서비스 할 때 오류가 발생합니다. 최신 버전을 설치하기 위해서는 별도의 방법이 필요합니다.

## PPA 설치 방법

사용하고자 하는 Node.js 의 버전이 고정되어 있는 경우 좋은 선택이 될 수 있습니다. 설치는 PPA 저장소의 URL 을 등록하고, apt 를 사용하여 설치합니다. 다른 버전이 필요한 경우 `setup-20.x` 에서 원하는 버전으로 변경합니다.

```bash
$ curl -fsSL https://deb.nodesource.com/setup_20.x | sudo bash -
$ sudo apt-get install -y nodejs
```

## NVM 설치 방법

NVM (Node Version Manager) 은 다양한 버전의 Node.js 를 설치하고, 관리하는 도구입니다. 다른 버전의 Node.js 를 사용하는 프로젝트를 관리할 때 유용합니다. 설치하는 사용자에게만 적용되기 때문에 다수의 사용자를 쓰는 환경에서는 PPA 설치를 하는 것을 권장합니다.

아래와 같이 쉘 스크립트를 사용하여 설치합니다. 설치 경로는 `~/.nvm` 이며, 환경 설정은 `~/.nvm/nvm.sh` 에 있습니다.

```bash
$ curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
```

`-v` 옵션을 사용하여 NVM 버전을 확인합니다. 만약 `nvm` 명령이 실행되지 않는다면, 다시 로그인 하거나, `source ~/.nvm/nvm.sh` 명령을 실행하여 NVM 환경을 설정합니다.

```bash
$ nvm -v
0.39.7
```

`ls-remote` 옵션을 사용하여 설치 가능한 Node.js 버전을 확인할 수 있습니다. 여기서는 LTS 최신 버전은 `v20.12.2` 입니다.

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

'install' 옵션을 사용하여 필요한 Node.js 를 설치합니다.

```bash
$ nvm install 20
```

'use' 옵션을 사용하여 특정 버전의 Node.js 로 변경합니다.

```bash
$ nvm use 21
```

'alias' 옵션을 사용하여 기본 버전을 지정할 수 있습니다.

```bash
$ nvm alias default 20
```

## 참고

- [Nodesource Node.js](http://deb.nodesource.com/)
- [Download Node.js](https://nodejs.org/en/download/package-manager)
