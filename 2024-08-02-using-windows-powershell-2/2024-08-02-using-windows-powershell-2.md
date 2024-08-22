---
published: true
title: Windows PowerShell 활용하기 (2) - 파이프라인
summary: PowerShell 은 Windows 에서 제공하는 기능들을 명령으로 실행할 수 있는 강력한 도구로서 개발자에게 유용한 옵션입니다.
date: 2024-08-02 00:00:00.0+09:00
image: powershell.jpg
imageAlt: Microsoft PowerShell
tags:
- windows
- powershell
- pipeline
---

PowerShell 에서 파이프라인에 명령을 결합하는 방법을 살펴봅니다.
다른 주제는 아래를 목록을 참고 하시면 됩니다.

-   [Windows PowerShell 활용하기 (1)  - Cmdlet](https://inforgra.com/posts/2024-08-01-using-windows-powershell-1)
-   [Windows PowerShell 활용하기 (2)  - 파이프라인](https://inforgra.com/posts/2024-08-02-using-windows-powershell-2)
-   [Windows PowerShell 활용하기 (3)  - 환경변수 관리하기](https://inforgra.com/posts/2024-08-03-using-windows-powershell-3)
-   [Windows PowerShell 활용하기 (4)  - 프로세스](https://inforgra.com/posts/2024-08-04-using-windows-powershell-4)
-   Windows PowerShell 활용하기 (5)  - 서비스
-   Windows PowerShell 활용하기 (6)  - 패키지


# Pipeline 이란

파이프라인은 연산자 `|` 로 연결한 일련의 명령입니다.
각 파이프라인 연산자는 이전 명령의 결과를 다음 명령의 입력으로 보냅니다.

아래의 예시에서 첫번째 명령 `Command-1` 의 결과는 두번째 명령 `Command-2` 의 입력으로 전송합니다.
두번째 명령 `Command-2` 을 처리하고, 세번째 명령 `Command-3` 에 전송합니다.
더 이상 처리할 명령이 없으면 콘솔에 표시합니다.

```
Command-1 | Command-2 | Command-3
```


# Get-Member

입력 개체에서 제공하는 멤버 정보를 출력합니다.
개체의 속성이나 메소드 등이 멤버가 될 수 있습니다.


## 예제: 디렉토리의 멤버 출력하기

이 예제는 디렉토리 개체의 멤버를 출력합니다.

```
> Get-Item . | Get-Member

   TypeName: System.IO.DirectoryInfo

Name                      MemberType     Definition
----                      ----------     ----------
LinkType                  CodeProperty   System.String LinkType{get=GetLinkType;}
Mode                      CodeProperty   System.String Mode{get=Mode;}
...
```


## 예시: 프로세스의 멤버 출력하기

이 예제는 프로세스 개체의 멤버를 출력합니다.

```
> Get-Process | Get-Member

   TypeName: System.Diagnostics.Process

Name                       MemberType     Definition
----                       ----------     ----------
Handles                    AliasProperty  Handles = Handlecount
Name                       AliasProperty  Name = ProcessName
NPM                        AliasProperty  NPM = NonpagedSystemMemorySize64
...
```


# Select-Objecet

입력한 개체에서 특정 멤버를 선택합니다.


## 예제: 프로세스 목록에서 이름과 CPU 사용량 출력하기

이 예제는 프로세스 개체에서 `Name`, `CPU` 멤버를 선택하여 출력합니다.
문자(`,`)를 구분자로 하여 여러개의 멤버를 선택할 수 있습니다.

```
> Get-Process | Select-Object Name,CPU

Name                                  CPU
----                                  ---
Adguard                          23.34375
Adguard.BrowserExtensionHost     3.703125
AdguardSvc
AggregatorHost
AppHelperCap
AppleMobileDeviceProcess                0
ApplePhotoStreams                  0.0625
```


# Sort-Obeject

입력한 개체를 정렬합니다.
오름차순(Ascending) 정렬을 기본으로 하며, 내림차순(Descending) 정렬을 위해서는 `-Descending` 옵션을 사용합니다.


## 예제: 프로세스 목록에서 CPU 사용량이 높은 순으로 출력하기

이 예제는 프로세스 개체에서 `CPU` 사용량을 내림차순으로 정렬하고, `Name`, `CPU` 멤버를 선택하여 출력합니다.

```
> Get-Process | Sort-Object CPU -Descending | Select-Object Name,CPU

Name                                    CPU
----                                    ---
emacs                            248.828125
MiniBin                                  79
OmenCommandCenterBackground           74.25
Adguard                           24.171875
Dropbox                           21.703125
```


# Where-Object

특정 개체를 선택하여 반환합니다.


## 예제: 특정 프로세스 목록 조회하기

```
> Get-Process | Where-Object {$_.Name -eq "msedge"}

Handles  NPM(K)    PM(K)      WS(K)     CPU(s)     Id  SI ProcessName
-------  ------    -----      -----     ------     --  -- -----------
    436      28   504576     129156       6.11   5088   1 msedge
    367      25    38472      10084       0.42   5320   1 msedge
    286      22    26700       7184       0.05  11212   1 msedge
    248      14     8216      23328       0.06  13280   1 msedge
    367      25    56584     107036       1.28  17444   1 msedge
```


# Measure-Object

개체의 속성 값을 계산합니다.
글자수 `-Character`, 단어수 `-Word`, 라인수 `-Line` 등의 옵션을 사용할 수 있습니다.


## 예제: 전체 프로세스의 개수 조회하기

```
> Get-Process | Measure-Object

Count    : 281
Average  :
Sum      :
Maximum  :
Minimum  :
Property :
```


## 예제: 전체 프로세스의 개수 조회하기

갯수 계산의 경우 `Count` 멤버를 사용할 수 있습니다.

```
> $(Get-Process).Count
281
```


## 예제: 텍스트 파일 내의 글자수, 단어수, 라인수 조회하기

```
> Get-Content .\test.txt | Measure-Object -Character -Word -Line

Lines Words Characters Property
----- ----- ---------- --------
  116   383       3820
```

