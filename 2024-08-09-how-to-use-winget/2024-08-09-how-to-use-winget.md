---
published: true
title: Winget 을 사용하여 프로그램 설치하기
date: 2024-08-19 09:00:00.0+09:00
image: winget.jpg
imageAlt: content="Searching, discovering and installing winget packages made effortless without any third-party programs
summary: Windows 에서 패키지를 관리하는 유형들을 살펴보고, 개발자에게 유용한 `winget` 패키지 관리자를 사용하여 패키지를 관리하는 방법을 알아보겠습니다.
tags:
  - windows
  - winget
  - pakcage manager
---

Windows 에서 응용프로그램(또는 앱)을 설치하는 방법은 여러 가지가 있습니다. 대부분 아래의 유형으로 설치 과정을 거치며 각각 장단점이 있습니다.

- MSI 패키지를 다운 받아 실행하여 설치
- ZIP 패키지를 다운 받아 적절한 경로에 압축을 풀기
- Microsoft Store 에서 설치
- Winget 명령을 사용하여 설치

`Winget`은 마이크로소프트에서 개발한 Window 패키지 관리 프로그램입니다. 명령을 사용하여 패키지를 검색, 설치, 업그레이드 및 제거를 일관성 있게 처리할 수 있습니다. `VSCode`, `Android Studio`, `Git`, `NVM`, `Everything` 등 개발시 필요한 패키지들을 제공하기 때문에 개발자에게 매우 유용합니다. 또한 Windows 10/11 에 기본 설치가 되어 있어, 별도의 작업 없이 사용 가능 합니다.

## 패키지 검색하기

`search` 옵션을 사용하여 패키지를 검색합니다.

```
> winget search nvm

이름                              장치 ID                                         버전   일치         원본
------------------------------------------------------------------------------------------------------------
NVM for Windows                   CoreyButler.NVMforWindows                       1.1.12 Command: nvm winget
Fast Node Manager                 Schniz.fnm                                      1.37.1 Tag: nvm     winget
Volta                             Volta.Volta                                     1.1.1  Tag: nvm     winget
CrystalDiskInfo                   CrystalDewWorld.CrystalDiskInfo                 9.3.2  Tag: nvme    winget
CrystalDiskInfo Aoi Edition       CrystalDewWorld.CrystalDiskInfo.AoiEdition      9.3.2  Tag: nvme    winget
CrystalDiskInfo Kurei Kei Edition CrystalDewWorld.CrystalDiskInfo.KureiKeiEdition 9.3.2  Tag: nvme    winget
CrystalDiskInfo Shizuku Edition   CrystalDewWorld.CrystalDiskInfo.ShizukuEdition  9.3.2  Tag: nvme    winget
```

[https://winget.run/] 에서도 패키지를 검색할 수 있습니다.

## 패키지 설치하기

`install` 옵션을 사용하여 응용프로그램을 설치합니다. 패키지의 이름은 중복 가능하기 때문에 `장치 ID`를 사용합니다.

```
> winget install --id CoreyButler.NVMforWindows
찾음 NVM for Windows [CoreyButler.NVMforWindows] 버전 1.1.12
이 응용 프로그램의 라이선스는 그 소유자가 사용자에게 부여했습니다.
Microsoft는 타사 패키지에 대한 책임을 지지 않고 라이선스를 부여하지도 않습니다.
다운로드 중 https://github.com/coreybutler/nvm-windows/releases/download/1.1.12/nvm-setup.exe
  ██████████████████████████████  5.51 MB / 5.51 MB
설치 관리자 해시를 확인했습니다.
패키지 설치를 시작하는 중...
설치 성공

```

## 패키지 제거하기

`remove` 옵션을 사용하여 패키지를 제거합니다.

```
> winget remove --id CoreyButler.NVMforWindows
찾음 NVM for Windows 1.1.12 [CoreyButler.NVMforWindows]
패키지 제거를 시작하는 중...
성공적으로 제거됨
```

## 참고

- [Windows Package Manager](https://learn.microsoft.com/en-us/windows/package-manager/)
- [https://winget.run/](https://winget.run/)
