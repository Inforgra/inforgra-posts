---
published: true
title: Windows 작업 표시줄에서 검색창 활성화 및 비 활성화 하기
summary: Windows 작업 표시줄에서 검색창을 활성화 하거나 비활성화 하는 방법을 살펴보겠습니다.
date: 2024-08-22 00:00:00.0+09:00
image: step-1.png
imageAlt: Windows Taskbar
tags:
- windows11
- taskbar
- powershell
---

Windows 는 작업 표시줄에 있는 검색창을 통해 사용자 다양한 항목을 찾을 수 있는 기능을 제공합니다.
PC 내에 있는 앱, 문서, 사진, 메일, 폴더 등을 검색 할 수 있습니다.

검색창은 작업표시줄에서 꽤 큰 공간을 차지합니다.
만약 검색을 잘 사용지 않는 분이라면, 검색창을 비활성화 하는 것이 좋습니다.

검색창을 비활성화하거나 다시 활성화 하는 방법을 살펴보겠습니다.


## Windows 작업표시줄 메뉴에서 변경하기

![img](./step-1.png)

작업 표시줄에서 우측 클릭을 하고 `작업 표시줄 설정` 을 클릭합니다.

![img](./step-2.png)

`개인설정 > 작업 표시줄` 에서 검색 항목을 숨기기로 선택합니다.


## PowerShell 명령 사용하기

숨기기

```powershell
Set-ItemProperty 'HKCU:\Software\Microsoft\Windows\CurrentVersion\Search' -Name SearchboxTaskbarMode -Value 0
```

검색 아이콘만

```powershell
Set-ItemProperty 'HKCU:\Software\Microsoft\Windows\CurrentVersion\Search' -Name SearchboxTaskbarMode -Value 1
```

​검색 아이콘 및 레이블

```powershell
Set-ItemProperty 'HKCU:\Software\Microsoft\Windows\CurrentVersion\Search' -Name SearchboxTaskbarMode -Value 2
```

​검색 상자

```powershell
Set-ItemProperty 'HKCU:\Software\Microsoft\Windows\CurrentVersion\Search' -Name SearchboxTaskbarMode -Value 3
```

