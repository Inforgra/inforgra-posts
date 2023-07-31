---
title: 당신이 무었을 다운로드 하고 있는지 알고 있다.
description:
author: Kukjin Kang
date: 2023-08-01
tags: ["토렌트"]
image: https://i.ibb.co/d7yR827/iknowwhatyoudownload.png
---

얼마전에 [iknowwhatyoudownload](https://iknowwhatyoudownload.com/) 사이트를 알게 되어서
IP를 조회해보니, 그 동안 받았던 토렌트 내역이 모두 나왔다. 관련 해서 좀 더 살펴보니 몇
년 전쯤에 크게 이슈가 된 적이 있었던것 같다. 당연히 아래 내역은 다른 분들의 IP를 조회해
본 결과이다.

<img src="https://i.ibb.co/d7yR827/iknowwhatyoudownload.png" width="100%" />

이렇게 IP와 함께 받았던 목록이 노출되면, 다른 누군가가 볼 수도 있다. 법적 효용성은 둘째
치더라도 매우 껄끄러워진다. 이 자료는 어떻게 수집는지, 어떻게 하면 막을 수 있는지 살펴보자.

## iknowwhatyoudownload 의 수집 방법

토렌트 파일을 수집하기 위해 토렌트 사이트 파싱과 DHT 네트웍크 수신이라는 방법을 사용한다고
홈페이지에서 밝히고 있다. 토렌트 사이트 파싱은 별 상관이 없을듯 하고, DHT 네트웍크 수신이
무었인지 한번 알아 볼 필요가 있겠다.

## 분산 해쉬 테이블 (Distributed Hash Table)

내가 이해한 바로는 DHT는 사용자와 파일에 대해 고유한 ID를 부여한 테이블이다. 여러명의
사용자가 모여서 DHT를 구성하면, 사용자들끼리 파일을 주고 받을 수 있게 해준다. 예를
들어 `a.txt` 라는 파일을 이 테이블에서 찾으면 누가 가지고 있는지 바로 알 수 있다.

DHT를 쓰게 되면 탈 중앙화가 가능하고, 좀 더 빠르게 다운로드와 업로드를 가능하게 한다.
단점으로는 누가 어떤 파일을 가지고 있는지 알 수 있다는 것이다.

## 기록을 남기지 않는 방법

기록을 남기지 않기 위해서는 토렌트 클라이언트에서 DHT 관련 옵션을 꺼야 한다. 속도가 좀
느릴 수는 있겠지만, 엄청 큰 차이는 보이지 않을 것이다.


## 삭제가 가능한가?

몇몇 글들을 읽어보니 몇 가지 방법들을 설명하고 있는데, 크게 효용성이 있지 않는 것 같다.
그냥 시간이 지나 삭제되는 것이 기다리는게 가장 좋은 방법인것으로 보인다.


## 참고

- [Distributed hash table](https://en.wikipedia.org/wiki/Distributed_hash_table)
- [DHT 개념](https://ddongwon.tistory.com/76)
