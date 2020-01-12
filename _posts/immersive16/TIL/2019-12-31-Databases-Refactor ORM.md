---
layout: post
title: "Databases Refactor ORM"
tags: 
    - Immersive 16
comments: true
---


# ORM
* Object-Relational Mapping
* 오브젝트 <-> ORM <-> Relational Database
* ORM을 이용하여 객체의 형태로 데이터베이스에 객체를 삽입, 수정 등이 가능하다.

# ORM 왜 사용?
* 자바스크립트의 객체와 데이터베이스의 테이블의 형태는 서로 다르기 때문에 ORM을 이용하여 자바스크립트의 객체의 형태로 데이터베이스를 다룰 수 있다.
* 효율적으로 데이터베이스 작업이 가능하다.

# Sequelize
* 프로미스 기반의 node.js ORM이다. => 따라서 then(), catch()가 쓰임

![ORM 예시코드]()
* sequelize 모듈을 통해 디비와 연결 (공식문서 참고!!)

* define 이라는 메소드를 이용하여 디비의 정보를 자바스크립트 처럼 변수를 만든다
* 아이디는 특별히 없으면 자동
* 형식도 지정 가능

* sync() 메서드를 사용하여 위의 만든 변수(테이블)를 사용
* 프로미스 기반이기 때문에 then, catch 사용
* 변수.create(), findAll() 등 메서드 사용


```javascript
/* You'll need to
 *   npm install sequelize
 * before running this example. Documentation is at http://sequelizejs.com/
 */

var Sequelize = require('sequelize');
var db = new Sequelize('chatter', 'root', '');
/* TODO this constructor takes the database name, username, then password.
 * Modify the arguments if you need to */

/* first define the data structure by giving property names and datatypes
 * See http://sequelizejs.com for other datatypes you can use besides STRING. */
var User = db.define('User', {
  username: Sequelize.STRING
});

var Message = db.define('Message', {
  userid: Sequelize.INTEGER,
  text: Sequelize.STRING,
  roomname: Sequelize.STRING
});

/* Sequelize comes with built in support for promises
 * making it easy to chain asynchronous operations together */
User.sync()
  .then(function() {
    // Now instantiate an object and save it:
    return User.create({username: 'Jean Valjean'});
  })
  .then(function() {
    // Retrieve objects from the database:
    return User.findAll({ where: {username: 'Jean Valjean'} });
  })
  .then(function(users) {
    users.forEach(function(user) {
      console.log(user.username + ' exists');
    });
    db.close();
  })
  .catch(function(err) {
    // Handle any error in the chain
    console.error(err);
    db.close();
  });
```

**같이 공부하면 좋은 주제**
* Association: JOIN을 직관적으로 도와주는 Sequelize의 기능
* Transaction: 
    은행업무를 예를 들면, 보내는 사람의 통장에서 받는 사람의 통장으로 돈을 보내야 하는데, 중간에서 에러가 생기면 문제가 생김. 이런 것을 방지하기 위해 Trasaction을 이용하여 에러가 생기면 원래 보내는 사람의 통장으로 돈을 돌려주는 '롤백'의 기능을 할 수 있도록 한다.


