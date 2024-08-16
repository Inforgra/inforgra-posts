---
published: true
title: Windows PowerShell 활용하기 (1) - Cmdlet
summary: PowerShell 은 Windows 에서 제공하는 기능들을 명령으로 실행할 수 있는 강력한 도구로서 개발자에게 유용한 옵션입니다.
date: 2024-08-01 00:00:00.0+09:00
image: powershell.jpg
imageAlt: Microsoft PowerShell
tags:
- windows
- powershell
---

PowerShell 은 Windows 에서 제공하는 기능들을 명령으로 실행할 수 있는 강력한 도구입니다.
사용자의 환경 변수를 조회하거나, 서비스를 시작하고, 특정 프로세스 중지 하는 등의 작업을 할 수 있습니다.
또한 파이프라인을 사용하여 한번에 다양한 명령들을 순차 실행할 수 있습니다.

-   [Windows PowerShell 활용하기 (1)  - 기초](https://inforgra.com/posts/2024-08-01-using-windows-powershell-1)
-   [Windows PowerShell 활용하기 (2)  - 파이프라인](https://inforgra.com/posts/2024-08-02-using-windows-powershell-2)
-   [Windows PowerShell 활용하기 (3)  - 환경변수 관리하기](https://inforgra.com/posts/2024-08-03-using-windows-powershell-3)
-   Windows PowerShell 활용하기 (4)  - 프로세스
-   Windows PowerShell 활용하기 (5)  - 서비스
-   Windows PowerShell 활용하기 (6)  - 패키지


## Cmdlet 이란

Cmdlet 은 Command-Lets 의 줄임말로서 PowerShell 에서 개체를 조작하기 위한 명령입니다.
이 명령은 별도로 실행하는 파일이 아니라, 순수 PowerShell 의 명령입니다.

Cmdlet 명령의 결과는 개체로 표현합니다.
하나의 개체는 여러개의 속성을 가질 수 있으며, 다른 개체들을 하위에 둘 수 있습니다.
파일시스템, 레지스트리 등 유형별로 제공하는 필드는 다를 수 있습니다.


## Get-PSDrive

PowerShell 은 Windows 내의 다양한 리소스를 드라이브(Drive)라는 개체로 제공합니다.
드라이브는 다음과 같은 유형들이 있습니다.

-   파일 시스템에서 제공하는 정보 (ex: C:, D:)
-   레지스터리에서 제공하는 정보 (ex: HKLM:, HKCU:)
-   PowerShell 내부에서 제공하는 정보 (ex: Alias:, Cert:, Env:)


### 예시: 모든 드라이브 조회하기

이 예제는 PowerShell 에서 제공하는 모든 드라이브 개체를 조회합니다.

```
> Get-PSDrive

Name           Used (GB)     Free (GB) Provider      Root                                               CurrentLocation
----           ---------     --------- --------      ----                                               ---------------
Alias                                  Alias
C                 168.98        305.84 FileSystem    C:\                                                    
Cert                                   Certificate   \
Env                                    Environment
Function                               Function
HKCU                                   Registry      HKEY_CURRENT_USER
HKLM                                   Registry      HKEY_LOCAL_MACHINE
Variable                               Variable
WSMan                                  WSMan
```


## Get-Item

각 개체의 대한 정보는 `Get-Item` 명령으로 조회할 수 있습니다.
주로 개체의 정보를 출력하나 하위 개체의 목록을 출력하는 경우도 있습니다.

조회할 경로를 지정할 때, 드라이브 개체의 경우 접미사 `:` 를 붙여 사용합니다.
이후에 하위 경로의 개체명을 추가하여 조회할 수 있습니다.
첫번째 인자는 항상 `-Path` 이기 때문에 생략 가능합니다.


### 예시: 현재 디렉터리 가져오기

이 예제는 `FileSystem` 공급자를 사용하여, 현재 디렉터리를 가져옵니다.
문자(`.`)는 현재 위치를 나타내며, `C:.` 와 동일합니다.

```
> Get-Item -Path .

    디렉터리: C:\

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----      2024-00-00   오전 00:00                ps-test
```


### 예시: 지정한 경로의 모든 항목 가져오기

이 예제는 `FileSystem` 공급자를 사용하여, `C:\ps-test` 내의 모든 개체를 가져옵니다.
와일드 카드 문자(`*`)는 현재 개체의 모든 내용을 나타냅니다.

```
> Get-Item C:\ps-test\*

    디렉터리: C:\ps-test

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----      0000-00-00   오전 0:00              0 data.csv
-a----      0000-00-00   오전 0:00              0 test.txt

```


### 예시: 조회한 결과에서 특정 속성만 가져오기

이 예제는 `FileSystem` 공급자를 사용하여, `C:\ps-test\*` 내의 모든 개체를 가져옵니다.
이후 Name= 속성만을 선택하여 출력합니다.

```
> $(Get-Item C:\ps-test\*).Name
data.csv
test.txt
```


### 예시: 사용자 레지스트리 가져오기

이 예제는 `Registry` 공급자를 사용하여, 사용자 레지스트리의 개체를 가져옵니다.
`레지스트리 편집기` 에서 `HKEY_CURRENT_USER` 경로와 동일합니다.

```
> Get-Item HKCU:

    Hive:

Name                           Property
----                           --------
HKEY_CURRENT_USER
```


### 예시: 사용자 레지스트리의 모든 항목 가져오기

이 예제는 `Registry` 공급자를 사용하여 사용자 레지스트리의 모든 개체를 가져옵니다.
`레지스트리 편집기` 에서 `HKEY_CURRENT_USER` 의 하위 항목들과 동일합니다.

```
> Get-Item HKCU:*

    Hive: HKEY_CURRENT_USER

Name                           Property
----                           --------
AnchorSearchKiWoom
AppEvents
Console                        ColorTable00             : 789516
                               ColorTable01             : 14300928
                               ColorTable02             : 958739
...
```


### 예시: 사용자 레지스트리의 특정 항목 가져오기

이 예제는 `Registry` 공급자를 사용하여, 사용자 레지스트리에서 환경변수 `Environment` 개체를 가져옵니다.

```
> Get-Item HKCU:\Environment\

    Hive: HKEY_CURRENT_USER

Name                           Property
----                           --------
Environment                    ChocolateyLastPathUpdate : 133336991101727625
                               ChocolateyToolsLocation  : C:\tools
...
```


### 예시: Alias 조회하기

이 예제는 `Alias` 공급자를 사용하여, Alias 개체를 가져옵니다.

```
> Get-Item Alias:

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Alias           foreach -> ForEach-Object
Alias           % -> ForEach-Object
...
```


## Set-Item

개체의 값을 지정한 값으로 변경합니다.
만약 경로가 존재하지 않는 경우, 대부분 새로 생성하며 `New-Item` 과 동일합니다.


### 예시: 메모장에 대한 별칭 만들기

이 예제는 메모장의 별칭을 `np` 로 지정합니다.
`np` 명령을 사용하여, 메모장을 실행할 수 있습니다.

```
Set-Item -Path Alias:np -Value "C:\Windows\notepad.exe"
```


### 예시: 사용자 환경변수 변경하기

이 예제는 환경변수 하위 개체(`NVM_HOME`)의 값을 지정한 값으로 변경합니다.

```
> Set-Item ENV:NVM_HOME -Value C:\dev\nvm
```


## New-Item

새 항목을 만듭니다.


### 예시: 빈 파일 만들기

이 예제는 `C:\ps-test\empty.txt` 경로를 가지는 빈 파일을 생성합니다.

```
> New-Item empty.txt

    디렉터리: C:\ps-test

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----      0000-00-00   오전 00:00              0 empty.txt
```


### 예시: 디렉터리 만들기

이 예제는 `C:\ps-test` 내에 `Log` 디렉터리를 만듭니다.
ItemType 매개변수의 기본값은 `File` 이며, 디렉터리 생성을 위해 `Directory` 로 지정합니다.

```
> New-Item -Path C:\ps-test -Name Log -ItemType Directory

    디렉터리: C:\ps-test

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----      000-00-00  오전 00:00                Log
```


### 예시: 심볼릭 링크 만들기

이 예제는 `Log` 경로를 가르키는 심볼릭 링크 `MyLog` 를 만듭니다.
심볼릭 링크 생성의 경우 관리자 권한이 필요합니다.

```
> New-Item -Path C:\ps-test -Name MyLog -Value Log -ItemType SymbolicLink

    디렉터리: C:\ps-test

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d----l      2024-08-15  오전 10:04                MyLog

```


## Remove-Item

지정한 경로의 개체를 제거합니다.
이 명령은 `del`, `erase`, `rm`, `rm-dir` 등의 의 별칭을 가집니다.


### 예시: 파일 삭제하기

이 예제는 `empy.txt` 파일을 제거합니다.

```
> Remove-Item .\empty.txt
```


### 예시: 환경 변수 삭제하기

이 예제는 환경변수 목록에서 `NVM_HOME` 을 제거합니다.

```
> Remove-Item Env:NVM_HOME
```


## Get-ChildItem

하위 개체 항목들을 조회합니다.


### 예시: 특정 디렉터리의 하위 항목 조회하기

이 예제는 디렉터리 `C:\ps-test` 내의 하위 항목을 조회합니다.

```
> Get-ChildItem C:\ps-test

    디렉터리: C:\ps-test

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----      0000-00-00  오전  0:00                Log
d----l      0000-00-00  오전  0:00                MyLog
-a----      0000-00-00   오전 0:00              0 data.csv
-a----      0000-00-00   오전 0:00              0 test.txt
```


### 예시: 사용자 레지스트리 하위 개체 목록 조회하기

이 예제는 `레지스트리 편집기` 에서 `HKEY_CURRENT_USER` 의 내용과 동일한 결과를 출력합니다.

```
> Get-ChildItem HKCU:

    Hive: HKEY_CURRENT_USER

Name                           Property
----                           --------
AppEvents
Console                        ColorTable00             : 789516
...
```


## Get-ItemProperty

개체의 속성을 조회합니다.
개체 조회시 `Property` 필드로 되어 있는 경우 조회합니다.


### 예시: 사용자 레지스터리의 특정 항목 조회하기

이 예시는 사용자 레지스터리에서 `Environment` 개체의 속성을 조회합니다.

```
> Get-ItemProperty HKCU:\Environment\

NVM_HOME         : C:\dev\nvm
NVM_SYMLINK      : C:\dev\nodejs
...
```


## 참고

-   [PowerShell 설명서를 사용하는 방법](https://learn.microsoft.com/ko-kr/powershell/scripting/how-to-use-docs)

