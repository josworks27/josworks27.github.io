---
layout: post
title: "[유튜브 클론코딩] #1 NodeJS Theory"
tags: 
    - Nomad coders
    - 유튜브 클론 코딩
    - Javascript
    - NodeJS
comments: true
---

## #1.0 What is NodeJS
* JS를 브라우저가 아닌 유저의 컴퓨터에서 작동할 수 있게 만들어진 것 (브라우저 밖의 JS *브라우저 종속에서 벗어나 가능성 확대)
* file system을 다룰 수 있음
  1. 서버 만들기
  2. Web scrapper를 만들어서 정보수집
  3. 모바일 앱 만들기 등


## #1. 1 Use Cases for NodeJS
* NodeJs VS django, laravel의 차이점
  1. NodeJS는 모든걸 유저가 커스터마이징 해야 하고, django, laravel은 기본적인 틀이 갖추어져 있음
  2. NodeJS는 많은 데이터를 다룰 때 장점이 있음 (DB생성/삭제, 유저에게 전송/알림, 실시간 처리 등)
  3. 바이트 단위의 어려운 작업 or 비디오 인코딩/디코딩 등은 NodeJS에 적합하지 않음
* 데이터를 저장/생성/삭제, 실시간 보여주기 등은 NodeJS의 장점
  1. 예를 들어 채팅기능 등의 실시간 처리기능은 Python에 적합하지 않은데, 이는 비동기 언어가 아니기 때문 *실시간 처리를 할 element가 없음
  2. Uber, Paypal, Netflix가 대표적인 사례


## #1. 2 Who Uses NodeJS
* Uber, Paypal, Netflix 등은 데이터를 다루기 때문에 NodeJS로 만들어 짐
* 웹사이트를 만들 때, 꼭 한가지 언어로만 백엔드를 만들 필요 없음


## #1. 3 Installing NodeJS
* MacOS는 brew로 설치
  1. 설치도 가능하지만 문제가 있을 때 고칠 수 있음
  2. 설치/이용/삭제 시에 Path의 문제
* 설치 관련 참고자료 : [링크](https://www.dyclassroom.com/howto-mac/how-to-install-nodejs-and-npm-on-mac-using-homebrew)