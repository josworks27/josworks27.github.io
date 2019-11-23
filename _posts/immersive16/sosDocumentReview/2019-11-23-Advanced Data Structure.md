---
layout: post
title: "Data Structure(3)_Advanced Data Structure"
tags: 
    - Immersive 16
    - SOS Document Review

comments: true
---

# Linked List
링크드 리스트에 속한 각 엘리먼트들을 노드(node)라고 부른다. 하나의 노드는 데이터 파트(data part)와 넥스트 파트(next part)의 두 부분으로 이루어져 있다. 데이터 파트에는 값이 저장되어 있고, 넥스트 파트에는 다음 노드를 가리키는 역할을 한다.

* 링크드 리스트에 필요한 프로퍼티
    1. 리스트의 가장 첫 번째 노드: head
    2. 마지막 노드: tail

* 링크드 리스트에 필요한 메서드
    1. 링크드 리스트의 끝에 노드를 추가: addToTail
    2. 가장 앞의 노드를 제거: removeTail
    3. 특정 값이 링크드 리스트 내에 존재하는 확인: contains

* 자바스크립트를 이용하여 링크드 리스트의 구현: functional

```javascript
    const LinkedList = function() {
      const list = {};
    
      /* instance의 head와 tail을 초기화 시켜줍니다. */
      list.head = null;
      list.tail = null;
    
      /* addToTail은 Tail node에 node를 추가하는 메소드입니다. */
      list.addToTail = function(value) {
    
        /* 값이 아닌 새로운 node를 list에 추가하는 것이 핵심입니다.
           새로운 node를 생성해줍니다 */
        const newTail = new Node(value);
    
        /* Head node가 없다면, 다시 말해 list에 어떠한 노드도 없다면 
           방금 생성한 node인 newTail을 할당해줍니다. */
        if (!list.head) {
          list.head = newTail;
        }
    
        /* Tail node가 존재한다면, 즉 list에 어떤 노드가 이미 존재했다면 
    			 해당 Tail node의 next에 생성한 node를 연결해줍니다. */
        if (list.tail) {
          list.tail.next = newTail;
        }
    
        /* Tail node에 새로 생성된 node를 연결해줍니다. */
        list.tail = newTail;
      };
    
      /* removeHead는 instance의 head를 없애는 것입니다. */
      list.removeHead = function() {
    
        /* Head node가 없다면, 없앨 node도 없습니다. */
        if (list.head === null) {
          return null;
        }
    
        /* 삭제할 head node를 잠시 currentHead에 할당해주고, 
    			 삭제할 head node의 다음 node를 새로운 head node로 지정해줍니다. */
        const currentHead = list.head;
        list.head = list.head.next;
    
        /* 삭제한 head node를 return 해줍니다. */
        return currentHead.value;
      };
    
      /* contains는 instance에 해당 node가 있는지 판별하는 메소드입니다.
    	   핵심은, 모든 instance를 순회(traversal)하여야 한다는 것입니다. */
      list.contains = function(target) {
    
        /* 순회를 시작할 node를 지정해줍니다. */
        let node = list.head;
    
        /* node가 null이 되기 전까지, 즉 리스트의 끝에 도달할 때까지 아래를 반복합니다. */
        while (node) {
          /* node의 값이 targe과 같으면 true를 바로 반환합니다. */
          if (node.value === target) {
            return true;
          }
    
          /* node 변수에 현재 node의 다음 node를 할당합니다. */
          node = node.next;
        }
    
        /* 위 과정(순회)이 끝났음에도 불구하고, true를 반환하지 못한다면, 
    			 list 내의 어떤 node의 value도 target과 일치하지 않았다는 뜻입니다.
           false를 return해 줍니다. */ 
        return false;
      };
    
      return list;
    };
    
    const Node = function(value) {
    
      /* 아래와 같이 Node의 경우도 functional style로 작성될 수 있습니다.
         var node = {};
    
    	   node.value = value;
    	   node.next = null;
    
    	   return node;
    
    	   아래는 pseudoclasscial style로 작성되어 있습니다.
    	   value는 node의 value, next는 현재 node의 다음 node를 연결해줍니다. */
    
      this.value = value;
      this.next = null;
    };
```


# Tree
트리는 노드로 이루어진 계층적인 형태를 갖는 자료구조이다. 많은 양의 정보를 보기 쉽게 정리할 때 유용하며 족보, 컴퓨터 디렉토리, 회사 조직도 등이 대표적인 트리구조이다.

