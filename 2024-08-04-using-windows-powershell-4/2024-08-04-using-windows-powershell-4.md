---
published: true
title: Windows PowerShell 활용하기 (4) - 프로세스 관리하기
summary: PowerShell 의 Process Cmdlet 을 사용하면 프로세스를 관리할 수 있습니다. 프로세스의 조회, 중지 등의 명령을 내릴 수 있으며, 파이프라인을 통해 다양한 작업을 할 수 있습니다.
date: 2024-08-04 00:00:00.0+09:00
image: powershell.jpg
imageAlt: Microsoft PowerShell
tags:
- windows
- powershell
- process
---

Windows 에서 프로세스를 확인하거나 종료할 때 `작업 관리자` 를 주로 사용합니다.

PowerShell 의 Process Cmdlet 을 사용하면 프로세스를 관리할 수 있습니다.
프로세스의 조회, 중지 등의 명령을 내릴 수 있으며, 파이프라인을 통해 다양한 작업을 할 수 있습니다.

-   [Windows PowerShell 활용하기 (1)  - Cmdlet](https://inforgra.com/posts/2024-08-01-using-windows-powershell-1)
-   [Windows PowerShell 활용하기 (2)  - 파이프라인](https://inforgra.com/posts/2024-08-02-using-windows-powershell-2)
-   [Windows PowerShell 활용하기 (3)  - 환경변수 관리하기](https://inforgra.com/posts/2024-08-03-using-windows-powershell-3)
-   [Windows PowerShell 활용하기 (4)  - 프로세스](https://inforgra.com/posts/2024-08-04-using-windows-powershell-4)
-   Windows PowerShell 활용하기 (5)  - 서비스
-   Windows PowerShell 활용하기 (6)  - 패키지


# 프로세스 조회하기

`Get-Process` 명령을 사용하여 프로세스를 조회할 수 있습니다.

```
> Get-Process

Handles  NPM(K)    PM(K)      WS(K)     CPU(s)     Id  SI ProcessName
-------  ------    -----      -----     ------     --  -- -----------
    697      72   173328      22832      88.59  13620   1 Adguard
    473      33    70956      76104       2.02  11340   1 Adguard.BrowserExtensionHost
   1249     123   482660      27616              4280   0 AdguardSvc
...
```

`-Id` 매개변수를 사용하여 지정한 `Id` 프로세스를 조회할 수 있습니다.

```
> Get-Process -Id 13620

Handles  NPM(K)    PM(K)      WS(K)     CPU(s)     Id  SI ProcessName
-------  ------    -----      -----     ------     --  -- -----------
    690      72   181880      20444      88.94  13620   1 Adguard
```

`-Name` 매개변수를 사용하여 지정한 이름을 가진 프로세스를 조회할 수 있습니다.

```
> Get-Process -Name Adguard*

Handles  NPM(K)    PM(K)      WS(K)     CPU(s)     Id  SI ProcessName
-------  ------    -----      -----     ------     --  -- -----------
    702      72   181980      22448      88.98  13620   1 Adguard
    573      34    71396      61740       2.11  11340   1 Adguard.BrowserExtensionHost
   1205     121   482484      29264              4280   0 AdguardSvc
```

`Measure-Object` 명령을 사용하여, 전체 프로세스의 수를 확인할 수 있습니다.

```
> Get-Process | Measure-Object

Count    : 254
Average  :
Sum      :
Maximum  :
Minimum  :
Property :
```

`Select-Object` 명령을 파이프라인으로 연결하여 조회할 속성과 갯수를 지정할 수 있습니다.

```
t> Get-Process | Select-Object Id,ProcessName | Select-Object -First 10

   Id ProcessName
   -- -----------
13620 Adguard
11340 Adguard.BrowserExtensionHost
 4280 AdguardSvc
 3676 AggregatorHost
13756 AppleMobileDeviceProcess
 4480 ApplePhotoStreams
14016 ApplicationFrameHost
 6220 AppVShNotify
13452 APSDaemon
14536 AutoHotkeyU64
```

`Where-Object` 명령을 파이프라인으로 연결하여 특정 프로세스를 조회할 수 있습니다.

```
> Get-Process | Where-Object ProcessName -eq "Notepad"

Handles  NPM(K)    PM(K)      WS(K)     CPU(s)     Id  SI ProcessName
-------  ------    -----      -----     ------     --  -- -----------
    759      34    62540     107108       0.22  20664   1 Notepad
```

`Sort-Object` 명령을 파이프라인으로 연결하여 CPU 사용량이 높은 프로세스를 조회할 수 있습니다.
프로세스의 수가 많기 때문에 `Select-Object` 명령을 파이프라인하여 조회 범위를 지정할 수 있습니다.

```
> Get-Process | Sort-Object CPU -Descending | Select-Object -First 10

Handles  NPM(K)    PM(K)      WS(K)     CPU(s)     Id  SI ProcessName
-------  ------    -----      -----     ------     --  -- -----------
  10493     172   377448     306512     469.50  14728   1 Dropbox
   1441     106   184488     204960     412.11   2940   1 emacs
   1943      64    82820      89760     324.17  13992   1 iCloudHome
    596      24    36492      33952     186.39  13000   1 ctfmon
    737      72   179088      20788      88.77  13620   1 Adguard
    383      25    59388      81588      86.75   5128   1 Dropbox
   2030     246   125568     239312      72.92  19780   1 msedge
    718      35    88704      91560      69.36   4064   1 WindowsTerminal
    489      25    93524     128800      55.91   3756   1 msedge
    259      21    25216      30992      55.55   9292   1 wireguard
```


# 프로세스 중지하기

`Stop-Process` 명령을 사용하여 프로세스를 중지할 수 있습니다.

이 명령은 프로세스ID `-Id` 또는 프로세스명 `-Name` 매개변수를 사용하여, 특정 프로세스를 지정할 수 있습니다.

```
> Stop-Process -Name Notepad
```

```
> Stop-Process -Id 20664
```

와일드카드 `*` 와 같이 여러 프로세스를 중지하는 경우 원하지 않는 프로세스를 중지할 수 있습니다.
`-Confirm` 매개 변수를 사용하여, 프로세스를 확인하는 절차를 추가할 수 있습니다.

```
> Stop-Process -Name Note* -Confirm

확인
이 작업을 수행하시겠습니까?
대상 "Notepad (8696)"에서 "Stop-Process" 작업을 수행합니다.
[Y] 예(Y)  [A] 모두 예(A)  [N] 아니요(N)  [L] 모두 아니요(L)  [S] 일시 중단(S)  [?] 도움말 (기본값은 "Y"): y
```

`Get-Process` 를 파이프라인하여, 프로세를 중지할 수 있습니다.

```
> Get-Process -Name Notepad | Stop-Process
```

