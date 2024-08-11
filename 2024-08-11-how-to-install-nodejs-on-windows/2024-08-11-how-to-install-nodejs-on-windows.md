---
published: true
title: 윈도우에서 nodejs 설치하기
date: 2024-08-11 09:00:00.0+09:00
image: nodejs.png
imageAlt: Node.js - Run JavaScript Everywhere
summary: 윈도우에서 Winget을 사용하여 Nodejs를 설치하는 방법을 살펴보겠습니다.
tags:
  - nvm
  - nodejs
  - windows
  - winget
---

개인적으로 `Nodejs` 기반의 프로젝트를 진행하는 경우, 서버 환경에서 개발을 진행하였습니다. 윈도우 환경에서 `Nodejs` 가 필요한 상황이 발생하여, 설치하는 방법을 살펴보겠습니다.

## NVM 설치하기

`NodeJs` 를 설치할 때 버전이 고정되어 있다면, 바로 설치하는 것이 좋은 선택이 될 수 있습니다. 여러 버전 번갈아 사용하거나 지속적으로 업그레이드가 필요한 경우 `NVM` 사용하는 것이 좋습니다.

Windows 의 경우 우분투의 `apt`와 유사한 `winget` 패키지 관리자가 있습니다. `winget` 을 사용하면 보다 쉽게 설치가 가능합니다. `winget` 의 사용법은 [여기](https://inforgra.com/posts/2024-08-09-how-to-use-winget) 에서 자세히 볼 수 있습니다.

```
> winget install CoreyButler.NVMforWindows
찾음 NVM for Windows [CoreyButler.NVMforWindows] 버전 1.1.12
이 응용 프로그램의 라이선스는 그 소유자가 사용자에게 부여했습니다.
Microsoft는 타사 패키지에 대한 책임을 지지 않고 라이선스를 부여하지도 않습니다.
다운로드 중 https://github.com/coreybutler/nvm-windows/releases/download/1.1.12/nvm-setup.exe
  ██████████████████████████████  5.51 MB / 5.51 MB
설치 관리자 해시를 확인했습니다.
패키지 설치를 시작하는 중...
설치 성공
```

## NVM 설정하기

`NVM`을 설치시에 사용자 환경에 두개의 환경 변수를 추가합니다. 

- NVM_HOME: `NVM` 명령이 위치한 경로
- NVM_SYMLINK 기본을 설정한 `nodejs`의 경로

`PATH` 에 이 경로들을 추가합니다. `고급 시스템 설정 보기` 또는 다음과 같은 명령을 사용하여 경로를 추가할 수 있습니다.

```
> setx PATH "$([Environment]::GetEnvironmentVariable("PATH", "USER"));%NVM_HOME%;%NVM_SYMLINK%"
```

## NodeJS 설치하기

[우분투 20.04에서 Node.js 설치하기](https://inforgra.com/posts/2024-04-14-how-to-install-nodejs-on-ubunutu-20.04) 에서 사용한 방법으로 설치합니다.

```
> nvm list available

|   CURRENT    |     LTS      |  OLD STABLE  | OLD UNSTABLE |
|--------------|--------------|--------------|--------------|
|    22.6.0    |   20.16.0    |   0.12.18    |   0.11.16    |
|    22.5.1    |   20.15.1    |   0.12.17    |   0.11.15    |
|    22.5.0    |   20.15.0    |   0.12.16    |   0.11.14    |
|    22.4.1    |   20.14.0    |   0.12.15    |   0.11.13    |
|    22.4.0    |   20.13.1    |   0.12.14    |   0.11.12    |
|    22.3.0    |   20.13.0    |   0.12.13    |   0.11.11    |
|    22.2.0    |   20.12.2    |   0.12.12    |   0.11.10    |
|    22.1.0    |   20.12.1    |   0.12.11    |    0.11.9    |
|    22.0.0    |   20.12.0    |   0.12.10    |    0.11.8    |
|    21.7.3    |   20.11.1    |    0.12.9    |    0.11.7    |
|    21.7.2    |   20.11.0    |    0.12.8    |    0.11.6    |
|    21.7.1    |   20.10.0    |    0.12.7    |    0.11.5    |
|    21.7.0    |    20.9.0    |    0.12.6    |    0.11.4    |
|    21.6.2    |   18.20.4    |    0.12.5    |    0.11.3    |
|    21.6.1    |   18.20.3    |    0.12.4    |    0.11.2    |
|    21.6.0    |   18.20.2    |    0.12.3    |    0.11.1    |
|    21.5.0    |   18.20.1    |    0.12.2    |    0.11.0    |
|    21.4.0    |   18.20.0    |    0.12.1    |    0.9.12    |
|    21.3.0    |   18.19.1    |    0.12.0    |    0.9.11    |
|    21.2.0    |   18.19.0    |   0.10.48    |    0.9.10    |

```

LTS 최신 버전 (20.16.0) 으로 설치합니다.

```
> nvm install 20.16.0
Downloading node.js version 20.16.0 (64-bit)...
Extracting node and npm...
Complete
npm v10.8.1 installed successfully.


Installation complete. If you want to use this version, type

nvm use 20.16.0
```

기본 Nodejs 버전을 20.16.0 으로 지정합니다.

```
> nvm use 20.16.0
Now using node v20.16.0 (64-bit)
```


설치한 Nodejs 의 버전을 확인합니다.

```
> node --version
v20.16.0
```

## 참고

- [Nodesource Node.js](http://deb.nodesource.com/)
- [Download Node.js](https://nodejs.org/en/download/package-manager)
