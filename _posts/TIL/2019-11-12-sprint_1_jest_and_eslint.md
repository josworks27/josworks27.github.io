---
layout: post
title: "TIL - Jest And ESLint"
tags: 
    - TIL
comments: true
---


## 1. Code Quality
* 1)의도한 대로 동작하는가 2)결함이나 문제가 없는가 3)읽기 쉽고, 유지 및 확장하기 용이한가
* Testing Tool
    1. End to end test
    2. Integration test
    3. Unit test

## 2. Jest
* Jest는 자바스크립트로 작성한 코드를 테스트 하기 위한 프레임워크임
* 기초적인 사용방법

```javascript
테스트할 코드(sum.js)
function sum(a, b) {
    return a + b;
}

테스트할 케이스(sum.test.js)
const num - require('./sum');

test('add 1 + 2 to equal 3', () => {
    expect(sum(1, 2).toBe(3));
});
```

* matcher function 중 toBe와 toEqual을 기능상 유사하지만 toBe가 더 엄격하게 체크함(like === operation)
* toEqual은 같은 모양, 같은 값을 가졌는지 확인(===과 다름)
* 대표적인 global 함수의 종류

```javascript
beforeEach(fn); // 모든 테스트의 시작 전에 fn 을 실행
 
describe(name, fn); // 테스트 블록 형성
describe.skip(name, fn); // 테스트 블록에 속한 모든 테스트 검사하지 않고 넘어감
 
test(name, fn, timeout); // 테스트 항목 만들기 fn은 나중에 실행됨
it(name, fn, timeout); // test와 동일(alias)
 
test.skip(name, fn); // 테스트 항목 건너뛰기
it.skip(name, fn); // test.skip과 동일
xit(name, fn); // it.skip과 동일
xtest(name, fn); // xit과 동일
```

[Jest](https://jestjs.io/en/)

## 3. ESLint
* ESLint는 코드의 룰을 지키는지 체크해 주는 역할을 함
* 대표적으로 에어비엔비, 네이버 등에서 자신들의 ESLint 룰을 공유하고 있음
* 자신만의 룰을 만들고 싶다면 아래의 공식문서 참조!
[ESLint](https://eslint.org/)