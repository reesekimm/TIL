연결 리스트는 각 노드가 다음에 노드에 대한 참조 정보를 가지고 있는 자료구조이다.

배열에서 Element라는 용어를 쓰듯이 연결 리스트에서는 노드(Node) 또는 버텍스(Vertex, 정점, 꼭지점)라는 용어를 쓴다. **연결성**이 강조된 표현이라고 보면 된다.

<br />

## 배열 vs 연결리스트

이 둘은 **trade off 관계** 의 좋은 사례이다. (trade off : 어떤 특성이 좋아지면 다른 특성이 나빠지는 상황)

![https://i.postimg.cc/htkjKpFF/image.png](https://i.postimg.cc/htkjKpFF/image.png)

![https://i.postimg.cc/R0TNGKYj/image.png](https://i.postimg.cc/R0TNGKYj/image.png)

### **데이터 탐색 속도 → Array가 우세**

- Array는 데이터가 메모리상에 연속적으로 존재한다는 특성 때문에 데이터에 대한 접근 속도가 매우 빠르다.
  (단점 : 고정된 크기를 차지하기 때문에 크기가 커지면 이를 수용할 수 있는 메모리 위치를 찾아 통째로 이동해야 한다.)
- Linked List의 경우 각 노드가 메모리 공간상에 어디에 위치하는지는 각각의 노드들만 알고있고 사용자는 첫 번째 노드의 위치(head)만 알고 있기 때문에 배열에 비해 데이터에 접근하는데 시간이 많이 소요된다.
  (n개의 데이터가 있는 linked list에서 찾고자 하는 값이 맨 마지막 node에 위치할 경우 n번의 접근을 해야 한다.)

### **데이터 추가 및 삭제 → Linked List가 우세**

- Array는 데이터 추가 삭제 시 Array의 최대 크기를 변경할 수 없어 제약이 발생하며, 다른 원소들의 위치를 이동시켜야 하기 때문에 효율성이 떨어진다. (요소를 추가하거나 삭제하는 경우 해당 요소의 뒤에 있는 모든 요소의 인덱스 shifting이 필요)
- Linked List는 데이터를 무분별하게 삽입하더라도 그에 맞게 리스트가 자동으로 확장/축소 되기 때문에 메모리를 효율적으로 활용할 수 있다. (동적자료구조) 링크를 통해 다른 데이터를 참조할 수 있어 데이터 추가/삭제 시 다른 원소들을 움직일 필요가 없다.
  → 데이터의 수가 dynamic하게 바뀌는 경우 Array보다 Linked List를 활용하는것이 적절하다.

<br />

![https://i.postimg.cc/VvJhD9Kg/image.png](https://i.postimg.cc/VvJhD9Kg/image.png)

<br />

> 포인터 개념 잡기

```jsx
let obj1 = { a: true };
let obj2 = obj1; // pointer : 특정 값의 메모리 주소를 참조하는 것

obj1.a = "booya";

console.log("1", obj1); // 1 { a: "booya" }
console.log("2", obj2); // 2 { a: "booya" }

delete obj1; // ?? 객체를 통째로 삭제하는게 가능한가? delete는 프로퍼티를 지우는 메서드 아닌가?

console.log("1", obj1); // Error: obj1 is not defined
console.log("2", obj2); // 2 { a: "booya" }

// obj1는 메모리상에서 지워졌지만 (garbage collected)
// obj2는 여전히 { a: "booya" } 의 메모리주소를 참조하고 있음 (pointing!)

obj2 = "hello";

console.log(obj2); // "hello"
// { a : "booya" } 는 메모리상에서 지워짐 (garbage collection)
```

<br />

### 단일 연결 리스트 vs 이중 연결 리스트

단일 연결 리스트는 node에 next 포인터만 있기 때문에 이중 연결 리스트에 비해 구현이 간단하고 메모리를 적게 소모한다.

이중 연결 리스트는 추가적으로 prev 포인터를 가지기 때문에 단일 연결 리스트에 비해 구현 코드양이 조금 더 많고 메모리를 많이 차지한다. 대신 prev 포인터를 사용해서 tail에서부터 탐색하는게 가능하다. 따지고보면 시간복잡도가 O(n/2)이기 때문에 좀 더 효율적이다. 양방향 순회/탐색이 필요한 경우 유용하다.

<br />

### Summary

![https://i.postimg.cc/g28jMJzM/image.png](https://i.postimg.cc/g28jMJzM/image.png)

<br />

[Linked List 구현](https://repl.it/repls/InsecureVioletredAngles)

[Doubly Linked List 구현](https://repl.it/repls/CarefreeHiddenBackticks#index.js)

<br />

---

[생활코딩 - 연결리스트](https://opentutorials.org/module/1335/8821)

[Master the Coding Interview](https://www.udemy.com/course/master-the-coding-interview-data-structures-algorithms/learn/lecture/12324586#overview)