* 트리의 주요 개념
    * 데이터를 갖는 노드(node)
    * 최상위 노드 root
    * root를 기준으로 다른 노드로의 접근을 위한 거리 depth
    * 같은 depth에 존재하는 노드들은 slibling 관계
    * 상위 depth 노드와 하위 depth 노드의 관계는 부모(parent) - 자식(child)
    * 노드와 노드를 잇는 선 edge

* 트리에 필요한 프로퍼티
    1. 값이 들어갈 프로퍼티: value
    2. 자식들이 들어갈 프로퍼티: children

* 트리에 필요한 메서드
    1. child 노드를 추가: addChild
    2. 특정한 값이 트리 내에 존재하는지 확인: constains
    3. 전체 트리를 순회하며 각 child에게 callback을 적용: traverse

* 자바스크립트를 이용하여 트리의 구현: functional shared

```javascript
    const Tree = function(value) {
      const newTree = {};
      
    	/* node의 값을 instance에 할당합니다.*/
      newTree.value = value;
    
      /* 하위 구조의 children을 배열로 하여 할당합니다. */
      newTree.children = [];
    
      /* Object.assign은 Object.create와 더불어 꼭 알아야하는 method입니다.
    	   mdn을 참조해주세요! */
      Object.assign(newTree, treeMethods);
    
      return newTree;
    };
    
    
    /* 각 트리 노드가 공유하는 method를 담을 객체를 만들어 줍니다. */
    const treeMethods = {};
    
    /* addChild는 부모에 자식을 추가해주는 것입니다. */
    treeMethods.addChild = function(value) {
      
      /* 자식도 부모와 마찬가지로 Tree 형태이여야만 합니다.
    	   instance를 생성해줍니다. */
      const child = Tree(value);
      /* 여기서 this는 부모를 가리킵니다. 자식은 children이라는 배열에 들어갑니다.
    	   따라서, push를 통해 자식을 배열에 추가해줍니다. */
      this.children.push(child);
    };
    
    /* contains는 해당 target이 tree에 있는지 여부를 따지는 것입니다. */ 
    treeMethods.contains = function(target) {
      /* target과 같은 값이 있다면, true를 return 해줍니다. */
      if (this.value === target) {
        return true;
      }
      /* 자식들은 배열에 각 tree의 instance로 저장되어 있습니다. 
    	   for loop를 이용하여 해당 자식들에 대하여 접근할 수 있습니다. */
      for (let i = 0; i < this.children.length; i += 1) {
        /* 자식 하나를 child 변수로 할당하여, contains method를 recursive 동작하게 합니다.*/
        const child = this.children[i];
        if (child.contains(target)) {
          /* 만약 그 결과가 true라면, true를 return해줍니다. */
          return true;
        }
      }
      /* 위 과정을 모두 마치고도 return true를 하지 못했다면, 해당 target이 없다는 뜻입니다. */
      return false;
    };
    
    /* traverse는 전체 트리를 순회하며, 각 노드에 callback을 적용시키는 메소드입니다. */
    treeMethods.traverse = function(callback) {
      /* 현재 노드의 값을 callback 함수의 인자로 넣어줍니다. */
      callback(this.value);
    
      /* 만약 자식이 없다면, 순회를 중단합니다. */
      if (!this.children) {
        return;
      }
      /* 모든 자식들이 순회되어야 하기 때문에, contains와 같이 recursive하게 구현합니다. */
      for (let i = 0; i < this.children.length; i += 1) {
        const child = this.children[i];
        child.traverse(callback);
      }
    };
```
# Graph
그래프는 여러 노드(vertex)와 노드를 연결하는 엣지(arc)로 이루어져 있다. 노드와 노드 간의 관계를 표현하기 위해 그래프 구조를 사용한다. 또한 그래프는 방향성을 갖는지에 따라 방향그래프/무방향그래프로 나뉜다.

* 그래프에 필요한 메서드
    1. 새로운 노드를 그래프에 추가:addNode
    2. 어떤노드가 그래프 내에 존재하는지 확인: contains
    3. 그래프에서 노드를 제거: removeNode *해당 노드에 연결된 엣지도 함께 삭제
    4. 그래프를 순회하며 전체 노드들에 callback을 적용: forEachNode
    5. 노드와 노드 사이에 엣지를 생성: addEdge
    6. 노드와 노드 사이의 엣지를 제거: removeEdge
    7. 두 노드가 연결되어 있는지 확인: hasEdge

* 자바스크립트를 이용하여 그래프의 구현: pseudoclassical

