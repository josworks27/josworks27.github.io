---
layout: post
title: "WEB3 Express"
tags: 
    - 생활코딩
comments: true
---


# 3. Hello World
* express의 가장 기본적이며 핵심적인 골격

```javascript
const express = require('express')
const app = express()
const port = 3000

// route, routing: 패스에 따라 콜백함수가 실행됨
app.get('/', (req, res) => res.send('/(root) 페이지에 Hello World!'))
app.get('/test', (req, res) => res.send('/test 페이지에 Hello World'))

app.listen(port, () => console.log(`Example app listening on port ${port}!`))
```

# 4. 홈페이지 구현
* commonJS는 각 라우트별로 조건문을 통해 처리해 줘야했지만 express는 필요한 라우트를 설정하여 필요한 코드들이 들어가 있는 형식으로 훨씬 보기 편하고 간단함

```javascript
app.get('/', (request, response) =>
  fs.readdir('./data', function (error, filelist) {
    var title = 'Welcome';
    var description = 'Hello, Node.js';
    var list = template.list(filelist);
    var html = template.HTML(title, list,
      `<h2>${title}</h2>${description}`,
      `<a href="/create">create</a>`
    );
    response.send(html);
  })
);
```

# 5.1 상세 페이지 구현 1 - Route Parameter
* 최근 트렌드는 쿼리스트링을 성능이나 형태적으로 좋지 않다고 생각함
* 그래서 ':' 라고 하는 와일드 카드를 이용함
* : 다음에 오는 값을 다음과 같이 추출할 수 있음

```javascript
app.get('/page/:pageId', (request, response) => {
  response.send(request.params);
});
// {"pageId":"234234"}
```

# 5.2 상세 페이지 구현 2
* 와일드 카드 방식 변환했을 때의 처리

```javascript
app.get('/page/:pageId', (request, response) => {
  fs.readdir('./data', function (error, filelist) {
    var filteredId = path.parse(request.params.pageId).base;
    fs.readFile(`data/${filteredId}`, 'utf8', function (err, description) {
      var title = request.params.pageId;
      var sanitizedTitle = sanitizeHtml(title);
      var sanitizedDescription = sanitizeHtml(description, {
        allowedTags: ['h1']
      });
      var list = template.list(filelist);
      var html = template.HTML(sanitizedTitle, list,
        `<h2>${sanitizedTitle}</h2>${sanitizedDescription}`,
        ` <a href="/create">create</a>
                <a href="/update?id=${sanitizedTitle}">update</a>
                <form action="delete_process" method="post">
                  <input type="hidden" name="id" value="${sanitizedTitle}">
                  <input type="submit" value="delete">
                </form>`
      );
      response.send(html);
    });
  });
});
```

# 6. 페이지 생성 구현
* get과 post의 구분으로 처리
* 생성하기 위한 폼을 get으로 불러옴

```javascript
// GET 처리
app.get('/create', (request, response) => {
  fs.readdir('./data', function (error, filelist) {
    var title = 'WEB - create';
    var list = template.list(filelist);
    var html = template.HTML(title, list, `
          <form action="/create_process" method="post">
            <p><input type="text" name="title" placeholder="title"></p>
            <p>
              <textarea name="description" placeholder="description"></textarea>
            </p>
            <p>
              <input type="submit">
            </p>
          </form>
        `, '');
    response.send(html);
  });
});
```

* get으로 얻은 form을 post로 요청하기 때문에 그에 따라 서버에서 fs.writeFile 메서드로 원래 파일 내용 수정 처리

```javascript
// POST 처리
app.post('/create_process', (request, response) => {
  var body = '';
  request.on('data', function (data) {
    body = body + data;
  });
  request.on('end', function () {
    var post = qs.parse(body);
    var title = post.title;
    var description = post.description;
    fs.writeFile(`data/${title}`, description, 'utf8', function (err) {
      response.writeHead(302, { Location: `/?id=${title}` });
      response.end();
    })
  });
});
```


# 7. 페이지 수정 구현
* get과 post의 구분으로 처리
* create와 유사 단지 수정한다는 것만 다름

