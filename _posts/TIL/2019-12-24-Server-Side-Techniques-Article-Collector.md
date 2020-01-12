---
layout: post
title: "Server Side Techniques_Article Collector"
tags: 
    - Immersive 16
comments: true
---


# 1. Intro Article Collector
* 비동기 처리를 이용하여 웹 앱을 긁어오는 프로그램 만들기

* src에서 서버쪽 작성

* 헬퍼 함수를 작성 2개
  http를 통해 아티클을 가져오는 함수
  파일을 읽고 쓰는 함수: 어딘가에 저장해서 불러와야하기 때문에 필요

* 헬퍼 작성 후 헬퍼를 '이용'해서 각각의 라우트의 엔드포인트 작성
  각각의 요청이 들어왔을떄 작동할 함수





# Advanced
1.  https 뿐만 아니라 http 형태의 URL도 (e.g. Naver) 사용가능하도록 만들어보세요.
   * http, https 프로토콜 확인: https://gunhoflash.tistory.com/56

2. medium blog가 아닌, 다른 형태의 HTML을 응답으로 받더라도, 내용을 수집할 수 있도록 만드세요.
  * const content = dom.window.document.querySelector("article").textContent;
부분을 건들기

```
현재, Article Collector는 Medium 이라는 블로깅 서비스의 내용만을 받아올 수 있습니다. src/server/routes/data.js의 POST 요청 부분을 살펴보면, Medium HTML 구조 중에서, 작성된 글을 담고 있는 특정 selector를 추출해내는 것을 볼 수 있습니다.
```

3. web app에 업로드 하기
https://wayhome25.github.io/nodejs/2017/02/21/nodejs-15-file-upload/