```javascript
    const Graph = function() {
      /* 객체로 노드가 담길 그래프를 구현합니다. */
      this.nodes = {};
    };
    
    /* addNode는 Node를 추가해 줍니다. || 연산자를 이용한 초기화에 대하여 알아보세요!
    	 (search keyword : initialization with logical operator) */
    Graph.prototype.addNode = function(node) {
      this.nodes[node] = this.nodes[node] || { edges: {} };
    };
    
    /* contains는 해당 node가 그래프에 있는지 찾아 줍니다.
    	 객체로 구현된 node를 찾습니다.
    	 !!에 대해서는 pre-course에서 학습하신 trusy & falsy에 대해 알아보세요! */
    Graph.prototype.contains = function(node) {
      return !!this.nodes[node];
    };
    
    /* removeNode는 node를 없애는 것입니다. */
    Graph.prototype.removeNode = function(node) {
      /* node를 없애기 위해서, 일단 node가 존재하는지 확인합니다. */
      if (this.contains(node)) {
        /* 삭제할 node와 연결된 다른 모든 node 사이의 edge들을 제거합니다.
    	     이 과정 역시 recursion을 이용해 구현합니다. */
        const keys = Object.keys(this.nodes[node].edges);
        for (let i = 0; i < keys.length; i += 1) {
          this.removeEdge(node, keys[i]);
        }
    		/* node를 삭제합니다. */
        delete this.nodes[node];
      }
    };
    
    /* hasEdge는 2개의 node 간에 edge가 있는지를 판별합니다.
    	 removeNode에서 쓰인 것과 같은 연산자를 활용하여 간단하게 표현할 수 있습니다. */
    Graph.prototype.hasEdge = function(fromNode, toNode) {
      if (!this.contains(fromNode)) {
        return false;
      }
      return !!this.nodes[fromNode].edges[toNode];
    };
    
    /* addEdge는 두 node 간에 edge를 추가합니다.*/
    Graph.prototype.addEdge = function(fromNode, toNode) {
      /* 만약 2개의 node중에 존재하지 않는 node가 있다면 종료합니다. */
      if (!this.contains(fromNode) || !this.contains(toNode)) {
        return;
      }
    
      /* 객체 참조를 이용해서 2 개의 노드의 edge로 참조시켜줍니다. */
      this.nodes[fromNode].edges[toNode] = toNode;
      this.nodes[toNode].edges[fromNode] = fromNode;
    };
    
    /* removeEdge는 2개의 node 간의 edge를 제거하는 것입니다. */
    Graph.prototype.removeEdge = function(fromNode, toNode) {
      
      /* addEdge와 같이, 존재하지 않는다면, 종료합니다. */
      if (!this.contains(fromNode) || !this.contains(toNode)) {
        return;
      }
    
      /* delete를 이용하여 객체의 key/value를 없앱니다. */
      delete this.nodes[fromNode].edges[toNode];
      delete this.nodes[toNode].edges[fromNode];
    };
    
    /* for loop를 통해 각 node를 callback으로 전달합니다. */
    Graph.prototype.forEachNode = function(cb) {
      const keys = Object.keys(this.nodes);
      for (let i = 0; i < keys.length; i += 1) {
        cb(keys[i]);
      }
    };
```


# Hash Table
해싱(Hasing)이란 데이터 관리를 위해 다양한 길이의 데이터를 고정된 형태의 데이터로 대응(mapping)시키는 작업을 뜻한다. 이러한 기능을 구현한 함수를 해시 함수라고 한다.

* 해시 함수의 특징
    1. 주어진 저장소의 크기 만큼의 값(0부터 저장소.length-1)을 해시값으로 반환 필요
    2. 특정한 키를 받으면 항상 같은 값을 반환
    3. 해시 함수는 어떠한 값도 저장하지 않고, 호출되었을 때 해시 값을 반환하는 역할

* 해시 테이블에 필요한 프로퍼티
    1. 해시 테이블에 데이터가 얼마나 담겨 있는지 확인: size
    2. 해시 테이블의 최대 크기를 설정: limit
    3. 데이터 저장소: storage *limit만큼의 크기를 가짐

* 해시 테이블에 필요한 메서드
    1. 새로운 key-value를 저장: insert
    2. key를 이용하여 value 확인: retrieve
    3. key를 이용하여 해당 key와 value를 제거: remove
    4. limit 값이 바뀔 때마다 size를 변경: resize

* 자바스크립트를 이용하여 그래프의 구현: pseudoclassical