```javascript
// GET 처리
app.get('/update/:pageId', (request, response) => {
  fs.readdir('./data', function (error, filelist) {
    var filteredId = path.parse(request.params.pageId).base;
    fs.readFile(`data/${filteredId}`, 'utf8', function (err, description) {
      var title = request.params.pageId;
      var list = template.list(filelist);
      var html = template.HTML(title, list,
        `
            <form action="/update_process" method="post">
              <input type="hidden" name="id" value="${title}">
              <p><input type="text" name="title" placeholder="title" value="${title}"></p>
              <p>
                <textarea name="description" placeholder="description">${description}</textarea>
              </p>
              <p>
                <input type="submit">
              </p>
            </form>
            `,
        `<a href="/create">create</a> <a href="/update?id=${title}">update</a>`
      );
      response.send(html);
    });
  });
});
```

```javascript
// POST 처리
app.post('/update_process', (request, response) => {
  var body = '';
  request.on('data', function (data) {
    body = body + data;
  });
  request.on('end', function () {
    var post = qs.parse(body);
    var id = post.id;
    var title = post.title;
    var description = post.description;
    fs.rename(`data/${id}`, `data/${title}`, function (error) {
      fs.writeFile(`data/${title}`, description, 'utf8', function (err) {
        response.writeHead(302, { Location: `/?id=${title}` });
        response.end();
      })
    });
  });
});
```

# 8. 페이지의 삭제 구현
* post 요청을 통해 서버에서 fs.unlink()로 파일을 제거함
* response.redirect('/') 를 통해 root로 보내기

```javascript
app.post('/delete_process', (request, response) => {
  var body = '';
  request.on('data', function (data) {
    body = body + data;
  });
  request.on('end', function () {
    var post = qs.parse(body);
    var id = post.id;
    var filteredId = path.parse(id).base;
    fs.unlink(`data/${filteredId}`, function (error) {
      response.redirect('/');
    })
  });
});
```

# 9.1 미들웨어의 사용 - body parser
* 다른 사람들이 만든 모듈들을 부품으로 해서 내 소프트웨어를 만들어 가기 위해 미들웨어 사용
* create 파트에서 post로 형식을 받는데, 기존에는 request.on을 이용하였지만 복잡하였음
* request.on의 과정을 body parser를 이용하면 훨씬 보기 쉽고 간단하게 처리해줌
* 클라이언트로 부터 온 post 데이터를 분석해서 필요한 형태로 가공해 줌
* $ npm install body-parser --save로 설치하지만 최근 node는 기본 내장임

```javascript
// body parser 사용법, 미들웨어 사용
app.use(bodyParser.urlencoded( { extended: false} ))
```

```javascript
app.post('/create_process', (request, response) => {
  var post = request.body;
  var title = post.title;
  var description = post.description;
  fs.writeFile(`data/${title}`, description, 'utf8', function (err) {
    response.writeHead(302, { Location: `/?id=${title}` });
    response.end();
  })
  
  // var body = '';
  // request.on('data', function (data) {
  //   body = body + data;
  // });
  // request.on('end', function () {
  //   var post = qs.parse(body);
  //   var title = post.title;
  //   var description = post.description;
  //   fs.writeFile(`data/${title}`, description, 'utf8', function (err) {
  //     response.writeHead(302, { Location: `/?id=${title}` });
  //     response.end();
  //   })
  // });
});
```

# 9.2 미들웨어의 사용 - compression
* 데이터를 압축시켜서 압축된 데이터를 전송함. 획기적으로 효율적인 전송이 가능함
* 압축을 푸는 작업이 필요함
* 콘솔의 네트워크 확인해 보면 express의 사이즈가 눈에 띄게 줄어든 것을 알 수 있음!

```javascript
// 설치방법
$ npm install compression --save

// import 하고 미들웨어 장착
var compression = require('compression');

app.use(compression());
```

# 10. 미들웨어 만들기
* express에서 미들웨어는 함수의 형태를 갖고 있음, next()를 넣어야 다음 미들웨어로 넘어감
* app.use()로 사용
* app.user는 request, response, next의 순의 파라미터로 구성
* 미들웨어를 만들면 모든 라우터에서 사용 가능함

