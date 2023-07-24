---
title: 하나의 PC에서 다수의 GitHub 저장소를 사용하는 방법
description: 개발을 하다 보면 업무 계정과 개인 계정을 분리해서 저장하고 싶은 경우가 있다. 이와 같이 GitHub와 같은 저장소를 N개의 계정으로 쓰는 방법을 설명한다.
author: Kukjin Kang
date: 2023-07-07
tags: ["github", "ssh"]
image: 
---

개발을 하다 보면 업무 계정과 개인 계정을 분리해서 저장하고 싶은 경우가
있다. 이와 같이 GitHub와 같은 저장소를 N개의 계정으로 쓰는 방법을
설명한다.

여기서는 'user-office', 'user-home' 의 두개의 계정을 GitHub 저장소를
쓰는 것을 가정한다.

## Steps:
- [Step1](#step-1) : 각 계정별로 ssh-key 를 생성한다.
- [Step2](#step-2) : 생성한 암호키를 ssh-agent 에 등록한다.
- [Step3](#step-3) : 생성한 공개키를 GitHub 에 등록한다.
- [Step4](#step-4) : SSH 설정파일에 Host정보를 입력한다.
- [Setp5](#step-5) : 각각의 계정으로 GitHub 저장소를 clone 한다.

## Step 1

### 각 계정별로 SSH key를 생성한다

우선 SSH 키를 생성할 디렉토리로 이동한다.

```bash
$ cd ~/.ssh
```

그리고 'ssh-keygen' 명령을 통해 키를 생성한다.

```bash
$ ssh-keygen -t rsa -C "your_email@example.com" -f "github-username"
```

- -t 옵션은 키의 암호화 방법을 지정한다.
- -C 옵션은 SSH 키에 대한 설명을 지정한다.
- -f 옵션은 생성할 키의 파일명을 지정한다.

## Step 2

### 생성한 암호키를 ssh-agent 에 등록한다

각 계정별로 ssh-agent 에 키를 등록한다

```bash
$ ssh-add ~/.ssh/user-office
$ ssh-add ~/.ssh/user-home
```

ssh-agent의 설명은 [여기](/2023-07-03-useful-ssh-agent)를 참고한다.

## Step 3

### 생성할 공개키를 GitHub 에 등록한다

- [GitHub 키 등록](https://github.com/settings/keys) 페이지를 연다.
- 'New SSH key' 버튼을 클릭한다.
- 공개키의 열어 텍스트를 복사한 다음 입력한다.
- 'Add SSH key' 버튼을 클릭한다.


## Step 4

### SSH 설정파일에 Host 정보를 입력한다

설정파일의 경로는 '~/.ssh/config' 이며, 다음과 같이 입력한다.

```
Host github.com-office
Hostname github.com
IdentityFile ~/.ssh/user-office

Host github.com-home
Hostname github.com
IdentityFile ~/.ssh/user-home

```

- Host: Hostname, Identity 속성을 가지는 별도의 이름
- Hostname: 실제 접속할 서버의 주소
- IdentityFile 접속에 사용할 암호키

이제 접속이 잘 되는지를 확인하자

```bash
$ ssh -T git@github.com-office
Hi user-office! You've successfully authenticated, but GitHub does not provide shell access.
$ ssh -T git@github.com-home
Hi user-work! You've successfully authenticated, but GitHub does not provide shell access.
```

설정에서 지정한 주소와 키를 가지고 접속을 시도하는 경우 ID를 다르게
출력하는 것을 볼 수 있다.


## Step 5

### 각각의 계정으로 GitHub 저장소를 clone 한다

```bash
$ git clone git@github.com-office:user-office/a.git
$ git clone git@github.com-office:user-home/a.git
```

## 마지막으로

git 에서 사용하는 계정은 보통 global (~/.gitconfig) 에
저장한다. 여기서는 각 저장소마다 계정이 다르기 때문에 각 저장소마다
계정을 등록해야한다.

```bash
$ git config user.name "user-office"
$ git config user.email "user-office@example.com"
```

저장한 정보는 '.git/config'에서 확인할 수 있다.


이제 명령을 실행해 보자.

```bash
$ git pull
$ git push
```
