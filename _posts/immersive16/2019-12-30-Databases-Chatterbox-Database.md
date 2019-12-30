---
layout: post
title: "Databases_Chatterbox Databases"
tags: 
    - Immersive 16
comments: true
---


# Intro Chatterbox-Database
* 스키마 만들기 => sql 명령문 생각하고 => mysql에서 만들기
* 테스트케이스가 스키마와 구조가 다르다면 테스트케이스를 맞추기


## 2. Familiarize Yourself with the Directories
**schema.sql**
* schema.sql은 데이터베이스를 사용하고 그 안에서 새로운 테이블을 생성하기 위해 고안된 뼈대이다.
* schema.sql 파일에서 하나 이상의 CREATE TABLE 구문을 작성하게 될 것이다.
* CREATE TABLE 구문은 데이터베이스 테이블의 구조를 정의하고 MySQL 서버에서 그것들을 로딩할 것이다.

**app.js**
* app.js는 express를 활용하고 node.js 웹 서버 코드를 위한 시발점이 될 것이다. 
* express는 MVC 프레임 워크이지만 Backbone 에 비해 조금 다른 점이 있다.
* express에서 view는 express 서버의 응답이 되기 위해 고려되며, model과 controller를 위한 코드는 자신의 경로 안에 존재한다.

**server/db/index.js**
* 로컬 컴퓨터에서 작동하는 데이터베이스 서버에 연결하기 위해 mysql npm module을 사용한다.

**server/models/index.js**
* 어플리케이션에서 사용될 messages와 users의 models를 정의한다. 
* models의 뼈대는 이미 만들어져 있지만 각각의 메서드를 세부적으로 작성해야 한다.

**server/controllers/index.js**
* 어플리케이션에서 사용될 messages와 users의 contorllers를 정의한다.
* contollers의 뼈대는 이미 만들어져 있지만 각각의 메서드를 세부적으로 작성해야 한다.

**server/spec/server-spec.js**
* node 서버가 데이터베이스에 읽고 쓰는 능력을 테스트기 위한 스펙이 포함된다.
* 스펙은 아직 완성된건 아니고, 내가 생성한 데이터베이스 테이블의 세부사항에 맞춰 코드를 수정해야 한다.

## 3. Create MySQL Persistent Storage for Chatterbox App
* 채터박스-서버 용의 데이터를 유지하기 위한 멀티-테이블 스키마를 설계하기
    1. 툴을 이용하여 테이블 설계
    2. 설계한 테이블을 정의하기 위해 schema.sql을 편집
        * mysql -u root < path/to/schema.sql로 MySQL 서버에 스키마를 로드
    3. mysql 쌍방형 프롬프트를 열고 테이블이 제대로 만들어 졌는지 확인하기 위해 SHOW TABLES, DESCRIBE <table-name> 명령어을 이용
* mysql을 설치 및 저장하기 위해 npm 이용하기. npm install mysql --save
* server/spec/server-spec.js의 테스트케이스의 스펙을 잘 보고 뭘 하는건지 파악하기
* 영속적인 SQL 채터박스 서버를 만들기 위해 모든 파일들을 사용한다.
* server/app.js는 어플리케이션의 시발점으로 사용되고, server/models/index.js and server/controllers/index.js의 메서드를 만들어야 한다.
* 웹 서버가 MySQL 데이터베이스에 연결되도록하고 데이터베이스 연결을 사용하여 메시지가 들어오면 데이터를 저장하기
* 서버의 영속성을 테스트하기: 메세지를 보내고 서버를 멈춘 다음에 다시 서버를 실행했을때 보냈던 메시지가 있는지 확인
* 서버를 터미널에서 작동시키고, 테스트는 다른 터미널에서 실시