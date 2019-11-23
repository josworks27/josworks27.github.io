---
layout: post
title: "Data Structure(4)_Time Complexity"
tags: 
    - Immersive 16
    - SOS Document Review

comments: true
---

# Time Complexity
Time Complexity에 대해서 중요한 것은 자신이 작성하는 알고리즘의 시간보잡도에 대해 고민할 수 있는 자세다. 알고리즘을 작성할 때, 시간복잡도를 고민함과 동시에 적당한 데이터구조를 선택하는 것은 개발자의 중요한 역량 중 하나이다.

## Big-O 노테이션의 대표적인 사례

1. O(1) constant time
  : 입력 값 n이 주어졌을 때, 알고리즘이 문제를 해결하는데 오직 한 단계만 거친다.

```javascript
var dummyArray = [1, 2, 3, 4];
    dummyArray.pop() // 4
```

2. O(log N) log time
  : 입력 값 n이 주어졌을 때, 문제를 해결하는데 필요한 단계들이 연산마다 특정 요인에 의해 줄어든다.
  
```javascript
function FindItemBinarySearch(items, match) {
          var low = 0,
              high = items.length -1;
    
          while (low <= high) {
              mid = parseInt((low + high) / 2);
    
               current = items[mid];
    
              if (current > match) {
                 high = mid - 1;
               } else if (current < match) {
                  low = mid + 1;
                } else {
                  return mid;
               }   
          }       
    
          return -1;
      }
```

3. O(n) linear time
  : 문제를 해결하기 위한 단계의 수와 입력 값 n이 1대1 관계를 가진다.

```javascript
Array.prototype.indexOf = function (element){
      for (var x = 0, count = this.length; x < count; x++){
        if(this[x] === element){
          return x;
        }
      }
      return -1;
    };
```

4. O(n^2) quadratic time
  : 문제를 해결하기 위한 단계의 수는 입력 값 n의 제곱이다.

```javascript 
const buildMatrix = (n) => {
      var matrix = new Array(n);
      for (var i = 0; i < n; i++) {
          var cols = new Array(n);
          matrix[i] = cols;
          for (var j = 0; j < n; j++) {
            cols[j] = j
          }
      }
      return matrix
    }
```

5. O(c^n) exponential time
  : 문제를 해결하기 위한 단계의 수는 주어진 상수 값 C의 n 제곱이다.

```javascript
function fibonacci(num) {
    	var answer = 0;
    	if( num <= 1 ) {
    		return num;
    	}
    	else if( num > 1 ) {
    		answer = fibonacci(num-1) + fibonacci(num-2);
    	}
    	return answer;
    }
```

![시간 복잡도](./_posts/immersive16/sosDocumentReview/sosImages/3.png)