```javascript
// 미들웨어 만들기 샘플
var myLogger = function(req, res, next) {
  console.log('logger!');
  next();
}
```

* app.use()를 사용하면 필요하지 않은 모든 곳에서도 읽어올 수 있도록 처리가 되기 때문에 불필요한 자원소모가 일어남!
* 그래서 app.get('*', {...}) 으로 바꾸면 get방식에서만 읽어올수 있도록 처리함! 그리고 * 는 모든 경로에서 사용가능하게 됨

```javascript
app.get('*', function (request, response, next) {
  fs.readdir('./data', function (error, filelist) {
    request.list = filelist;
    next();
  })
})
```

# 11. 미들웨어의 실행순서
* next()는 다음 미들웨어를 의미하며, 다음 미들웨어의 동작을 현재 미들웨어가 결정하는 것임
* next()에 인자를 넣는 방식으로 미들웨어의 흐름을 제어할 수 있음
* 조건문 처리로 next('route')를 하면 조건이 성립하면 다음 미들웨어를 실행함

```javascript
app.get('/user/:id', (req, res,next) => {
  if (req.params.id === '0') next('route')
  else next()
}, (req, res, next) => {
  res.render('regular')
})

// 위 미들웨어에서 조건문이 성립하면 아래의 미들웨어가 실행됨
app.get('/user/:id', (req, res, next) => {
  res.render('special')
})
```

# 12. 정적인 파일의 서비스
* 정적인 파일의 서비스: 이미지, 자바스크립트, css같은 파일들을 웹브라우져로 다운로드하는 경우
* 이미지를 넣을려면 정적인 파일을 처리할 수 있도록 설정해야함
* 정적인 파일을 서비스할려고 하는 디렉토리를 지정해주면 됨

```javascript
app.use(express.static('public'));
```

# 13. 에러처리
* 에러도 마찬가지로 미들웨어로 처리함

```javascript
// 에러처리: 최후까지 와도 처리가 안된경우 얘가 404에러로 처리함
app.use(function(erq, res, next) {
  res.status(404).send('Sorry cnat find that!');
})

// 에러 핸들러 미들웨어: 얘로 에러 관련 처리를 할 수 있음
app.use((err, req, res, next) => {
  console.log(err.stack);
  res.status(500).send('Something BROKE!')
}) 
```

# 14.1. 라우터 - 주소체계 변경
* 소프트웨어가 커지면 복잡도가 증가하기 때문에 라우터로 따로 잘 정리정돈 하는 것이 필요함

# 14.2 라우터 - 파일로 분리
* 파일로 분리한 /topic으로 시작하는 라우트들에게 topicRouter를 적용
* main.js에 있던 /topic라우터들을 분리한 곳에서 이전

```javascript
// 필요한 모듈 갖고오기
var express = require('express');
var router = express.Router();
var template = require('../lib/template');
var fs = require('fs');
var path = require('path');
var sanitizeHtml = require('sanitize-html');

// app => router로 바꾸고 패스에서 /topic 제거
router.get('/create', (request, response) => {
    var title = 'WEB - create';
    var list = template.list(request.list);
    var html = template.HTML(title, list, `
            <form action="/topic/create_process" method="post">
              <p><input type="text" name="title" placeholder="title"></p>
              <p>
                <textarea name="description" placeholder="description"></textarea>
              </p>
              <p>
                <input type="submit">
              </p>
            </form>
          `, '');
    response.send(html);
});

// router를 밖으로 내보내기
module.exports = router;
```

# 14.3 라우터 - 파일로 분리 - index
* '/' 루트도 분리하기, 위와 같음

# 15. 보안
* 항상 최신버젼 유지
* helmet, nsp 등 모듈 사용
* express 공식문서 보안 관련 참고

# 16. express generator
* 기본적인 똑같은 일을 반복하는 것을 방지
* 공식문서 express generator 참고
* 기본적인 express의 구성을 자동으로 준비해줌