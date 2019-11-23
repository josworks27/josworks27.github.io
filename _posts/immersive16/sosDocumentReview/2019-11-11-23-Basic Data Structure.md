---
layout: post
title: "Data Structure(2)_Basic Data Structure"
tags: 
    - Immersive 16
    - SOS Document Review

comments: true
---

# Stack
스택은 Last In first Out(LIFO) 규치겡 따라 동작하는 동적인 데이터 구조를 의미한다. 비유하자면, 어러개의 접시를 쌇아 놓은 것 같은 형태와 비슷하며 접시가 필요하면 맨 위에 있는 접시는 꺼내는 형태이다.

* 스택에 필요한 프로퍼티
    1. 자료를 저장할 공간: storage
    2. 스택의 크기와 자료가 얼마나 저장되어 있는지 확인: count

* 스택에 필요한 메서드
    1. 자료를 스택에 입력: push
    2. 자료를 스택에서 제거: pop

* 자바스크립트를 이용하여 스택의 구현: Pseudoclassical

```javascript
const Stack = function () {
    this.storage = {};
    this.count = {};
};
// Stack이라는 생성자 함수를 선언
// 자료를 저장하는 프로퍼티: storage (type: Object)
// 자료를 카운트하는 프로퍼티: count (type: Object)

Stack.prototype.push = function(value) {
    this.storage[this.count] = value;
    this.count++;
};
// 매개변수로 받는 value의 값을 this.strogate[this.count]에 저장
// 저장한 횟수만큼 count 증가

Stack.prototype.pop = function () {
    if (count < 0) return;
    let popp = this.storage[this.count - 1];
    delete this.storage[this.count - 1];
    this.count--;
    return popp;
};
// 자료를 제거하는 pop메서드를 prototype에 작성
// 항상 마지막에 넣은 자료가 제거되기 때문에 가장 최신의 인덱스인 count - 1번째를 delete
// 제거한 만큼 count를 줄이고, 제거되는 자료를 리턴 Pseudoclassical Initiation

Stack.prototype.size = function () {
    return this.count;
};
// 스택의 사이즈를 알기 위해 현재 count를 리턴
```


# Queue
큐는 First in First Out(FIFO) 구조로 데이터를 저장하는 방식이다. 스택과 달리 큐에서는 가장 먼저 들어온 자료가 가장 먼저 나간다. 비유하자면, 고속도록 톨게이트에서 들어온 순서대로 요금 정산 후 나가는 형태와 비슷하다.

* 큐의 요구사항
    1. 자료를 저장한 공간
    2. 큐의 크기를 확인하는 기능
    3. 자료를 큐에 넣는 메서드: enqueue
    4. 자료를 큐에서 제거하는 메서드: deququq

* 자바스크립트를 이용하여 큐의 구현: Pseudoclassical

```javascript
const Queue = function () {
    this.storage = {};
    this.start = 0;
    this.end = 0;
}
// Queue라는 생성자 함수 선언
// 자료를 저장하는 프로퍼티: storage (type: Object)
// 자료를 입력하는 위치 프로퍼티: end (type: Number)
// 자료를 출력하는 위치 프로퍼티: start (type: Number)

Queue.prototype.enqueue = function (value) {
    this.storage[this.end] = value;
    this.end++
};
// 입력위치인 end번째 인덱스에 value 저장
// end + 1

Queue.prototype.dequeue = function () {
    let de = this.storage[this.start];
    delete this.storage[this.start];

    if (this.start !== this.end) this.start++

    return de;
};
// 출력위치인 start번째 인덱스에 value 제거
// start와 end의 위치가 다를 경우(저장된 값이 있을 경우) start + 1

Queue.prototype.size = function () {
    return this.end - this.start;
};
// 저장된 자료의 갯수 리턴
```