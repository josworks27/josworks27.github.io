---
layout: post
title: "TIL - JavaScript의 ProtoType 이란?"
tags: 
    - TIL
comments: true
---


## 3. JavaScript에서 Prototype은 무엇이고 왜 사용해야 하는지?
오승환님의 [Javascript] 프로토타입 이해하기를 바탕으로 재구성하였습니다.
[Link](https://medium.com/@bluesh55/javascript-prototype-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-f8e67c286b67)

* 자바스크립트는 프로토타입 기반의 언어라고 불림

### 3.1. Prototype과 Class
* 자바스크립트는 객체지향언어이지만 객체지향언어에서 중요한 클래스(Class)라는 개념이 없음
* 대신 프로토타입(Prototype)이라는 것이 존재하는데, 이를 이용하여 클래스가 하는 역할인 상속을 흉내내도록 구현해 사용함
* 자바스크립트에서는 함수와 new를 활용해 클래스를 비슷하게 흉내냄

```javascript
function Person() {
  this.eyes = 2;
  this.nose = 1;
}
var kim  = new Person();
var park = new Person();
console.log(kim.eyes);  // => 2
console.log(kim.nose);  // => 1
console.log(park.eyes); // => 2
console.log(park.nose); // => 1
```

* kim과 park은 eyes와 nose를 공통적으로 가지고 있는데, 메모리에는 eyes와 nose가 두개씩 총 4개 할당됨
* 객체를 100개 만들면 200개의 변수가 메모리에 할당되어 효율적이지 못함
    * 이런 문제를 프로토타입으로 해결할 수 있음

```javascript
function Person() {}
Person.prototype.eyes = 2;
Person.prototype.nose = 1;
var kim  = new Person();
var park = new Person():
console.log(kim.eyes); // => 2
...
```
* Person.prototype이라는 빈 Object가 어딘가에 존재하고, Person 함수로부터 생성된 객체(kim, park)들은 어딘가에 존재하는 Object(Person)에 들어있는 값을 모두 갖다쓸 수 있음

### 3.2. Prototype Link와 Prototype Object
* Prototype Link와 Prototype Object을 통틀어 프로토타입이라고 함

#### Prototype Object
* 함수가 정의될 때 다음과 같은 두 가지 일이 동시에 이루어짐

1. 해당 함수에 Constructor(생성자) 자격 부여
    * Constructor 자격이 부여되면 new 키워드를 통해 객체를 만들 수 있음
    * 함수만 new 키워드를 사용할 수 있는 이유임
2. 해당 함수의 Prototype Object 생성 및 연결
    * 함수를 정의하면 함수만 생성되는 것이 아니라 Prototype Object도 같이 생성됨
    
* Prototype Object는 기본적인 속성으로 constructor와 __proto__를 갖고 있음
* Prototype Object는 일반적인 객체이므로 속성을 마음대로 추가/삭제 할 수 있음
* kim과 park은 Person 함수를 통해 만들어졌으니 Person.prototype 참조할 수 있게 됨

#### Prototype Line
* kim객체는 eyes를 직접 가지고 있지 않기 때문에 eyes 속성을 찾을 때 까지 상위 프로토타입(Person.prototype)을 탐색함
* 최상위인 Object의 Prototype Object까지 도달했는데도 못찾았을 경우에는 undefined를 리턴함
* 이렇게 __proto__속성을 통해 상위 프로토타입과 연결되어있는 형태를 프로토타입 체인(Chain)이라고 함
