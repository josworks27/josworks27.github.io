---
layout: post
title: "TIL - 기초적인 JavaScript ES6 문법"
tags: 
    - TIL
comments: true
---


# Scope & Closure

## Hoisting
* var 키워드에서 가능
* let, const 키워드에 경우에는 'ReferenceError' 가 발생

```javascript
console.log(a); // undefined
var a = 1;
```

## Default Parameter
* 함수의 Prameter에 기본값을 줄 수 있음

```javascript
function testFunc(a = 'hello') {
    console.log(a);
}

testFunc() // 'hello'
testFunc('world') // 'world'
```

## Template Literal

```javascript
let a = 'hello';
let b = 'world';

let c = `${a} + ${b} is 'hello world'`;
console.log(c); // hello + world is 'hello world'
```

## Arrow Function

```javascript
보통의 함수표현식 예시
let foo = function(a) {
    return a;
}

화살표 함수를 이용한 함수표현식 예시
let foo = a => a;
}
```

## Rest Parameter
* arguments와 같이 유사배열이 아닌 배열형태(* 배열 메소드 활용가능)

```javascript
function foo(a, b, ...params) {
    console.log(a);
    console.log(b);
    console.log(params);
}

foo(1, 2, 3, 4, 5);
// 1
// 2
// [3, 4, 5];
```