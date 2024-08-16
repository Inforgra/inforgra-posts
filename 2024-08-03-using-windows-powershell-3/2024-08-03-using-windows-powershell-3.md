---
published: true
title: Windows PowerShell 활용하기 (3) - 환경변수 관리하기
summary: 환경변수는 시스템이나 프로그램에서 사용하는 데이터를 저장합니다. PowerShell 에서 환경변수를 조회, 변경, 추가 및 삭제 등과 같이 관리하는 방법을 살펴봅니다.
date: 2024-08-03 00:00:00.0+09:00
image: powershell.jpg
imageAlt: Microsoft PowerShell
tags:
- windows
- powershell
- environment
---

환경변수는 시스템이나 프로그램에서 사용하는 데이터를 저장합니다.
PowerShell 에서 환경변수를 조회, 변경, 추가 및 삭제 등과 같이 관리하는 방법을 살펴봅니다.

-   [Windows PowerShell 활용하기 (1)  - 기초](https://inforgra.com/posts/2024-08-01-using-windows-powershell-1)
-   [Windows PowerShell 활용하기 (2)  - 파이프라인](https://inforgra.com/posts/2024-08-02-using-windows-powershell-2)
-   [Windows PowerShell 활용하기 (3)  - 환경변수 관리하기](https://inforgra.com/posts/2024-08-03-using-windows-powershell-3)
-   Windows PowerShell 활용하기 (4)  - 프로세스
-   Windows PowerShell 활용하기 (5)  - 서비스
-   Windows PowerShell 활용하기 (6)  - 패키지


## 환경 변수란?

환경변수는 키와 값을 문자열 형태로 저장합니다.
시스템의 이름, 사용자명, 사용자의 홈 디렉토리 등과 같은 데이터를 환경 변수에 저장할 수 있습니다.

Unix 계열의 쉘의 경우 환경변수의 이름은 대소문자를 구분합니다.
그러나 PowerShell 는 대소문자를 구분하지 않습니다.

환경변수의 범위는 다음 세 가지 범위로 정의할 수 있습니다.

-   시스템 범위
-   사용자 범위
-   프로세스 범위

프로세스를 실행하면, 부모 프로세스의 환경 변수를 상속합니다.
시스템, 사용자 범위에서 정의한 환경 변수를 사용하여 생성합니다.

PowerShell 에서 환경 변수를 관리하는 방법은 세가지가 있습니다.

-   변수 구문
-   환경 공급자 및 항목 cmdlet
-   .Net System.Environment 클래스

환경 변수의 값을 변경하면, 현재 세션(또는 프로세스)에만 반영합니다.
시스템이나 프로세스를 재시작하면 환경변수의 값은 초기화 됩니다.

환경변수를 영구적으로 변경하는 방법은 네 가지가 있습니다.

-   Profile 설정
-   .Net System.Environment 클래스의 SetEnvironmentVariable() 메소드 사용
-   레지스트리 변경을 위해 cmdlet 을 명령을 사용
-   시스템 제어판 사용


## 변수 구문 사용하기

다음 구문을 사용하여 환경 변수의 값을 조회하고, 변경합니다.

```
$Env:<환경변수명>
```

예를 들어 `WINDIR` 변수의 값을 조회하려면 아래와 같이합니다.

```
> $Env:WINDIR
C:\WINDOWS
```

문자 `\=` 을 사용하여 환경변수의 값을 변경합니다.

```
$Env:<환경변수명> = "<새로운 값>"
```

환경변수의 값은 항상 문자열 형태를 가집니다.

```
$Env:FOO = "An Example"
```


## Cmdlet 사용하기

경로 `-Path` 에 `Env:` 드라이버를 지정하면 환경변수의 전체 목록을 조회합니다.

```
> Get-Item Env:

Name                           Value
----                           -----
ProgramFiles(x86)              C:\Program Files (x86)
ProgramW6432                   C:\Program Files
```

환경변수명을 사용하여 지정한 환경변수만을 조회합니다.

```
> Get-Item Env:windir

Name                           Value
----                           -----
windir                         C:\WINDOWS
```

환경변수의 값은 `Value` 구문을 사용하여 조회합니다.

```
> $(Get-Item Env:WINDIR).Value
C:\WINDOWS
```

`Set-Item` 명령을 사용하여, 환경변수의 값을 수정합니다.

```
> Set-Item -Path Env:windir -Value D:\Windows
```

`New-Item` 명령을 사용하여, 새로운 환경변수를 추가합니다.

```
> New-Item -Path Env:mydir -Value C:\Windows
```

`Remove-Item` 명령을 사용하여, 환경변수를 제거합니다.

```
> Remove-Item -Path Env:mydir
```


## .Net System.Environment 클래스 사용하기

System.Environment 클래스는 환경변수를 조회하고, 수정하는 메소드를 제공합니다.

`GetEnvironmentVariable()` 메소드를 사용하여 특정 환경 변수를 조회합니다.

```
> [Environment]::GetEnvironmentVariable("WINDIR")
D:\Windows
```

`SetEnvironmentVariable()` 메소드를 사용하여 환경변수를 수정합니다.

```
> [Environment]::SetEnvironmentVariable("WINDIR", "D:\Windows")
```


## 영구적으로 환경변수 변경하기


### Profile 을 사용하여 변경

이 방법은 추후 별도의 문서에서 다룰 예정입니다.


### SetEnvironmentVariable() 메소드를 사용하여 변경

`SetEnvironmentVariable()` 메소드의 세번째 매개변수를 지정하여, 해당 범위의 환경 변수를 변경합니다.

-   `Machine` 시스템 범위의 환경변수를 변경
-   `User` 사용자 범위의 환경변수를 변경

```
> [Environment]::SetEnvironmentVariable("WINDIR", "D:\Windows", "Machine")
```


### Cmdlet 을 사용하여 레지스트리 변경

각 환경변수의 범위에 해당하는 레지스트리 드라이버는 다음과 같습니다.

-   시스템: HKLM
-   사용자: HKCU

레지스트리 드라이버 내 `Environment` 경로를 사용하여 환경변수에 접근합니다.
환경변수의 항목들은 속성 `Property` 으로 관리합니다.

```
> Get-Item HKCU:\Environment\

    Hive: HKEY_CURRENT_USER

Name                           Property
----                           --------
Environment                    ChocolateyLastPathUpdate : 133336991101727625
                               ChocolateyToolsLocation  : C:\tools
                               FLUTTER_HOME             : E:\Developments\flutter
...
```

`Get-ItemProperty` 명령을 사용하여, 지정한 환경변수를 조회합니다.

```
> Get-ItemProperty -Path HKCU:\Environment\ -Name FLUTTER_HOME

FLUTTER_HOME : E:\Developments\flutter
PSPath       : Microsoft.PowerShell.Core\Registry::HKEY_CURRENT_USER\Environment\
PSParentPath : Microsoft.PowerShell.Core\Registry::HKEY_CURRENT_USER
PSChildName  : Environment
PSDrive      : HKCU
PSProvider   : Microsoft.PowerShell.Core\Registry
```

`Set-ItemProperty` 명령을 사용하여, 환경변수를 변경합니다.

```
Set-ItemProperty -Path HKCU:\Environment\ -Name FLUTTER_HOME -Value C:\dev\flutter
```

