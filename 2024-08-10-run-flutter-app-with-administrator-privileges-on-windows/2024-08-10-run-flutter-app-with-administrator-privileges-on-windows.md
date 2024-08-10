---
published: true
title: Flutter 앱을 윈도우 관리자 권한으로 실행하기
date: 2024-08-10 09:00:00.0+09:00
image: flutter.png
imageAlt: Flutter transforms the entire app development process. Build, test, and deploy beautiful mobile, web, desktop, and embedded apps from a single codebase.
summary: Flutter 에서 윈도우 앱을 빌드할 때 관리자 권한으로 실행할 수 있도록 설정하는 방법을 살펴보겠습니다.
tags:
  - flutter
  - windows
  - tip
---

윈도우에서 서비스를 시작, 정지하거나 프로그램을 설치, 삭제할 때와 같이 시스템 변경이 필요한 경우에 관리자 권한이 필요합니다. 이러한 기능을 가진 앱들은 빌드시에 관리자 권한을 요청하는 설정을 포함해야 합니다.

[Flutter](https://flutter.dev) 에서 윈도우 앱을 빌드할 때 관리자 권한으로 실행할 수 있도록 설정하는 방법을 살펴보도록 하겠습니다.

## 관리자 권한 설정 추가하기 

`CMakeLists.txt` 마지막 줄에 다음 내용을 추가합니다.

```bash filename=./windows/runner/CMakeLists.txt
set_target_properties(${BINARY_NAME} PROPERTIES LINK_FLAGS "/MANIFESTUAC:\"level='requireAdministrator' uiAccess='false'\" /SUBSYSTEM:WINDOWS")
```

`runner.exe.manifest` 에 `trustInfo` 를 추가합니다.

```filename=./windows/runner/runner.exe.manifest
<assembly>
  ...
  <trustInfo xmlns="urn:schemas-microsoft-com:asm.v2">
    <security>
      <requestedPrivileges>
        <requestedExecutionLevel level="requireAdministrator" uiAccess="false"/>
      </requestedPrivileges>
    </security>
  </trustInfo>
</assembly>
```

## 관리자 권한으로 실행하기

일반 권한에서 프로그램을 실행하면 다음과 같은 오류가 발생합니다. 일반 권한으로 실행한 `VSCode` 내에서 바로 실행해도 같은 오류가 나옵니다.

```
PS > flutter run -d windows
Launching lib\main.dart on Windows in debug mode...
Building Windows application...                                     7.4s
√ Built build\windows\x64\runner\Debug\flutter_01.exe
Unable to start executable "build\windows\x64\runner\Debug\flutter_01.exe": ProcessException: 요청한 작업을 수행하려면  권한 상승이
필요합니다.

  Command: build\windows\x64\runner\Debug\flutter_01.exe
ProcessException: 요청한 작업을 수행하려면 권한 상승이 필요합니다.

  Command: build\windows\x64\runner\Debug\flutter_01.exe
```

관리자 권한을 가진 `powershell` 에서 `run` 명령을 실행해야 합니다.

```
PS > flutter run -d windows
Launching lib\main.dart on Windows in debug mode...
Building Windows application...                                     6.9s
√ Built build\windows\x64\runner\Debug\flutter_01.exe
Syncing files to device Windows...                                  40ms

Flutter run key commands.
r Hot reload.
R Hot restart.
h List all available interactive commands.
d Detach (terminate "flutter run" but leave application running).
c Clear the screen
q Quit (terminate the application on the device).

A Dart VM Service on Windows is available at: http://127.0.0.1:57107/ltOd3P3TpRo=/
The Flutter DevTools debugger and profiler on Windows is available at:
http://127.0.0.1:9100?uri=http://127.0.0.1:57107/ltOd3P3TpRo=/
```

## 빌드하기

빌드한 결과물을 일반 권한으로 실행하면, 관리자 권한을 요청하는 알림을 띄웁니다. 여기서 관리자 권한을 승인하여 프로그램을 실행할 수 있습니다. 

## 참고

- [How to launch other app(go service) when flutter windows app start?](https://stackoverflow.com/questions/71090014/how-to-launch-other-appgo-service-when-flutter-windows-app-start)
