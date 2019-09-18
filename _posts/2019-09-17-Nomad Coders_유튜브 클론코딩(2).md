---
layout: post
title: "[유튜브 클론코딩] #2 ExpressJS"
tags: 
    - Nomad coders
    - 유튜브 클론 코딩
    - Javascript
    - ExpreesJS
comments: true
---

## #2.0 What is Server
* 서버란, 인터넷으로 연결되어 요청에 응답하는 컴퓨터
  1. 인터넷(네트워크)으로 연결된 한 덩어리의 코드


## #2.1 What is Express
* ExpressJS는 NodeJS에서 작동하는 서버를 만들기 위한 프레임워크
 1. 이미 만들어져 있기 때문에 사용법을 배워서 편리하게 이용가능 함
 2. 매우 안정적임


## #2.2 Installing Express with NPM
* NPM(Node Package Manager)은 Node.js와 관련된 모든 것들의 중심적 역할을 함
* 각자의 Package에 관리하며 활용 가능
* 이용방법(terminal)  * Node.js를 받으면 바로 이용가능
  1. npm -v : 버전 확인
  2. npm init
    * package name은 나의 웹사이트(wetube)
  3. package.json 생성됨
* npm 설치 방법
  1. 콘솔에 npm install express
    * npm은 package.json이 있는 곳에 만들어야 함
  2. 설치하면 node_modules가 다운로드 됨
* package.json은 매우 유용
  1. 프로젝트를 협업하고 싶다면 단지 package.json을 공유하고 npm install을 콘솔에 입력하면 끝


## #2.3 Your First Express Server
* 서버구축 방법
  1. github에 wetube repository 생성
  2. node_module을 ignore 하기
    * 내가 만든 것만 관리하기 위해
  3. wetube에 .gitignore 생성하고 gitignore의 표준(?) 복사하여 입력 [링크](https://github.com/github/gitignore/blob/master/Node.gitignore)
  4. FuseBox cache .fusebox/ 밑에 package-lock.json 추가
  5. README.md 생성
  6. repository 주소 복사 후 클론하기
    * git remote add origin https://github.com/josworks27/wetube
  7. git add, commit, push 하기
  8. github 확인 및 완료
* Express.js 웹사이트에서 기본 라우팅 복사 후 설치 [링크](https://expressjs.com/ko/)
* 설치방법은 내용이 길어 강의 영상 참고하기

### index.js
```javascript
const express = require('express');
const app = express();

const PORT = 4000;

function handleListening() {
    console.log(`Listening on: http:localhost:${PORT}`);
}

app.listen(PORT, handleListening);
```

### package.json
```json
{
  "name": "2.-wetuve",
  "version": "1.0.0",
  "description": "Cloning Youtube with VanillaJS and NodeJS",
  "main": "index.js",
  "author": "Jo Seong-Cheol",
  "dependencies": {
    "express": "^4.17.1"
  },
  "scripts": {
    "start": "node index.js"
  }
}
```

## #2.4 Handling Routes with Express
* GET/POST method를 통한 브라우저(HTTP)의 작동방식
  1. GET : GET Method를 통해 페이지를 읽음
  2. POST : POST Method를 통해 정보를 전달함
    * ex) 영상에 코멘트를 작성
* Express.js에서 함수는 2가지를 호출함
  1. request object
    * 정보를 얻거나 누군가 페이지를 요청하거나 어떤 종류의 데이터가 페이지로 전송됐는가 등
  2. response object
* 서버, 라우트를 생성하고 작성한 함수의 res.send를 통해 html/css 등을 보내서 응답하는 형식


## #2.5 ES6 on NodeJS using Babel
* **Babel : 최신의 JS코드를 보통의 JS코드로 바꿔주는 컴파일러**
* Babel에는 0-3의 stage가 있고 stage에 따라 preset의 차이가 있음
  1. stage-0이 가장 실험적임
* env는 최신이지만 너무 실험적이진 않음  * 이거 사용
* 설치방법
  1. Babel node 설치 : npm install @babel/node
  2. Babe preset-env 설치 : npm install @babel/preset-env 

**@babel/preset-env 설명**
> @babel/preset-env is a smart preset that allows you to use the latest JavaScript without needing to micromanage which syntax transforms (and optionally, browser polyfills) are needed by your target environment(s). This both makes your life easier and JavaScript bundles smaller!

* .babelrc을 생성하고 Node.js에 관한 원하는 설정을 세팅
### .babelrc
```
{
    "presets": ["@babel/preset-env"]
}
```
* Arrow function 활용하기 [MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Functions/%EC%95%A0%EB%A1%9C%EC%9A%B0_%ED%8E%91%EC%85%98)
* npm install @babel/core
* npm install nodemon -D

**nodemon 이란?**
> nodemon is a tool that helps develop node.js based applications by automatically restarting the node application when file changes in the directory are detected.

