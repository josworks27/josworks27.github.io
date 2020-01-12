---
layout: post
title: "TIL - 시간복잡도(Time Complexity)"
tags: 
    - TIL
comments: true
---


## 1. Complexity Analysis
* 복잡도 분석이란, 알고리즘이 문제 해결을 위한 시간과 공간을 어람나 차지하는지를 분석함
* 복잡도 분석의 중요성: 알고리즘이 얼마나 효울적인지 알 수 있음
* 프로그래밍 인터뷰에서 중요성이 강조되고 있음

![배열 예시 그림](/images/immersive16/data_structure(complexity)_1.jpg)
* 모든 숫자 비교: n2 => O(n²)
* 가장 큰 수와 작은수를 확인하여 비교: 2n => O(n)
* 정렬하여 첫번쨰 수, 마지막 수의 차 비교: 3 => O(1)

``` 
대표적인 예시이며 훨씬 더 다양함

SLOW

    O(C²) - Expohential: 메모이제이션 하지 않는 피보나치수열

    O(n²) - Quadratic: constant time operation inside two nested for-loops

    O(n) - Linear: Linked List, Linear와 관련된 배열 

    O(log n) - Logarithmic: Binary Search

    O(1) - Constant: Array Lookup, Hash Table 등

FAST
```

![복잡도 그래프](/images/immersive16/1920px-Comparison_computational_complexity.svg.png)

## 2. Data Structure With Time Complexity

## 2.1. Array
1. Lookup(position): O(1) => 배열의 인덱스를 알고 있기 때문에 바로 접근
2. Assign: O(1) => 배열의 인덱스를 알고 있기 때문에 바로 접근
5. Find: O(n) => 첫 번째 인덱스부터 차례대로 접근
3. Insert: O(n) => 삽입 후 다른 엘리먼트들을 재정렬
4. Remove: O(n) => 지우고 다른 엘리먼트들을 재정렬


## 2.2. Linked List
1. Lookup: O(n) => 헤드부터 차례대로 접근 (배열과 같은 느낌)
2. Assign: O(n) => 상동
5. Find: O(n) => 상동
3. Insert: O(1) => 테일에 붙이면 바로 접근
    * 중간에 추가하더라도 추가할 위치의 노드를 알기 때문에 O(1)
4. Remove: 헤드는 O(1), 중간은 O(n)
    * 중간에 경우는 헤드부터 찾아야 하기 때문


## 2.3. Doubly Linked List
1. Lookup: O(n)
5. Assign: O(n)
4. Find: O(n)
2. Insert: O(1)
3. Remove: O(1)
보완


## 2.4. Trees: basic
1. Find: O(n) => DFS, BFS 하더라도 하나씩 다 찾아야 하기 때문
보완


## 2.5. Trees: Binary Search Tree
1. Find: O(log n) => 기준이 되는 수보다 크거나 작은 경우만 따지기 떄문에 굉장히 빠름

```
Algorithm	Average         Worst case
Space		O(n)	        O(n)
Search		O(log n)	    O(n)
Insert		O(log n)	    O(n)
Delete		O(log n)	    O(n)
```

* 하지만, 한 쪽으로 치우쳐진 트리의 경우 배열고 다를 바 없기 때문에 O(n)
* 이를 해결하기 위해 밸런스 조정을 한다면 O(log n)
보완

* 이 밖에도 많지만 추가적으로 공부하고 보완해야 함
* 자료구조의 복잡도는 각가의 자료구조가 갖고 있는 장점과 단점을 아는 것이 중요함
* 복잡도를 이해하는 것은 문제해결을 위한 올바른 자료구조를 선택하는데 도움이 됨


참고자료
1.데이터 구조, 정렬 알고리즘 별 시간/공간 복잡도 요약표 https://keichee.tistory.com/272