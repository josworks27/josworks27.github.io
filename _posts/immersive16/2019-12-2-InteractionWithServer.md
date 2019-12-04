---
layout: post
title: "[Interaction With Server]"
tags: 
    - Immersive 16
comments: true
---

# Browser
* 컴퓨터는 2진수만 이해할 수 있음
* 그러면 어떻게 우리가 작성한 코드를 컴퓨터는 이해할 수 있는가 그것은 브라우저 덕분이다. 

웹브라우저란
* HTML로 작성된 웹페이지를 볼 수 있도록 해주는 응용 소프트웨어다.(크롬, IE 등)
* 브라우저 사용자가 선택한 자원을 서버에게 요청하여 브라우저에 나타내는 것
* HTML문서, PDF 등의 자원이 표시 될 수 있다.

작동과정
1. 사용자가 웹브라우저를 통해 웹서버에게 Request(요청)을 한다.
2. 

브라우저는 어떻게 동작하는가? [상세](https://d2.naver.com/helloworld/59361)




# HTTP
HTTP: 클라이언트와 서버가 통신하기 위해서 HTTP라고 하는 규약을 지켜서 통신을 한다.

TCP/IP의 5계층 중 일부분을 담당하는 통신규약

작동방식
항상 요청과 응답으로 구현되는 방식
예: 메세지 줘 라고 요청하면 메시지를 주는 식으로 응답함. 없으면 없다는 응답을 함. 잘못된 요청이면 오류가 남

구성
헤더와 바디로 구성
헤더:
어디에서 보내는 요청인가(origin)
컨텐츠 타입은 무엇인가(content-type)
어떤 클라이언트를 이용해 보냈는가(user-agent)

바디:
바디는 가지고 있는 메서드도 있고 아닌 메서드도 있음(영상 링크 참조)

속성:
1. stateless
http의 각 요청은 모두 독립적이다. 같은 요청을 해도 각각의 독립적인 요청이다


2. connectionless
한번의 요청에는 한번의 응답
한번 응답된 요청된 거기서 끝남

메서드:
1. GET: 서버에 자원요청
2. POST: 서버에 자원을 생성
3. PUT: 서버의 자원을 수정
4. DELETE: 서버의 자원을 제거

# Server
자원을 serve하는 주체
서버는 비유하자면 스타벅스다(고객의 요청을 db에서 리소스를 꺼내 처리해주는 역할)
서버는 클라이언트의 요청을 DB에 리소스 요청과 응답에 대한 처리를 실시

![웹아키텍쳐 개념도]()


# API
서버는 클라이언트의 요청을 받아서 리소스를 조합해서 클라이언트에게 응답하는 역할
서버는 클라이언트가 db에 있는 리소스를 잘 사용할 수 있도록 인터페이스를 제공함. 그게 바로 API
서버 자원을 잘 가져다가 쓸 수 있게 만들어 놓은 인터페이스!
스타벅스로 비유하면 메뉴판의 역할임

# Ajax
old way: 로그인을 하면 페이지 전체를 다시 렌더링함
현재 : 필요한 부분만 렌더링함

dynamic web page의 등장
단순한 web page가 아닌, 보다 애플리케이션 다운 web app의 등장

AJAX란 Asynchronous Javscript and XML
* 서버와 자유롭게 통신할 수 있다
=> XMLHttpRequest(XHR)의 등장
* 페이지 깜빡임 없이 seamless하게 작동한다
=> javascript의 DOM 이용

보다 쓰기 쉬운 표준 API를 만들자는 목적으로 fecth API 등장
fecth와 XMLHttpRequest이 어떤 차이점이 있는지 비교


# why we use 'fecth'?
fecth는 서버에서 자원을 가져오기 위해 이용
이러한 기능은 XMLHttpRequest, jQuery ajax, fetch 등등 다양함

1. XMLHttpRequest
영상링크 참조

2. jQuery ajax
체이닝 방식으로 바뀌었으나 아직 복잡함

3. Fetch API
영상링크 참조
네트워크 통신을 포함한 리소스 취득을 위한 인터페이스가 정의되어 있음
XMLHttpRequest와 같은 비슷한 API가 존재하지만, 새로운 Fetch API는 조금 더 강력하고 유연한 조작이 가능하다는 장점이 있음

Fetch(서버주소)
  .then()
  .then()
  .cacth() 에러처리
  ...
비동기처리

![fetch 개념도]()