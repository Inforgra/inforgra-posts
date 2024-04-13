---
published: true
title: SSH 키를 생성하고, ssh-agent 에서 사용하기
summary: SSH 프로토콜은 SSH 키를 사용합니다. 새로운 SSH 키를 생성하고, ssh-agent 에 추가하는 방법을 알아보겠습니다.
date: 2024-04-09
image: ssh-agent.png
imageAlt: ssh
tags: 
  - ssh
  - ssh-agent
---

SSH (Secure Shell Protocol) 는 암호화 네트워크 프로토콜 입니다. 이 프로토콜을 사용하면 원격 서비스에 연결하고 인증할 수 있습니다. SSH 키를 사용하면 암호를 사용하지 않고, 연결할 수 있습니다.

SSH 키가 없는 경우 로컬에서 `ssh-keygen` 명령을 사용하여 새로운 키를 생성할 수 있습니다. 키를 생성한 후, 원격 서비스에 공개키를 등록하여 활성화 할 수 있습니다.  만약 여러개의 키와 원격 서비스를 사용하는 경우 별도의 설정을 통해 키와 원격 서비스를 대응 시킬 수 있습니다.

SSH 키는 생성할 때 암호를 추가하여, 키를 더욱 안전하게 보호할 수 있습니다. 이 키는 사용할 때 마다 암호를 입력해야 합니다. 키에 암호가 있고 사용할 때마다 암호를 입력하지 않고 싶은 경우 `ssh-agent` 에 키를 추가하여 사용하면 됩니다.

## SSH 키 생성하기

`ssh-keygen` 명령을 사용하여 SSH 키를 생성합니다. 이 명령에서 주로 사용하는 옵션들은 다음과 같습니다.

- -t: 키의 타입 (dsa, rsa, ed25519, ...)
- -b: 키의 크기 (rsa 의 경우 기본값은 3072 bit)
- -C: 키의 설명 (주로 이메일을 사용)

> [!NOTE]
> Github 에서 SSH 키 타입을 ed25519 로 생성하는 것을 추천합니다. RSA 타입을 사용하는 경우에는 키의 크기를 4096 을 사용하는 것을 추천하며,  DSA 타입은 지원하지 않습니다.

=== Mac

  터미널에서 `ssh-keygen` 명령을 실행합니다.

  ```bash
  % ssh-keygen -t rsa -C "your_email@example.com"
  ```
  
  키를 저장하는 경로를 물어보는 프롬프트에서 생성하는 키의 위치를 변경할 수 있습니다. 엔터를 입력하면 기본값($HOME/.ssh/id_rsa)으로 저장합니다. 만약 키가 이미 존재한다면, 덮어 쓸지 여부를 확인합니다.
  
  ```bash
  Enter file in which to save the key (\Users\admin\.ssh\id_rsa):
  ```
  
  키의 비밀번호를 물어보는 프롬프트에서 암호를 설정할 수 있습니다. 엔터를 입력하면 암호가 없는 키를 생성합니다. 입력한 암호가 맞는지 확인하기 위해 한번 더 암호를 입력합니다.
  
  ```bash
  Enter passphrase (empty for no passphrase):
  Enter same passphrase again:
  ```

=== Windows

  명령 프롬프트에서 `ssh-keygen` 명령을 실행합니다.

  ```bash
  C:\Users\admin> ssh-keygen -t rsa -C "your_email@example.com"
  ```
  
  키를 저장하는 경로를 물어보는 프롬프트에서 생성하는 키의 위치를 변경할 수 있습니다. 엔터를 입력하면 기본값($HOME/.ssh/id_rsa)으로 저장합니다. 만약 키가 이미 존재한다면, 덮어 쓸지 여부를 확인합니다.
  
  ```bash
  Enter file in which to save the key (C:\Users\admin\.ssh\id_rsa):
  ```
  
  키의 비밀번호를 물어보는 프롬프트에서 암호를 설정할 수 있습니다. 엔터를 입력하면 암호가 없는 키를 생성합니다. 입력한 암호가 맞는지 확인하기 위해 한번 더 암호를 입력합니다.
  
  ```bash
  Enter passphrase (empty for no passphrase):
  Enter same passphrase again:
  ```

