---
title: update-alternatives 사용하여 여려 버전의 패키지 관리하기
description: update-alternatives 사용하여 여려 버전의 패키지 관리하는 방법을 python 을 예시로 살펴봅니다.
author: Kukjin Kang
date: 2023-07-31
tags: ["update-alternatives"]
image:
---

## 개요

버전이 다른 패키지(또는 명령어)에 대해 기본 버전이나 경로를 설정해야
하는 경우가 종종 발생합니다. 예를 들어 python2, python3가 같이
설치되어 있을 때, python 명령을 python3로 사용하고 싶은 경우입니다.

가장 간단한 해결책은 `alias`를 쓰는 것입니다. 콘솔에서는 환경변수로
지정이 되어 있어 사용하는 것은 무리가 없지만, 배치와 같이 별도의
환경에서 실행할 때에는 해결책이 되지 않습니다.

두번째로 생각해 볼 수 있는 것은 심볼릭 링크(symbolic link)를 걸어
사용하는 것입니다. `/usr/bin/python`을 `/usr/bin/python3`에 링크해
두면 `python` 명령을 `python3`으로 사용할 수 있습니다. `python` 만
사용한다면 크게 문제가 없지만, `python-config` 등 다른 명령과 같이
사용한다면 링크를 관리하는 것도 쉽지 않은 일이 됩니다.

패키지 별로 심볼릭 링크들을 관리해주는 프로그램이
`update-alternatives`입니다. 우분투, CentOS, RedHat 등 다양한
배포판에서 사용할 수 있으니 한번 알아 두면 큰 도움이 될것입니다.

## 설치 (--install)

설치를 하기 위한 옵션은 다음과 같습니다.

```bash
update-alternatives --install <link> <name> <path> <priority>
```

- link: 생성할 심볼릭 링크의 경로명
- name: /etc/alternatives 내 생성할 이름 (보통 패키지명을 사용)
- path: 실제 실행할 명령의 경로명
- priority: 자동으로 동작할 때 우선 순위 (높을 수록 먼저 사용)

udpate-alternatives 를 사용하여 python2, python3 의 링크는 다음과
같이 생성할 수 있습니다. 같은 심볼릭 링크(/usr/bin/python)에 대해
python2, python3 버전을 각각 생성하였습니다.

```bash
$ sudo update-alternatives --install /usr/bin/python python /usr/bin/python2 200
$ sudo update-alternatives --install /usr/bin/python python /usr/bin/python3 300
```


## 표시 (--display)

생성한 심볼릭 링크가 어떤 경로를 사용하는지 확인하려면 `--display`
명령을 사용합니다.

```bash
update-alternatives --display <name>
```

앞에서 생성한 python2, python3 에 대해 `/usr/bin/python`이 어떤
경로를 사용하는지 확인할 수 있습니다. 우선순위가 높은 `python3`을
사용하는 것을 볼 수 있습니다.

```bash
$ sudo update-alternatives --display python
python - auto mode
  link best version is /usr/bin/python3
  link currently points to /usr/bin/python3
  link python is /usr/bin/python
/usr/bin/python2 - priority 200
/usr/bin/python3 - priority 300
```


## 설정 (--config)

생성한 심볼릭 링크의 경로는 보통 우선순위에 따라 결정되지만, `--config`
명령을 사용하여 변경할 수 있습니니다.

```
update-alternatives --config <name>
```

현재는 `/usr/bin/python3`이 자동으로 선택한 것을 볼 수 있으며, 번호를 지정하여,
`/usr/bin/python2`로 변경할 수도 있습니다.

```bash
$ sudo update-alternatives --config python
There are 2 choices for the alternative python (providing /usr/bin/python).

  Selection    Path              Priority   Status
------------------------------------------------------------
* 0            /usr/bin/python3   300       auto mode
  1            /usr/bin/python2   200       manual mode
  2            /usr/bin/python3   300       manual mode
Press <enter> to keep the current choice[*], or type selection number:
```


## 삭제 (--remove)

패키지를 제거하거나 필요가 없어지 경우 `--remove` 명령을 사용하여,
해당 링크를 제거 할 수 있습니다.

```bash
update-alternatives --remove <name> <path>
```

만약 우선순위가 높은 `python3`을 제거하더라도 `/usr/bin/python`은 없어지지 않습니다.
여기서는 다음 순위인 `python2`가 자동으로 링크되는 것을 볼 수 있습니다.

```bash
$ sudo update-alternatives --remove python /usr/bin/python3
update-alternatives: using /usr/bin/python2 to provide /usr/bin/python (python) in auto mode

$ sudo update-alternatives --display python
python - auto mode
  link best version is /usr/bin/python2
  link currently points to /usr/bin/python2
  link python is /usr/bin/python
/usr/bin/python2 - priority 200
```

## 참고

여기서 설명한 update-alternatives 의 명령 이외에도 `--set`, `--query`,
`--list` 등 다양한 명령이 있습니다. 좀 더 깊은 내용을 보고 싶으신 분은
man 페이지를 한번 읽어 보시길 권해 드립니다.

-
