---
layout: post
title: "TIL - 자바스크립트 비동기(Async)에 대해"
tags: 
    - TIL
comments: true
---


# Intro Server Side Techniques Sprint
1. Promises
* 솔로 스프린트
* 파일에 내용을 읽어서 그 내용을 바탕으로 다른 곳에 http요청을 보내고 응답을 다른 파일에 기록하는 스프린트

2. Article Collector
* pair 스프린트
* 원하는 블로그 글을 긁어와서 파일에 저장하는 프로그램을 만드는 스프린트

3. 목표
  1. Promises & async/await
  2. Data Presistence: 데이터를 보존할 수 있도록 파일시스템을 이용
  3. Getting resources from web: 웹페이지를 제공하는 것 이외에 웹에서 리소스를 가져오는 등의 방법


# Intro to JS Async
1. Asynchronous(비동기)
  * 클라이언트에서 요청을 보내면 서버는 처리하여 응답을 함
  * 이 와중에 클라이언트는 멈추는게 아니라 다른 작업을 처리함
  * 효율적으로 처리할 수 있다는 장점

2. Synchronous(동기)
  * 클라이언트가 요청하여 서버가 응답을 처리하는 중에 클라이언트는 응답을 받을 때까지 멈춤

![1](/images/immersive16/12-23-1.png)

* 예를 들어, 유튜브에서 영상을 클릭하면 로딩(요청)하는 동안 다른 것을 클릭하거나 코멘트를 달거나 다른 컴포넌트를 조작할 수 있음. 이런게 비동기적인 처리
* 처리 방식은 Event loop 참고


# Callback
* 비동기의 순서를 제어하고 싶을 때는??
* 콜백은 비동기를 다룰 수 있음
* 콜백 안에서 다음 처리를 다루면 순서를 제어할 수 있음!!

![2](/images/immersive16/12-23-2.png)

* Callback error handlicng Design
* 콜백을 인자로 받으며, 일반적으로 콜백의 앞의 인자가 에러, 뒤가 결과 데이터 순으로 처리함

![3](/images/immersive16/12-23-3.png)

* 콜백은 훌륭하지만 코드가 길어지면 가독성이 굉장히 나빠짐
* 그래서 Promise가 사용됨

![4](/images/immersive16/12-23-4.png)


# Promise / Async & Await

## 1. Promise
* 콜백 지옥을 벗어나기 위해 promise 사용
* 프로미스는 하나의 클래스 같은 것임 new Promise()
  1. resolve(): go to next action
  2. reject(): handle error
* 함수를 처리하고, then -> then 의 순으로 다음 태스크들을 처리할 수 있음
* 그리고 마지막 체인에 .catch로 에러 처리를 하기 때문에 모든 콜백에서 에러 처리를 할 필요가 없음
* 콜백 지옥의 형식이 아니라 평평하여 가독성이 증가함

![5](/images/immersive16/12-23-5.png)

* 프로미스 지옥도 일어날 수 있음 return을 통해서 해당 비동기를 다음으로 넘기면 지옥을 벗어날 있음

![6](/images/immersive16/12-23-6.png)

## 2. Async & Await
* 프로미스인데 보이는 모습이 조금 다른 형태. 최신
* await로 비동기 함수들을 마치 동기인 형태로 사용할 수 있음
* 프로미스 처리방식은 비슷하지만 코드 가독성이 더 좋음

![7](/images/immersive16/12-23-7.png)

* 결론적으로 비동기 처리의 순서를 다루는 것은 동기적인 처리와 다르게 동기적 처리는 동기적 처리대로 진행시키고, 비동기적인 작업이 필요한 것들이지만 순서가 중요한 경우를 위해 사용된다고 생각됨