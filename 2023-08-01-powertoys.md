---
title: Microsoft PowerToys 리뷰
description: 마이크로소프트 PowerToys에서 제공하는 다양한 기능들을 살펴보고 실무에 적용하여 생산성을 높여 봅시다.
author: Kukjin Kang
date: 2023-08-01
tags: ["powertoys", "windows"]
image:
---

## 개요

Microsoft PowerToys 는 Windows 환경에서 생산성을 높일 수 있는 도구 입니다. 화면
분할, 이미지 크기 조정, 키보드 배열 변경 등 다양한 기능을 제공합니다. 각각의
기능들을 익히기 위해서는 시간이 좀 필요하지만, 한번 익혀두면 매우 유용하게
사용할 수 있는 기능들이니 설치해서 사용해 보시길 바랍니다.


## 설치

Windows 에서 어플리케이션을 설치할 수 있는 방법은 크게 3가지 정도가 있습니다.
개인적으로 가장 간단하게 설치할 수 있는 winget 명령을 사용하는 것을 선호합니다.

### Microsoft Store 를 통한 설치

1. [Microsoft Store](https://apps.microsoft.com/store/detail/microsoft-powertoys/XP89DCGQ3K6VLD)
   를 클릭하여 스토어 앱을 실행합니다.
1. `스토어 앱에서 다운로드` 버튼을 클릭합니다.
1. Microsoft Store 설치 창에서 `설치` 버튼을 클릭합니다.
1. 설치 안내를 따라 설치를 진행합니다.

### powershell 에서 winget 명령을 사용하여 설치

1. `PowerShell` 을 관리자 권한으로 실행합니다.
1. 다음과 같이 명령을 실행합니다.
   ```shell
   winget install Microsoft.PowerToys
   ```
1. 설치 안내를 따라 설치를 진행합니다.

### 프로그램을 직접 다운 받아 설치

1. [GitHub](https://github.com/microsoft/PowerToys/releases/tag/v0.71.0) 릴리즈 페이지를 방문합니다.
1. 환경에 맞는 설치파일을 다운로드 받습니다.
   - 자신의 환경을 잘 모르겠다면, `Machine wide - x64`를 선택합니다.
1. 설치파일을 실행하여, 설치 안내를 따라 설치를 진행합니다.

## 주요기능

PowerToys 에서 제공하는 주요 기능들에 대해 살펴봅니다. 모든 기능들을 대상으로 하지 않고,
자주 사용할 수 있는 기능들을 몇 가지를 골라보았습니다.

### Alyways on Top

특정 창을 항상 위로 두게 합니다. 모니터 화면이 크거나, 여려개의 창을 띄워 사용할 때 유용합니다.
`WIN + Ctrl + T` 단축키를 사용하여 창을 고정하거나 해제할 수 있습니다. 고정된 창의 테두리는
별도의 색으로 표시를 하여 고정여부를 확인 할 수 있도록 합니다.

<img src="https://i.ibb.co/hMBJ3Z7/powertoys-1.png" width="100%" />

### FancyZones

모니터에서 화면의 레이아웃을 지정할 수 있고, 이 레이아웃에 따라 창을 배열합니다. 레이아웃은
보통 좌/중/우 3분할, 좌/우 분할, 상하 분할등 다양한 기본값이 있으며, 개인 취향에 따라 새로
만들거나 변경할 수 있습니다. 만약 모니터가 여려개라면 모니터별로 각각 지정할 수도 있습니다.

`shift` 키를 누른채로 창을 이동하면 현재 지정한 레이아웃을 볼 수 있으며, 선택한 레이아웃에
따라 창이 배치가 됩니다.

<img src="https://i.ibb.co/j8RgcFc/powertoys-fancyzone.png" width="100%" />

### Keyboard Manager

키보드에서 가장 쓸모 없는 키 중에 하나가 `caps lock`입니다. 개발자의 경우에는 `caps lock`을
`control` 키로 변경해서 사용하는 경우가 많습니다. 예전에는 레지스터리를 변경하거나 별도의
프로그램을 사용해야 했지만 이제는 PowerToys 에서 간단하게 설정할 수 있습니다.

`Keyboard Manager` 탭에서 `키 다시 맵핑` 메뉴를 선택하면 키를 변경할 수 있는 메뉴가 나옵니다.
`키 다시 맵핑` 창에서 실제키, 맵핑 키를 입력하여 변경할 수 있습니다. 다음은 `caps lock`을
`control` 키로 맵핑한 결과입니다.

<img src="https://i.ibb.co/47wj17Y/powertoys-keyboardmanager.png" width="100%" />

### Mouse Utilities

예전에는 17" 모니터를 사용하면 매우 크다라고 생각했지만, 요즘은 30" 이상의 큰 모니터를 여러
개 사용하는 경우가 많습니다. 이렇게 큰 화면을 사용하다 보면 마우스 커서를 잃어 버리는 경우가
종종 발생하는데, 마우스 커서 위치를 표시해 줍니다. `contron` 키를 두번 연속해서 누르면 마우스
커서의 위치를 알려줍니다.

<img src="https://i.ibb.co/YBdCcrn/powertoys-mouse-utilities.gif" width="100%" />

### Mouse without Borders

테스크탑, 노트북 등 여러대의 컴퓨터를 동시에 사용하는 경우도 많습니다. 이럴때 하나의 키보드와
마우스로 다수의 컴퓨터를 제어하는 것이 매우 유용합니다. 별도의 어플리케이션으로 제공했었으나,
이제는 PowerToys 에서 만날 수 있습니다.

Windows 환경에서만 작업을 한다면 가장 쓸만한 어플리케이션입니다. Windows 외에 이 기종간의
키보드, 마우스 공유 프로그램은 많지만 한/영 변환, 슈퍼키(위도우키) 등 몇몇 부분에서 잘 안되는
경우가 있습니다. 다른 어플리케이션에 대해 좀 더 알고 싶다면 아래 별도의 목록을 참고하시길 바랍니다.

한대의 컴퓨터에서 `보안 키`를 생성하고, 다른 컴퓨터에 생성한 `보안키`를  입력하는 것으로
간단한게 설정합니다. 장치 레이아웃 메뉴에서 컴퓨터의 배열을 지정할 수 있고, 여러가지 동작
방법을 옵션으로 선택할 수 있습니다.

<img src="https://i.ibb.co/C5NwcPj/powertoys-mouse-without-borders-press-new-key.png" width="100%" />

## 참고

- [PowerToys 공식 문서](https://learn.microsoft.com/ko-kr/windows/powertoys/)


다기종간의 키보드 마우스를 공유하는 어플리케이션 목록입니다.

- [ShareMouse](https://www.sharemouse.com)
- [Synergy](https://symless.com/synergy)
- [Logitech OPTIONS](https://www.logitech.com/ko-kr/software/options.html)