```javascript
    const { LimitedArray, getIndexBelowMaxForKey } = require("./hashTableHelpers");
    
    const HashTable = function() {
      this._size = 0;
      this._limit = 8;
      this._storage = LimitedArray(this._limit);
    };
    
    /* insert를 통해 storage에 새로운 key/value를 저장합니다. */
    HashTable.prototype.insert = function(k, v) {
    
      /* 해당 key의 hash 값을 얻습니다. (index를 얻습니다.) */
      const index = getIndexBelowMaxForKey(k, this._limit);
    
      /* index를 이용해, bucket을 얻습니다. 
    		 undefined라면, 즉 기존 bucket이 없다면 빈 bucket인 []을 얻습니다. */
      const bucket = this._storage.get(index) || [];
    
      
      for (let i = 0; i < bucket.length; i += 1) {
        const tuple = bucket[i];
        /* bucket의 일부인 tuple에서의 key값이 발견된다면, value를 return 해줍니다. */
        if (tuple[0] === k) {
          const oldValue = tuple[1];
          tuple[1] = v;
          return oldValue;
        }
      }
    
      /* key, value를 bucket에 push해줍니다. */
      bucket.push([k, v]);
      this._storage.set(index, bucket);
      this._size += 1;
    
      /* 만약 storage에서 데이터가 차지하고 있는 값이 최대 사이즈의 75%이상이라면, 2배로 resize합니다. */
      if (this._size > this._limit * 0.75) {
        this._resize(this._limit * 2);
      }
    
      return;
    };
    
    /* retrieve는 key를 이용하여 value를 얻는 것입니다. */
    HashTable.prototype.retrieve = function(k) {
      /* hash function을 이용하여, index를 얻습니다. */
      const index = getIndexBelowMaxForKey(k, this._limit);
    
      /* index를 이용해, bucket을 얻습니다. (undefined라면 []을 얻습니다.) */
      const bucket = this._storage.get(index) || [];
    
      /* bucket에서 value를 찾아 return 합니다. */
      for (let i = 0; i < bucket.length; i += 1) {
        const tuple = bucket[i];
        if (tuple[0] === k) {
          return tuple[1];
        }
      }
     
      /* 어느 값도 찾지 못했다면, undefined를 return 합니다. */
      return;
    };
    
    /* remove는 key를 인자로 받아, 해당 key와 value를 제거합니다. */
    HashTable.prototype.remove = function(k) {
    
      /* hash 함수를 이용해서, index를 얻습니다. */
      const index = getIndexBelowMaxForKey(k, this._limit);
    
      /* index를 이용해, bucket을 얻습니다. (undefined라면 []을 얻습니다.) */
      const bucket = this._storage.get(index) || [];
    
      /* bucket에서 value를 찾습니다. */
      for (let i = 0; i < bucket.length; i += 1) {
        const tuple = bucket[i];
        
        if (tuple[0] === k) {
          /* 만약 인자로 받은 key에 해당하는 value가 있다면, 
    				 bucket에서 해당 key/value를 없앱니다. */
          bucket.splice(i, 1);
          this._size -= 1;
    
          /* 없앤 후, 만약 사이즈가 최대 25%이내라면, 절반으로 resize합니다. */
          if (this._size < this._limit * 0.25) {
            this._resize(Math.floor(this._limit / 2));
          }
          return tuple[1];
        }
      }
    
      /* 인자로 받은 key가 해시 테이블 내에 존재하지 않았다면 undefined를 리턴합니다. */
      retur;
    };
    
    /* resize는 새로운 limit 값을 기반으로 storage의 size를 변경합니다. */
    HashTable.prototype._resize = function(newLimit) {
      /* 기존 저장소를 잠시 저장해둡니다. */
      const oldStorage = this._storage;
    
      /* 사이즈가 8보다 작은 경우는 resize 하지 않습니다. */
      newLimit = Math.max(newLimit, 8);
      if (newLimit === this._limit) {
        return;
      }
    
      /* storage를 새로 만듭니다. */
      this._limit = newLimit;
      this._storage = LimitedArray(this._limit);
      this._size = 0;
    
      /* 이미 저장되어 있던 bucket들을 새 저장소에 다시 넣어줍니다. */
      oldStorage.each(bucket => {
        if (!bucket) {
          return;
        }
        for (let i = 0; i < bucket.length; i += 1) {
          const tuple = bucket[i];
          this.insert(tuple[0], tuple[1]);
        }
      });
    };
```
# Binary Search Tree

