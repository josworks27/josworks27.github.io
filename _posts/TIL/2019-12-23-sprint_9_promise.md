---
layout: post
title: "TIL - Promise 란?"
tags: 
    - TIL
comments: true
---


# 1. Intro Promises
* 자바스크립트 대표적 비동기 로직: http의 요청/응답, 파일을 읽고 쓰는 파일 입/출력 등
* 비동기로 처리되는 작업들은 흐름을 예측하기 어려움 => 예측하기 쉬운 흐름으로 만드는게 비동기 처리! (콜백, 프로미스, 에이싱크 어웨잇)
* 스프린트 폴더의 example 간단한 예시들
* 이해하고 번호 순대로 exercise 진행

1. callback:
  * getDataFromFile: 파일의 내용을 읽어와서 결과를 콜백으로 넘겨주는 함수 만들기
  * getBodyFromGetRequest: http 응답의 body를 콜백으로 넘겨주는 함수 만들기
2. promiseConstructor: 콜백과 같지만 프로미스를 이용
3. promisification: util을 이용해서 콜백으로 작성되어 있는 함수들을 프로미스로 변환하기
4. basicChaning: 두 가지 이상의 비동기 작업들을 연속적으로 처리하기
  * 파일에서 읽어 오는 작업
  * 읽어온 결과를 가지고 다시 요청을 하는 작업
5. asyncAwait: basicChanig에서 했던 작업을 async await작업으로 리팩토링

* 유클 참고하기!


# 2. 자바스크립트 비동기 처리와 콜백 함수[캡틴판교]
* 비동기처리: 특정 코드의 연산이 끌날 때까지 코드의 실행을 멈추지 않고 다음 코드를 먼저 실행하는 자바스크립트의 특성을 의미

## 비동기 처리의 첫 번째 사례 - 제이쿼리 ajax
* tableData에 response가 담길 것으로 예상되지만 undefined가 담김. 그 이유는 $.get()으로 데이터를 요청하고 받아올 때까지 기다려주지 않고 다음 코드를 실행하기 때문

```javascript
function getData() {
	var tableData;
	$.get('https://domain.com/products/1', function(response) {
		tableData = response;
	});
	return tableData;
}

console.log(getData()); // undefined
```

## 비동기 처리의 두 번째 사례 - Web API setTimeout()
* setTimeout()도 비동기 방식으로 처리되므로 3초를 기다렸다가 다음 코드를 실행하는 것이 아니라 일단 setTimeout()을 실행하고 나서 다음 코드인 console.log('Hello Again') 으로 넘어감
* 따라서 Hello, Hello Again가 출력되고 3초 뒤에 Bye가 출력됨 

```javascript
// #1
console.log('Hello');
// #2
setTimeout(function() {
	console.log('Bye');
}, 3000);
// #3
console.log('Hello Again');
```


## 콜백 함수로 비동기 처리 방식의 문제점 해결하기
* 위와 같이 undefined 등의 문제를 해결하기 위해 콜백 함수를 이용함
* 위의 ajax 사례를 콜백 함수를 이용하는 코드로 개선하면 아래와 같음

```javascript
function getData(callbackFunc) {
	$.get('https://domain.com/products/1', function(response) {
		callbackFunc(response); // 서버에서 받은 데이터 response를 callbackFunc() 함수에 넘겨줌
	});
}

getData(function(tableData) {
	console.log(tableData); // $.get()의 response 값이 tableData에 전달됨
});
```


# 3. 자바스크립트 Promise 쉽게 이해하기[캡틴판교]
* Promise: 자바스크립트 비동기 처리에 사용되는 객체
* 프로미스는 서버에서 받아온 데이터를 화면에 표시할 때 사용


## 프로미스 코드 - 기초
* 콜백 함수와 프로미스의 비교
* new Promise(), resolve(), then()와 같은 프로미스 API 구조로 변경

```javascript
// 콜백 함수
function getData(callbackFunc) {
  $.get('url 주소/products/1', function (response) {
    callbackFunc(response); // 서버에서 받은 데이터 response를 callbackFunc() 함수에 넘겨줌
  });
}

getData(function (tableData) {
  console.log(tableData); // $.get()의 response 값이 tableData에 전달됨
});
```