=== Linux

  터미널에서 `ssh-keygen` 명령을 실행합니다.

  ```bash
  $ ssh-keygen -t rsa -C "your_email@example.com"
  ```

  키를 저장하는 경로를 물어보는 프롬프트에서 생성하는 키의 위치를 변경할 수 있습니다. 엔터를 입력하면 기본값 ( ~/. ssh/id_rsa)  으로 저장합니다. 만약 키가 이미 존재한다면, 덮어 쓸지 여부를 물어 봅니다.

  ```bash
  Enter file in which to save the key (~/.ssh/id_rsa): [Enter]
  ```

  키의 비밀 번호를 물어보는 프롬프트에서 암호를 설정할 수 있습니다. 엔터를 입력하면 암호가 없는 키를 생성합니다. 입력한 암호가 맞는지 확인하기 위해 한번 더 암호를 입력합니다.

  ```bash
  Enter passphrase (empty for no passphrase): [Enter]
  Enter same passphrase again: [Enter]
  ```

## 여러개의 SSH 키를 사용하기 

다수의 SSH 키와 원격 서비스가 있다고 가정해 봅시다. 이때 어떤 키를 어떤 서비스에 적용해야 할지 알 수 없습니다. `~/.ssh/config` 에서 키와 원격 서비스를 대응 방법을 정의 할 수 있습니다.

먼저 여러개의 키와 원격 서비스의 맵핑 방법은 아래와 같이 세가지 경우가 있습니다.

- 1:1 - 한개의 키를 하나의 원격 서비스에 사용
- N:1 - 여러개의 키를 하나의 원격 서비스에 사용
- 1:N - 한개의 키를 여러개의 원격 서비스에 사용

하나의 키를 여러개의 원격서비스에 사용하는 1:N  경우는 1:1 방식을 여러번 사용하는 것으로 대체할 수 있습니다. 1:1, N:1  방법만 이해하면 모든 경우에서 적용할 수 있습니다.

###  하나의 키를 하나의 원격 서비스에 사용하기 (1:1)

두개의 키 `id_rsa-a`, `id_rsa-b` 가 있고, 두개의 원격 서비스 `a.com` `b.com` 가 있습니다. 각각의 키를 서비스에 적용하기 위한 설정은 다음과 같습니다.

```filename=~/.ssh/config
HostName a.com
User username-a
IdentityFile ~/.ssh/id_rsa-a

HostName b.com
User username-b
IdentityFile ~/.ssh/id_rsa-b
```

###  여러개의 키를 하나의 원격 서비스에 사용하기 (N:1)

두개의 키 `id_rsa-a`, `id_rsa-b` 를 하나의 원격 서비스 `a.com` 이 있습니다. 같은 HostName 을 가지기 때문에 1:1 설정 방법을 적용할 수 없습니다.

이 때에는 Host 변수를 추가하여, 별도의 이름을 지정할 수 있습니다. Host  에서 지정한 이름으로 원격 서비스에 접속하면 다른 키를 사용하여 하나의 원격 서비스에 접속 할 수 있습니다.
```
Host key-a.a.com
HostName a.com
User username-a
IdentityFile ~/.ssh/id_rsa-a

Host key-b.a.com
HostName a.com
User username-b
IdentityFile ~/.ssh/id_rsa-b
```
 
## ssh-agent 실행하기

=== Mac

  MacOS 는 필요에 따라 `ssh-agent` 를 실행합니다. 

=== Windows

  관리자 권한으로 `powershell` 을 실행하고 다음 명령을 입력합니다
  
  ```bash
  Get-Service -Name ssh-agent | Set-Service -StartupType Manual
  Start-Serviec ssh-agent
  ```
  
  StartupType 이 "Manual" 인 경우 시스템 부팅시 매번 서비스를 시작하는 명령을 입력해야합니다. "Automatic" 으로 변경하면 부팅시에 자동으로 서비스를 시작합니다.
  

