---
title: 어떤 토렌트 클라이언트를 사용할 것인가?
description: 토렌트 파일을 받기 위해서 가장 먼저 해야할 것은 토렌트 클라이언트를 설치하는것이다. 어떤 경우는 애드웨어 광고를 기본적으로 띄우거나 비트코인 채굴기를 실행하는 문제점이 있다. 안전한 토렌트 클라이언트는 어떤것이 있는지 알아 보자.
author: Kukjin Kang
date: 2023-07-06
tags: ["torrent", "토렌트", "토렌트 클라이언트"]
image: 
---

토렌트 파일을 받기 위해서 가장 먼저 해야할 것은 토렌트 클라이언트를 설치하는
것이다. 어떤 경우는 애드웨어 광고를 기본적으로 띄우거나 비트코인 채굴기를
실행하는 문제점이 있어 조심할 필요성이 있다. (뭐 이게 조심한다고 되는지는 모르겠지만...)

특히 '토렌트'라는 키워드로 검색시 상단에 노출하는 [µTorrent(utorrent)](https://utorrent.com/),
[BitTorrent](https://www.bittorrent.com/) 와 같이 특정 회사에서 제공하는 경우
어떻게 동작하는지 내부를 알 수 없기 때문에 특히 주의가 필요하다.

오픈소스를 대상으로 관심이 높거나, 활발히 개발되고 있는 프로젝트들을 대상으로
살펴보았다. 당연히 앞으로 소개하는 모든 토렌트 클라이언트는 무료이면서, **광고는
없다**는 것을 참고하길 바란다.

## [qBittorrent](https://www.qbittorrent.org/)

<img src="https://i.ibb.co/Q9QnxY4/New-q-Bittorrent-Logo-svg.png" width="100px" />

토렌트 클라이언트 중 사용자의 관심이 높고 가장 활발하게 개발되는
프로젝트이다. 타 클라이언트 보다 많은 세부 옵션을 제공한다.

- [GitHub](https://github.com/qbittorrent/qBittorrent/), 21,2k Stars
- [Qt](https://www.qt.io)를 기반으로 다양한 플랫폼 (Windows, macOS, Linux 등) 지원
- 웹인터페이스를 지원하여, 외부에서 접근할 수 있는 방법을 지원


## [Transmission](https://transmissionbt.com/)

<img src="https://i.ibb.co/Hdx5nd5/Transmission-icon.png" width="100px" />

수 년전에는 macOS만 지원하는 것으로 알고 있었는데, 최근에는 대부분의 플랫폼을
지원한다. 군더더기 없는 깔끔한 UI를 원한다면 한번 설치해 볼만하다.

- [GitHub](https://github.com/transmission/transmission), 10.1k Stars
- [Qt](https://www.qt.io)를 기반으로 다양한 플랫폼 (Windows, macOS, Linux 등) 지원
- 웹인터페이스 지원하며, 직관적이고 단순한 UI 제공

## [PicoTorrent](https://picotorrent.org/)

<img src="https://i.ibb.co/4F22nc7/13157179.png" width="100px" />

이번에 알게되었는데, Windows 전용 토렌트가 있었다. Windows UI에 가장 적합하다고
보여지며, 타 OS를 사용해 보지 못한 분들은 이것을 설치하는 것도 괜찮다고
생각한다.

- [GitHub](https://github.com/picotorrent/picotorrent) 2.4k Stars
- Windows 플랫폼만을 지원

## [aria2](https://aria2.github.io/)

소개페이지에서 이것은 다운로드 유틸리티라고 이야기한다. 보통 HTTP/HTTPS
다운로드를 받을 때 curl 이나 wget 을 많이 사용하는데, 이와 같은 유틸리티라 보면
되겠다. 토렌트나 마그넷도 다운로드 중 하나의 방법이다 라는 관점으로 보는듯
하다. 다운로드가 종료되면 프로그램도 같이 종료가 되니, 잠깐 다운로드 받는 용도로
적합하다고 보인다.

- [GitHub](https://github.com/aria2/aria2) 30.9k Stars
- 다양한 플랫폼 (Windows, macOS, Linux 등) 지원
- 명령어 기반으로 UI를 제공하지 않음

이제 토렌트 클라이언트를 받았으면, [토렌트 바로가기](/torrent) 에서 사이트로
들어가 필요한 파일들을 다운로드 받아보자 


