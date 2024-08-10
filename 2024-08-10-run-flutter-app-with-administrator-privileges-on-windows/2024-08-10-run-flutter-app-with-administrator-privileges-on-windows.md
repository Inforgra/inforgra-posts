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

[Flutter](https://flutter.dev) 에서 윈도우 앱을 빌드할 때 관리자 권한으로 실행할 수 있도록 설정하는 방법은 다음과 같습니다.

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

## 참고

- [How to launch other app(go service) when flutter windows app start?](https://stackoverflow.com/questions/71090014/how-to-launch-other-appgo-service-when-flutter-windows-app-start)
