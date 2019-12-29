---
layout: post
title: "WEB1 HTML & Internet"
tags: 
    - 생활코딩
comments: true
---


# 6. 기본문법 태그
* 볼드: <strong></strong>
* 밑줄: <u></u>
다양한 태그들의 이름을 익숙한 단어로 이름지었기 때문에 자연스럽게 이해하기

# 7. 혁명적인 변화

# 8. 통계에 기반한 학습
* 평균적으로 25개의 태그가 사용됨
* 많이 사용되는 것을 잘 알아두는 것이 중요함

# 9. 줄바꿈: br vs p
* <br> 태그는 줄바꿈을 해줌, 닫을 필요 없음
* <p> 태그는 단락을 표현할 때 사용함
* br은 단순히 줄 바꿈. p는 단락이라는 의미를 주기 때문에 정보적으로 가치가 있음

# 10. HTML이 중요한 이유
* 접근성이 중요함. 태그를 이용한 웹 구성이 검색엔진의 상위에 노됨

# 11. 최후의 문법 속성 & img
* <img> 태그만으로는 정보가 부족하여 아무런 일이 일어나지 않음 => 속성(Attribute) 필요
* <img src=""> src=""가 속성 부분이며 이 곳에 url을 넣으면 이미지 표현가능

# 12. 부모자식과 목록
* <ul> <ol>과 <li>의 사용관계, 부모와 자식관계

# 13. 문서의 구조와 슈퍼스타들
* <title> : 웹페이지의 제목을 명시
* <meta charset="utf-8"> : utf-8 형식으로 한다는 의미
* <HTML> 최상위 태그
* <head>, <body>등이 차상위로 구분

이와 같은 태그들은 해당 웹페이지들을 정의하는 태그들

# 14. HTML태그의 제왕
* <a> Link 태그는 HTML상 가장 중요한 태그임
* <a href="링크할 주소">test</a> 의 형식으로 사용함

# 15. 웹사이트 완성
* <a href="경로의 파일">링크를 표현할 문장</a> 으로 구성 가능

```HTML
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <title>test</title>
</head>
<body>
<ul>
<li><a href="1.html">HTML</a></li>
<li><a href="2.html">CSS</a></li>
<li><a href="3.html">Javascript</a></li>
</ul>
</body>
</html>
```

# 16. 원시웹
* 인터넷과 웹은 같은가? 다르다. 인터넷이라는 거대한 틀 안에 웹이라는 작은 부분이 존재
* 최초의 웹 inf.cern.ch

# 17. 인터넷으로 여는 열쇠: 서버와 클라이언트
* 클라이언트의 요청(request)과 서버의 응답(response)으로 작동
* 클라이언트는 웹 브라우저를, 서버는 웹 서버를 이용

# 18. 웹호스팅: github page
* 깃헙은 무료 웹 호스팅 서비스를 제공함
* 세팅에서 브랜치 마스터로 바꿔주면 호스팅 해줌