* 바이너리 서치 트리의 특징
    1. 트리의 각 노드가 갖는 자식 노드의 수가 2개 이하
    2. 부모 노드의 왼쪽 하위 트리에 있는 노드의 데이터는 부모 데이터 보다 작거나 같음
    3. 부모 노드의 오른쪽 하위 트리에 있는 노드의 데이터는 부모 데이터 보다 큼

* 바이너리 서치 트리를 순회하는 방법
    1. DFS(Depth First Search) 깊이 우선 탐색
    2. BFS(Breadth First Search) 너비 우선 탐색

* 바이너리 서치 트리에 필요한 프로퍼티
    1. 값이 들어갈 프로퍼티
    2. 오른쪽 자식 노드가 들어갈 프로퍼티(부모보다 큰)
    3. 왼쪽 자식 노드가 들어갈 프로퍼티(부모보다 작거나 같은)

* 바이너리 서치 트리에 필요한 메서드
    1. child노드를 추가: insert
    2. 특정한 값이 트리에 있는지 확인: contains
    3. DFS방식으로 각 노드에 callback을 적용: depthFirstLog

* 자바스크립트를 이용하여 바이너리 서치 트리의 구현: prototypal

```javascript
/* Prototypal style의 특징은 instance 를 Object.create()를 통해 만들고,
    	 함수 외부 binaryTreePrototype에 메소드를 작성하는 것입니다. */
    
    const BinarySearchTree = function(value) {
      const binaryTree = Object.create(binaryTreePrototype);
      binaryTree.value = value;
      binaryTree.left = null;
      binaryTree.right = null;
    
      return binaryTree;
    };
    
    const binaryTreePrototype = {};
    
    /* insert는 새로운 node를 트리에 추가합니다. */
    binaryTreePrototype.insert = function(val) {
      if (val < this.value) {
        /* 만약 인자로 받은 값 val이 현재 노드의 값보다 작고, 노드의 left 값이 null이라면, 
    	     val을 value로 하는 새로운 BST를 만들어 현재 노드의 왼쪽 하위에 할당합니다. */
        if (this.left === null) {
          this.left = BinarySearchTree(val);
        } else {
          /* left가 null이 아니라면, 현재 노드의 왼쪽 자식에 대해 insert를 다시 실행합니다. */
          this.left.insert(val);
        }
      } else if (val > this.value) {
         /* 만약 인자로 받은 값 val이 현재 노드의 값보다 크고, right 값이 null이라면, 
    	      새로운 BST를 만들어 현 노드의 오른쪽 하위에 할당합니다. */
        if (this.right === null) {
          this.right = BinarySearchTree(val);
        } else {
          /* right가 null이 아니라면, 현재 노드의 오른쪽 자식에 대해 insert를 다시 실행합니다. */
          this.right.insert(val);
        }
      }
    };
    
    /* contains는 BST를 순회하여,인자로 받은 값이 있는지 판단합니다. */
    binaryTreePrototype.contains = function(val) {
      /* 현재 node(value)와 같다면, true를 return 합니다. */
      if (val === this.value) {
        return true;
      }
      if (val < this.value) {
        /* value가 현재 node(value)보다 작다면, 2가지를 check합니다.
    	     this.left가 비어있다면, false를 return 합니다. (!!null === false) 
    	     this.left가 비어있지 않다면, left에서의 contains를 다시 실행합니다.
    	     &&에 대해서는 console.log의 아래의 것들을 시도해보세요!
    		     console.log(undefined && 1);
    		     console.log(null && 1);
    		     console.log(2 && 1);
    		     console.log('a' && 1);
    		     + fasly & trusy에 대해서 재학습하세요!*/
        return !!(this.left && this.left.contains(val));
      }
      if (val > this.value) {
        /* 위 logic과 같습니다. */
        return !!(this.right && this.right.contains(val));
      }
    };
    
    /* DF는 깊이우선 탐색을 뜻합니다. 
    	 depthFirstLog는 DF 탐색시, 매 노드의 값에 대하여 callback을 실행시킵니다. */
    binaryTreePrototype.depthFirstLog = function(callBack) {
      /* 현재 value의 callback을 실행합니다. */
      callBack(this.value);
    
      /* 깊이우선 탐색이므로, left값의 값이 존재한다면, 계속 depthFirstLog를 실행시킵니다. */
      if (this.left) {
        this.left.depthFirstLog(callBack);
      }
      /* left값이 더이상 존재하지 않는다면, rigth값도 똑같이 실행시켜줍니다. */
      if (this.right) {
        this.right.depthFirstLog(callBack);
      }
    };
```