=== Linux

  실행 방법을 알아보기 전에, 동작 방식에 대해 살펴보겠습니다.

  1. 프로세스는 데몬(daemon)으로 동작합니다. 부모 프로세스는 명령을 실행한 터미널(또는 명령 프롬프트)이 아닌 최상위 프로세스(보통 initd) 입니다. 터미널을 닫거나 원격 서비스를 연결을 종료 하더라도 `ssh-agent` 서비스를 계속 사용할 수 있습니다.
  2. 프로세스의 통신은 파일 소켓을 사용합니다. 이 파일 소켓을 통해 키 등록, 조회 등의 메시지를 주고 받습니다. 터미널 환경에서 `SSH_AUTH_SOCK` 변수의 값을 소켓을 경로로 사용합니다.

  `ssh-agent` 명령을 실행하면, 두 개의 환경 변수를 출력합니다. 

  - SSH_AUTH_SOCK: 파일 소켓 경로
  - SSH_AGENT_PID: 프로세스의 ID

  이 명령을 여러번 실행하는 경우,  같은 갯수의 프로세스를 생성합니다. 여러번 생성하지 않도록 주의가 필요합니다.

  ```bash
  $ ssh-agent
  SSH_AUTH_SOCK=/tmp/ssh-2K877hL8D46R/agent.29917; export SSH_AUTH_SOCK;
  SSH_AGENT_PID=29917; export SSH_AGENT_PID; 
  echo Agent pid 29917;
  ```

  이 결과를 환경 변수에 반영하려면,  `eval` 명령과 함께 실행합니다. `echo` 명령으로 설정 값을 확인할 수 있습니다.
  
  ```bash
  $ eval "$(ssh-agent)" 
  $ echo $SSH_AUTH_SOCK 
  /tmp/ssh-2K877hL8D46R/agent.29917 
  $ echo $SSH_AGENT_PID 
  29917
  ```

  `kill` 명령을 사용하여 프로세스를 종료할 수 있습니다.

  ```bash
  $ kill -9 $SSH_AGENT_PID
  ```

   ssh-agent 환경 변수를 이용하면 로그인시에 프로세스의 실행여부를 확인할 수 있습니다. 만약 프로세스가 존재하지 않는다면, 새로운 프로세스를 실행합니다. 

  ```bash
  #!/usr/bin/env bash

  SSH_AGENT_ENV="$HOME/.ssh-agent.env

  function start_agent { 
      echo "Initialize ssh-agent..."
      /usr/bin/ssh-agent | sed 's/^echo/#echo/' > ${SSH_ENV} 
      chmod 600 ${SSH_AGENT_ENV} 
      . ${SSH_ENV} > /dev/null 
  } 

  if [ -f ${SSH_AGENT_ENV} ]; then
    . ${SSH_AGENT_ENV} > /dev/null 
    ps -ef | grep ${SSH_AGENT_PID} | grep ssh-agent$ > /dev/null || { start_agent; }
  else 
    start_agent 
  fi
  ```

### SSH 키 등록하기

`ssh-add` 명령으로 SSH 키를 등록할 수 있습니다. 이 명령과 함께 등록할 키의 경로를 입력합니다. 키에 암호가 없다면 엔터를 입력합니다.

```bash
$ ssh-add ~/.ssh/id_rsa 
Enter passphrase for ~/.ssh/id_rsa: [Enter] 
Identity added: ~/.ssh/id_rsa (username@server)
```

### SSH 키 조회하기

`-l` 옵션을 사용하면 등록한 키의 목록을 조회할 수 있습니다.

```bash
$ ssh-add -l
3072 SHA256:+pzweXC011JEwvTRKwnFfqncDPzNxmSOY0es+hAqlFk user@server (RSA)
```

### SSH 키 제거하기

`-D` 옵션을 사용하여 등록한 키를 제거합니다. 이 명령은 등록한 모든 키를 제거합니다.

```bash
$ ssh-add -D
All identities removed.
```

## 참고

- PowerShell [Get-Service](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.management/get-service?view=powershell-7.4), [Set-Service](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.management/set-service?view=powershell-7.4), [Start-Service](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.management/start-service?view=powershell-7.4)
