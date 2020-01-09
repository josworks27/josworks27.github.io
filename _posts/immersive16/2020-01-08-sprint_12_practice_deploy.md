---
layout: post
title: "Deploy 방법 정리"
tags: 
    - Immersive 16
comments: true
---


# Sprint: S3

**React Build**
* npx create-react-app으로 간단히 리액트 앱을 만들고 로컬호스트가 아닌 S3를 이용하여 퍼블릭 링크로 배포하기
* yarn build 하면 스태틱 폴더를 만든다.

**Create Bucket**
* AWS Management Console에서 s3 서비스로 접속한 뒤 버킷만들기
* 만든 버킷의 속성에서 정적 웹사이트 호스팅 설정

**Public Access**
* 권한에서 퍼블릭 엑세스 차단 모든 비활성화
* 버킷 정책은 저장 (영상 참고)
* 리액트 build 파일들을 개요에 업로드
* 엔드포인트에 접속하면 리액트 화면 보인다!


# Sprint: EC2

**Create Server Application**
* express로 간단한 서버 만들기

```javascript
// index.js
const express = require('express');

const app = express();

app.use('/', (req, res) => {
    res.send('hello practice node server deploy')
})

app.listen(5000, () => {
    console.log('server on 5000')
});
```

* localhost:5000에 접속하면 hello practice node server deploy 문구가 보일 것이다.
* 하지만 로컬호스트를 이용하는 것이 아닌 퍼블릭 링크로 만들어서 배포해야 한다.
* github 원격 저장소를 이용하여 EC2에 업로드 해야함

**Start Instance**
* 빌릴 컴퓨터를 셋업하기
* 다양한 OS중에 우분투 18.04(프리티어)로 진행
* 프리티어 아니면 과금이 생긴다 ㅠㅠ
* 접속할 수 있는 키페어 생성(키페어 보관 중요!!)
* ssh 설정 후 우분투 접속!

**Environment Settings**
* 우분투에 node.js와 npm 설치
https://www.digitalocean.com/community/tutorials/how-to-install-node-js-on-ubuntu-16-04

* 우분투에 github에서 서버 clone 하고 pakage.json 이용하여 노드 모듈 설치!
* 서버 실행 시키고 접속해보면 접속 가능!

**Intro PM2**
* PM2: 터미널 종료해도 서버가 계속 켜져 있을 수 있도록 하는 process manager tool
* 글로벌 설치시 제대로 설치가 안되는 경우가 있기 때문에 sudo를 이용하여 pm2 설치
* pm2 start 로 서버 실행후 터미널 종료해도 서버는 계속 돌아감!(wow.....)
```
sudo npm install pm2 -g

// 설치 후 index.js 실행
pm2 start index.js
```

# Sprint: RDS

**Create RDS Instance**
* 상대방의 mysql과 나의 mysql의 상황이 너무 달라서 데이터베이스의 접속부터 생기는 문제가 많음!!
* 따라서 클라우드 데이터베이스 서비스가 장점이 있음!
* 영상참고하여 초기 데이터베이스 세팅
* RDS 인스턴스 생성

**Connect RDS Database**
* RDS 인스턴스 생성 후 mysql 데이터베이스에 접근 가능한지 확인한다!
* CLI에선 아래와 같은 명령어로 가능하다.(GUI 클라이언트를 이용하는게 훨씬 편하다!)

```
mysql.server start

mysql -u '유저명' --host '호스트명' -P '포트번호' -p
```