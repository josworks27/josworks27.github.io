---
layout: post
title: "Deployment & Cloud Computing 개요"
tags: 
    - Immersive 16
comments: true
---


# 스프린트 개요
* 목표
    1. 배포의 의미를 알고 남에게 배포
    2. aws 서비스 블럭에서 최소 3개 이상의 서비스를 활용하고 설명(S3, EC2, RDS)
    3. ssh 접속의 의미를 알고 있다.
    4. 보안을 위해 github에 올리지 말아야 할 코드를 gitignore로 따로 분류

* 우분투에서 개발환경 다시 구축해야 함 ?!?!!
* 할당 받고 사용안하면 과금;
* 스프린트가 끝나면 aws 서비스 종료하기

# Intro Deployment
* 다양한 환경의 다른 컴퓨터에서 코드를 돌리는게 배포

**환경 체크 사항**
* node version
* dependencies : pakage.json 체크 중요!!
* port number
* hostname
* URLs and File Paths
* API keys

# Intro Deploy Strategy
**deploy strategy**
1. SPA serve strategy(클라이언트 배포)
2. Server Application deploy strategy(서버 배포)

**SPA serve strategy**
* 사용자들이 모든 파일을 다 받을 필요는 없다!
* build에 스태틱하게 하나로 합쳐서 제공
* build 파일 서브를 위한 클라우드를 이용!, Amazon S3
* 모바일 앱과 유사한 방식으로 다운로드(앱스토어 같이)
* 다운 받은 사용자는 어플을 이용하여 서버와 통신

**Server Application deploy strategy**
* 서버 개발을 혼자 할때는 외부접속을 고려하지 않았음
* 외부인이 내 컴퓨터에 막 들어오게 해선 안된다.
* 그래서 컴퓨터를 하나 임대해 주는 서비스가 Amazon EC2
* 사용자는 EC2에 있는 node 앱에 접속해서 api를 받아간다

EC2란?
* AWS가 가진 컴퓨터. 유저가 원격 접속할 수 있다.

RDS란?
* 데이터베이스 제공하는 서비스

결국 내 컴퓨터를 이용해서 클라이언트, 서버, 디비를 제공하는게 아니라 아마존에서 제공하는 서비스를 이용해서 쓰는것이다!!!!!

# Deploy Sprint Architecture

Bare minimum
1. S3: 리액트 클라이언트를 유저가 받아간다.
2. EC2: 받아간 유저는 EC2에서 구동되는 NODE 서버에 접속하여 api 받아간다.
3. RDS: EC2의 서버에서 필요한 데이터는 RDS에 쿼리를 통해 데이터를 받아간다.

Advanced
자료 참고

Nightmare
자료 참고

