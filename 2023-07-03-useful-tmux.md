---
title: 이것만 알아도 유용한 tmux
description: 한번에 다수의 창(터미널)을 생성하고, 중단 없이 작업을 할 수 있도록 하는 것이 tmux의 주요 기능이다. 세세한 부분은 공식 문서에 잘 정리되어 있으니 여기서는 설치, 실행과 몇 가지 중요한 명령만 소개한다.
author: Kukjin Kang
date: 2023-07-03
tags: ["tmux"]
image: https://i.ibb.co/wy23TJ9/useful-tmux.png
---

웹 서비스를 개발, 딥러닝 학습 등 원격 서버에 접속을 해서 작업을 해야하는 경우 몇
가지 애로 사항이 발생한다. 첫번째는 한번에 많은 터미널을 열어야 하는
경우다. 서버를 실행하고, 로그를 보면서 코드를 변경 하려면 적어도 3개의 세션이
필요하다. 두번째는 접속을 종료한 이후에도 실행한 작업을 유지하고 싶은
경우이다. 보통 서버 접속 종료를 하게되면, 모든 프로세스도 같이 종료되기 때문에
데몬과 같은 다른 형태의 프로그램으로 개발이 필요하다.

한번에 다수의 세션을 오픈하며, 중단 없이 작업을 할 수 있도록 하는 것이 tmux의
주요 기능이다. 세세한 부분은 [tmux Getting
Started](https://github.com/tmux/tmux/wiki/Getting-Started) 와 같이
공식 문서에 잘 정리 되어 있으니 여기서는 설치, 실행과 몇 가지 중요한
명령만 소개한다. 

## 설치하기

CentOS, Ubuntu등의 리눅스 배포판에서는 패키지 형태로 제공한다. 각 배포판에 맞는
명령을 수행하여 설치하자.

```bash
# for CentOS
$ sudo yum install -y tmux

# for Ubuntu
$ sudo apt install -y tmux
```

이제 tmux 명령만 실행하면 된다. 그러나 이후에 많은 설정과 단축키로
무었을 해야할지 고민스럽다. 이를 해결하기 위해 [Useful
tmux](https://github.com/Inforgra/useful-tmux) 를 추천한다. 이 명령은
몇 가지 tmux plugin 을 추가 설치하며, .tmux, .tmux.conf의 파일을 추가한다.

```bash
$ git clone https://github.com/Inforgra/useful-tmux
$ cd useful-tmux
$ ./init.sh
```

다음 명령을 먼저 실행하고 tmux를 실행해 보자. 이 스크립트는 하나의 계정에 하나의
세션을 쓰게 해준다. 세션명은 계정명과 같은 것으로 사용한다.

```bash
$ . ~/.tmux/tmux.sh
$ tmux
```

<img src="https://i.ibb.co/wy23TJ9/useful-tmux.png" width="100%" />

위 예시는 windows powershell 에서 서버에 접속하여 tmux를 실행한 결과이다. 총
3개의 창이 열려 있으며, "0"번 창이 현재 창을 표시한다.


## 주요명령

주요 명령을 알아보기 전에 세션에 대한 개념을 이해해 보자. 하나의 세션은 1개
이상의 창을 가질 수 있다. 세션을 생성하면 서버를 종료시키거나 모든 창을 닫기까지
종료되지 않는다.

주요명령은 다음과 같다. 새로운 창을 닫는 방법은 'exit' 명령으로 터미널을
종료하면 된다. 이제 익숙해질 때까지 몇 번 명령을 수행해보자.

| 단축키 | 명령 |
|---|---|
| C+\ c  | 새로운 창을 생성 |
| C+\ d  | 세션 밖으로 이동 |
|  F11   | 이전 창으로 이동 |
|  F12   | 다음 창으로 이동 |

복사 붙이기, 창을 세로로 나누기 등 좀 더 깊이 있게 들어가고 싶으신 분은 [tmux
공식문서](https://github.com/tmux/tmux/wiki/Getting-Started) 를 한번 읽어보자.


## 참고

이 문서에서 사용하는 tmux plugin 은 다음과 같다.

-   [Tmux Plugin Manager](https://github.com/tmux-plugins/tpm)
-   [Tmux sensible](https://github.com/tmux-plugins/tmux-sensible.git)
-   [Catppuccin for Tmux](https://github.com/catppuccin/tmux)

