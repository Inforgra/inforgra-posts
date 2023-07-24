---
title: 이것만 알아도 유용한 ssh-agent
description: 코드를 작성하고 GitHub와 같은 저장소에 소스를 올릴때나 원격 서버에 접속을 할 때 항상 마주치는 명령이 ssh 이다. 이 명령은 항상 계정과 암호를 요구하기 때문에 매번 입력야한다. 이런 불편한 점을 해결하고자 나온 도구가 ssh-agent이다. ssh-agent는 한번의 인증을 통해 암호를 매번 넣지 않아도 될 수 있도록 해 준다.
author: Kukjin Kang
date: 2023-07-03
tags: ["ssh-agent", "ssh"]
image: 
---

코드를 작성하고 GitHub와 같은 저장소에 소스를 올릴때나 원격 서버에
접속을 할 때 항상 마주치는 명령이 ssh 이다. 이 명령은 항상 계정과
암호를 요구하기 때문에 매번 입력야한다. 이런 불편한 점을 해결하고자
나온 도구가 ssh-agent이다. ssh-agent는 한번의 인증을 통해 암호를 매번
넣지 않아도 될 수 있도록 해 준다.


## ssh key 생성하기

ssh key는 아래와 같은 명령을 통해 생성할 수 있다. '-t' 는 암호화
방법을 정의하는데 'rsa'가 기본값이며 'dsa', 'ecdsa', 'ed25519' 를
사용할 수 있다. '-C'는 생성한 키를 설명하는 값이며 다른 키와 구분할 수
있도록 넣어준다. 일반적으로 이메일 주소를 많이 사용한다.

```bash
$ ssh-keygen -t raa -C "your_email@example.com"
```

## ssh-agent 실행하기

먼저 ssh-agent 가 어떻게 동작하는지 간단하게 알아보자.

ssh-agent 는 데몬으로 동작한다. 이는 서버 접속을 종료하더라도
ssh-agent 프로세스는 죽지않고 계속 실행중이라는 것을 의미한다.
프로세스가 실행하는 동안에는 서버 접속 및 종료와는 관계 없이 서비스를
받을 수 있다. 

ssh-agent 와의 통신은 파일소켓을 사용한다. 별도의 프로세스로 실행하기
때문에 메시지를 주고 받는 방법이 필요한데, 파일형태의 소켓을
사용한다.

다음과 같은 명령으로 ssh-agent를 실행하면 프로세스의 ID, 소켓의 위치를
알려준다. 명령을 여려번 실행하면 계속 프로세스가 실행되니 주의가 필요하다.

```bash
$ ssh-agent
SSH_AUTH_SOCK=/tmp/ssh-2K877hL8D46R/agent.29917; export SSH_AUTH_SOCK;
SSH_AGENT_PID=29918; export SSH_AGENT_PID;
echo Agent pid 29918;
```

이 값을 환경변수에 저장하면 ssh 관련 명령을 실행할 때 ssh-agent 의
도움을 받을 수 있다. 환경변수에 저장하기 위해서는 다음과 같이 명령을
실행한다.

```bash
$ $(ssh-agent)
```

ssh-agent 프로세스를 종료시키기 위한 별도의 명령은 없으며, 'kill -9
[pid]'를 통해 종료 할 수 있다.

## ssh-add 를 통한 키 관리

'ssh-add' 명령을 통해 ssh-agent에 키를 추가 할 수 있다.

```bash
$ ssh-add id_rsa
```

그리고 '-l' 옵션을 통해 등록한 키를 확인 할 수 있다.

```bash
$ ssh-add -l
```

## ssh-agent 를 지속적으로 사용하기

여기까지 진행했다면 ssh-agent를 사용하는 것은 크게 무리가 없을 것
이다. 그러나 재접속을 한다면 기존에 ssh-agent를 사용할 수 없다는
문제가 발생한다. ssh-agent를 실행할때 주어진 환경변수
`SSH_AUTH_SOCK`을 알 수 없기 때문이다.

다음 스크립트는 이런한 점을 보완한다. ssh-agent를 실행할 때 환경변수를
별도의 파일에 저장한다. 중복실행되는 경우 기존에 실행된 `pid`를
확인하여 기존 환경을 가져다 쓸 것인지, 새롭게 실행할 것인지를
결졍한다.


```bash
SSH_ENV="$HOME/.ssh/ssh-envrionment"

function start_agent {
    echo "Initialize ssh-agent..."
    /usr/bin/ssh-agent | sed 's/^echo/#echo/' > ${SSH_ENV}
    chmod 600 ${SSH_ENV}
    . ${SSH_ENV} > /dev/null
}

if [ -f ${SSH_ENV} ]; then
    . ${SSH_ENV} > /dev/null
    ps -ef | grep ${SSH_AGENT_PID} | grep ssh-agent$ > /dev/null || { start_agent; }
else
    start_agent
fi
```

위의 스크립트를 `~/.ssh/ssh.sh`에 저장한다. 매 접속시마다 이 명령을
사용할 수 있도록 `'~/.bash_profile`, `~/.bashrc` 또는 `~/.profile`에
라인을 추가한다.

```bash
source ~/.ssh/ssh.sh
```