```javascript
// 프로미스
function getData(callback) {
  // new Promise() 추가
  return new Promise(function (resolve, reject) {
    $.get('url 주소/products/1', function (response) {
      // 데이터를 받으면 resolve() 호출
      resolve(response);
    });
  });
}

// getData()의 실행이 끝나면 호출되는 then()
getData().then(function (tableData) {
  // resolve()의 결과 값이 여기로 전달됨
  console.log(tableData); // $.get()의 reponse 값이 tableData에 전달됨
});
```

## 프로미스의 3가지 상태(states)
* 상태는 프로미스의 처리 과정을 의미
  1. Pending(대기) : 비동기 처리 로직이 아직 완료되지 않은 상태
  2. Fulfilled(이행) : 비동기 처리가 완료되어 프로미스가 결과 값을 반환해준 상태
  3. Rejected(실패) : 비동기 처리가 실패하거나 오류가 발생한 상태


## 프로미스 코드 - 쉬운 예제
* 서버에서 제대로 응답을 받으면 resolve() 메서드를 호출하고, 응답이 없으면 reject() 메서드를 호출
* 호출된 메서드에 따라 then()이나 catch()로 분기하여 데이터 또는 오류를 출력

```javascript
function getData() {
  return new Promise(function (resolve, reject) {
    $.get('url 주소/products/1', function (response) {
      if (response) {
        resolve(response);
      }
      reject(new Error("Request is failed"));
    });
  });
}

// Fulfilled 또는 Rejected의 결과 값 출력
getData().then(function (data) {
  console.log(data); // response 값 출력
}).catch(function (err) {
  console.error(err); // Error 출력
});
```


## 여러 개의 프로미스 연결하기 (Promise Chaining)
* 프로미스의 또 다른 특징은 여러 개의 프로미스를 연결하여 사용할 수 있다는 점

```javascript
new Promise(function(resolve, reject){
  setTimeout(function() {
    resolve(1);
  }, 2000);
})
.then(function(result) {
  console.log(result); // 1
  return result + 10;
})
.then(function(result) {
  console.log(result); // 11
  return result + 20;
})
.then(function(result) {
  console.log(result); // 31
});
```


## 프로미스의 에러 처리 방법
* then()의 두번째 인자로 에러를 처리하는 방법과 catch()를 쓰는 방법이 존재
* 가급적이면 에러 처리를 위해 catch()를 쓰는 편이 좋음



# 4. Node.js에서 Request.js 사용하기
https://medium.com/harrythegreat/node-js%EC%97%90%EC%84%9C-request-js-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0-28744c52f68d

* 브라우저에서 자바스크립트를 쓴다면 fetch라는 내장 모듈이 있지만 Node.js에서 가장 많이 쓰는 HTTP 라이브러리는 request.js

```javascript
const request = require('request');

// 디폴트 형태
request('http://www.google.com', function (error, response, body) {
    console.error('error:', error); // Print the error if one occurred
    console.log('statusCode:', response && response.statusCode); // Print the response status code if a response was received
    console.log('body:', body); // Print the HTML for the Google homepage.
});

// GET 요청 형태
request.get({ uri: "http://www.google.com" }, function (error, response, body) {
    console.error('error:', error); // Print the error if one occurred
    console.log('statusCode:', response && response.statusCode); // Print the response status code if a response was received
    console.log('body:', body); // Print the HTML for the Google homepage.
});

// GET 파라미터가 있는 형태
const options = {
    uri: "http://www.google.com",
    qs: {
        page: 5
    }
};

request(options, function (error, response, body) {
    console.error('error:', error); // Print the error if one occurred
    console.log('statusCode:', response && response.statusCode); // Print the response status code if a response was received
    console.log('body:', body); // Print the HTML for the Google homepage.
});

// POST 요청

// form 요청
const options = {
    uri: 'http://google.com',
    method: 'POST',
    form: {
        key: 'value',
        key: 'value',
    }
}
request.post(options, function (err, httpResponse, body) { /* ... */ })


// JSON 요청
const options = {
    uri: 'http://google.com',
    method: 'POST',
    body: {
        key: value
    },
    json: true  //json으로 보낼경우 true로 해주어야 header값이 json으로 설정됩니다.
}
```


# 5. fs.readFile()

```javascript
// fs.readFile(path[, options], callback)
fs.readFile('/etc/passwd', (err, data) => {
  if (err) throw err;
  console.log(data);
});

// 옵션 넣는다면?
fs.readFile('/etc/passwd', 'utf8', callback);
```
