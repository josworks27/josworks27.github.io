---
layout: post
title: "TIL - Session과 Token을 이용한 인증구현"
tags: 
    - TIL
comments: true
---


# 1. Session 인증 방식

## SignUp

##### 접근방법
req.body로 받은 클라이언트의 요청 값을 이용하여 가입되어 있는 회원인지 파악하고 미가입 회원이라면 데이터베이스에 있는 유저 정보를 저장하도록 하였다.
이를 위해, Sequelize의 Query문인 findOne을 이용하여 데이터베이스에 유저정보가 있는지 확인하였고, 없다면 create를 통해 데이터베이스에 유저정보를 저장하였다.

```javascript
const { users } = require('../../models');

module.exports = {
  post: async (req, res) => {
    // TODO : 유저가 회원가입을 했을 때, 회원정보를 데이터베이스에 저장하도록 구현하세요.
    const body = req.body;

    let findResult = await users.findOne({
      where: {
        email: body.email
      }
    })

    // 메일이 존재하지 않는 경우, 디비저장 처리
    if (findResult === null) {
      let result = await users.create({
        email: body.email,
        username: body.username,
        password: body.password
      });

      // 디비 저장 결과 처리
      res.status(200).send(result.dataValues)
    } else {
      // 메일이 존재하는 경우, 에러 처리
      res.status(409).send('existing user');
    }
  }
};
```

## SignIn

###### 접근방법
req.body를 통해 유저가 보내는 로그인 정보를 파악하고 이 정보를 이용하여 query문인 findOne을 이용하여 데이터베이스에 해당 유저의 정보가 존재하는지 먼저 파악하였다.
그리고 정상적으로 로그인이 된 후에는 해당 유저의 접근권한을 부여하기 위해 세션 스토리지에 해당 유저의 정보를 저장하도록 하였다.

```javascript
const { users } = require('../../models');

module.exports = {
  post: async (req, res) => {
    // TODO : 유저가 로그인을 했을 때, 회원정보를 데이터베이스에서 확인하고, 회원의 id를 session에 담아주도록 구현하세요.

    const body = req.body;

    let findResult = await users.findOne({
      where: {
        email: body.email,
        password: body.password
      }
    });

    if (findResult) {
      req.session.userid = findResult.id;
      req.session.email = findResult.email;

      let resData = { id: findResult.id };
      res.status(200).send(resData);
    } else {
      res.status(404).send('invalid user');
    }
  }
};
```

## SignOut

###### 접근방법
세션 스토리지에 있는 세션 정보를 삭제하고 루트('/')로 redirect 될 수 있도록 하였다.

```javascript
module.exports = {
  post: (req, res) => {
    // TODO : 유저가 로그아웃했을 때, session 정보를 없애고, '/'로 redirect할 수 있도록 구현하세요.

    req.session.destroy();
    res.redirect('/');
  }
};
```

## Info

###### 접근방법
세션에 있는 유저의 정보를 이용하여 데이터베이스에 있는 정보를 찾아 정보를 제공하도록 하였다.

```javascript
const { users } = require('../../models');

module.exports = {
  get: async (req, res) => {
    // TODO : 유저의 session을 이용하여, 데이터베이스에 있는 정보를 제공하도록 구현하세요.

    const session = req.session;

    if (session.userid) {
      let findResult = await users.findOne({
        where: {
          id: session.userid
        }
      });
      res.status(200).send(findResult.dataValues)
    } else {
      res.status(401).send('need user session');
    }
  }
};
```

# 2. Token 인증 방식
위에서 작성한 세션 인증방식을 대신하여 토큰(Token)을 이용하는 방식으로 리팩토링하였으며, 토큰을 이용하기 위해 jsonwebtoken이라는 라이브러리를 사용하였다.

## SingIn

###### 접근방법
로그인하려는 유저의 정보가 데이터베이스에 존재하는 회원인 경우, 토큰을 발급하도록 하였다.

```javascript
const jwt = require('jsonwebtoken');
const secretKey = require('../../jwt');
const { users } = require('../../models');

module.exports = {
  post: async (req, res) => {

    const body = req.body;

    let findResult = await users.findOne({
      where: {
        email: body.email,
        password: body.password
      }
    });

    if (findResult) {
      // 존재하는 유저를 확인하였으므로 토큰 발급
      
      // 토큰 생성
      let token = jwt.sign({
        email: findResult.dataValues.email  // payload
      },
      secretKey.secret, // 비밀키
      {
        expiresIn: '5m' // 토큰 유효시간
      }
      );

      // 쿠키에 토큰을 저장
      res.cookie('user', token);
      let resData = { id: findResult.id };
      res.status(200).send(resData);

    } else {
      res.status(404).send('unvalid user');
    }
  }
};
```

## Info

###### 접근방법
클라이언트로 부터 받은 요청에서 쿠키를 확인하여 토큰의 권한을 확인하였다.

```javascript
const jwt = require('jsonwebtoken');
const secretkey = require('../../jwt');
const { users } = require('../../models');

module.exports = {
  get: async (req, res) => {

    let token = req.cookie.user;

    if (!token) {
      res.status(401).send('need token');
    } else {
      let decoded = jwt.verify(token, secretkey.secret);

      let findResult = await users.findOne({
        where: {
          email: decoded.email
        }
      });

      if (findResult) {
        res.status(200).send(findResult.dataValues);
      } else {
        res.status(400).send('something err');
      }
    }
  }
};
```