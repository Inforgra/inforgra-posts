---
title: qBittorrent 설치 및 사용법
description: 오픈소스 토렌트 클라이언트인 qBittorrent의 설치 방법과 사용법에 대해 알아봅니다.
author: Kukjin Kang
date: 2023-07-31
tags: ["qBittorrent"]
image: https://i.ibb.co/BzGzzMz/qbittorrent-8.png
---

## 소개

큐빗토렌트(qBittorrent)는 오픈 소스 프로젝트로서 Windows, MacOS, Linux 등
다양한 환경을 지원합니다. uTorrent가 설치시에 다양한 부가 프로그램 설치를
요구하고, 실행시에도 광고를 하는 반면에 실행에 필요한 것만 설치하며, 광고도
없습니다. 웹 버전을 지원하여 NAS용 클라이언트로도 사용 가능합니다.

## 설치방법

우선 [공식 다운로드 페이지](https://www.qbittorrent.org/download)에서 설치
환경에 맞는 파일을 다운로드 한다. 윈도우 사용자라면 `Windows x64 (qt6)`
버전을 받는 것을 추천합니다.

### 다운로드한 설치파일을 실행합니다.

![install](https://i.ibb.co/m05n6tJ/qbittorrent-1.png)

### 설치 시작화면에서 다음 버튼을 눌러준다.

![install](https://i.ibb.co/3cKQnFy/qbittorrent-2.png)

### 사용권 계약에 동의 후 다음 버튼을 눌러준다.

![install](https://i.ibb.co/vHgYm73/qbittorrent-3.png)

### 구성요소를 선택한 후 다음 버튼을 눌러준다.

![install](https://i.ibb.co/TKhVF94/qbittorrent-4.png)

### 설치위치를 선택한 후 다음 버튼을 눌러준다.

![install](https://i.ibb.co/jhcBzbv/qbittorrent-5.png)

### 설치완료 화면에서 마침 버튼을 눌러준다.

![install](https://i.ibb.co/xLDTbm8/qbittorrent-6.png)

### 법적공지를 한번 읽고 동의 버튼을 눌러준다.

![install](https://i.ibb.co/c1GDSCy/qbittorrent-7.png)

### 프로그램의 실행 결과를 확인한다.

![install](https://i.ibb.co/BzGzzMz/qbittorrent-8.png)

## 설정방법

대부분 기본 설정으로 가져가는 것이 좋지만, 변경이 필요한 지점이 몇 군데 있습니다.

### 다운로드 위치 설정

다운로드는 기본적으로 사용자의 다운로드 폴더를 사용합니다. 파일을 별도로 관리할
생각이 있다면 다른 디스크나 경로를 지정합니다. 별도의 디스크(ex: D:\\)가 있다면
따로 지정해서 사용하는 것이 좋습니다.

<img src="https://i.ibb.co/G7WZdJc/settings-1.png" width="100%" />

### 속도 제한 설정

토렌트는 하나의 파일을 여러 조각으로 나눈뒤 각 조각별로 다운로드와 업로드를
동시에 진행합니다. 만약 업로드에서 대역폭을 많이 쓰면 다운로드가 느려질 수 있습니다.
업로드와 다운로드의 속도는 각각 지정할 수 있으며, 올려주기(업로드)의 값은 작을 수록 좋습니다.

<img src="https://i.ibb.co/vXy7wtQ/settings-2.png" width="100%" />

### 배포 제한 설정

배포 제한 설정을 하지 않으면, 토렌트가 관리 목록에 존재하는 한 항상 업로드를 하게 됩니다.
따라서 비율 또는 시간을 맞추게 되면 토렌트 파일을 제거하는 것으로 합니다.

![settings]()
<img src="https://i.ibb.co/zhjGdcQ/settings-3.png" width="100%" />

### 로그 설정

개발자가 아닌 이상 로그를 살펴볼 일은 없습니다. 별도로 로그를 남기지 않도록 체크를 해제합니다.

![settings]()
<img src="https://i.ibb.co/KX4WvKV/settings-4.png" width="100%" />

## 사용방법

토렌트 파일 (.torrent) 과 마그넷은 설치시에 브라우저나 탐색기에서 쓸 수 있도록 연동해 두었습니다.
토렌트 파일을 다운로드 받아 실행하거나, magnet 경로를 복사해서 붙이면 다운로드 받을 수 있습니다.

### .torrent 파일 추가하기

<img src="https://i.ibb.co/3WS4Fys/usage-1.png" width="100%" />

### magenet 경로 추가하기

<img src="https://i.ibb.co/3Yj1NVJ/usage-2.png" width="100%" >

## 참고

- [qBittorrent 공식 홈페이지](https://www.qbittorrent.org/)
