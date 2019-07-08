# Stack & Queue & Linked List

> [Data-structure & Algorithm](https://wayhome25.github.io/cs/2017/04/17/cs-18/)

## 1. Stack

![stack](https://user-images.githubusercontent.com/42695954/60786321-1bc90700-a191-11e9-98ad-dda2eb422cc0.PNG)

* push : stack의 가장 위(top)에 추가
* pop : stack의 끝(top)에서 자료 삭제

### ● LIFO (Last In First Out)
: 나중에 넣은 데이터를 먼저 처리

### ● Big O
- Insertion : O(1)  ---  stack의 끝에만 추가하기 때문 
- Deletion : O(1)  ---  stack의 끝에서만 제거하기 때문
- Search : O(n)
> **`n`은 상황에 따라 달라진다.** 현재 n은 stack의 사이즈를 의미.

### ● Real Life Use Cases

stack은 서로 관계가 있는 여러 작업을 연달아 수행하면서 이전의 작업 내용을 저장해 둘 필요가 있을 때 사용한다.

- Undo / Redo Mechanism
- Backwards/Forwards Mechanism of Browsers
- Call stack

&nbsp; 

## 2. Queue

![queue](https://user-images.githubusercontent.com/42695954/60787353-82035900-a194-11e9-8e91-4368f7e8731d.PNG)

* Enqueue : 추가
* Dequeue : 삭제

### ● FIFO (First In First Out)
: 먼저 넣은 데이터를 먼저 처리

### ● Big O
- Insertion : O(1)
- Deletion : O(1) 
> 지우고자 하는 데이터를 찾는데 걸리는 시간(n)은 **제외**하고 제거하는 행위만 따진다.
- Search : O(n)

### ● Real Life Use Cases

큐는 순서대로 처리해야 하는 작업을 임시로 저장해두는 버퍼(buffer)로서 많이 사용된다.

- Line of people standing for something
- Callback queue (ref. Event Loop / 함수들이 줄을 서있다가 맨 처음 함수부터 실행)

&nbsp; 

## 3. Linked List

![linkedlist](https://user-images.githubusercontent.com/42695954/60786342-34d1b800-a191-11e9-80bb-5c973b3496fb.PNG)

연결 리스트, 링크드 리스트(linked list)는 각 노드가 데이터와 포인터를 가지고 한 줄로 연결되어 있는 방식으로 데이터를 저장하는 자료 구조이다. 이름에서 말하듯이 데이터를 담고 있는 노드들이 연결되어 있는데, 노드의 포인터가 다음이나 이전의 노드와의 연결을 담당하게 된다.

### ● 구조
linked list는 노드와 포인터(링크)로 구성되어 있다.  
- **노드**  
실제 정보를 담고 있는 하나의 단위
- **링크 (포인터)**  
다른 변수, 혹은 그 변수의 메모리 공간주소를 가리키는 변수를 말한다. linked list에서 링크는 노드간의 위치정보를 저장하고 있어 linked list의 순서를 유지할 수 있도록 하는 연결고리의 역할을 한다. 링크가 가리키는 값을 가져오는 것을 역참조라고 한다.  

노드의 시작점은 head / 끝은 tail이라 부른다.

### ● 종류
* 단일 연결 리스트  

![singlylinkedlist](https://user-images.githubusercontent.com/42695954/60786362-4915b500-a191-11e9-9ac6-74b8e7c5213d.PNG)

단일 연결 리스트는 각 노드에 자료 공간과 한 개의 포인터 공간이 있고, 각 노드의 포인터는 다음 노드를 가리킨다.

* 이중 연결 리스트  

![doublylinkedlist](https://user-images.githubusercontent.com/42695954/60786378-5632a400-a191-11e9-84ee-1825feb22e02.PNG)

이중 연결 리스트의 구조는 단일 연결 리스트와 비슷하지만, 포인터 공간이 두 개가 있고 각각의 포인터는 앞의 노드와 뒤의 노드를 가리킨다.

* 원형 연결 리스트  

![circularlinkedlist](https://user-images.githubusercontent.com/42695954/60786394-634f9300-a191-11e9-8ec6-60862bdbab21.PNG)

원형 연결 리스트를 일반적인 연결 리스트에 마지막 노드와 처음 노드를 연결시켜 원형으로 만든 구조이다.

### ● Big O
- Insertion : O(1)
- Deletion : O(1)
- Search : O(n)

연결 리스트는 늘어선 노드의 중간지점에서도 자료의 추가와 삭제가 O(1)의 시간에 가능하다는 장점을 갖는다. 그러나 배열이나 트리 구조와는 달리 특정 위치의 데이터를 검색해 내는데에는 O(n)의 시간이 걸리는 단점도 갖고 있다.

### ● Real Life Use Cases

- The history section of web browser
- Line of people standing for something

### ● Array vs Linked List

#### 1) 메모리의 구조

![arrayvslinked](https://user-images.githubusercontent.com/42695954/60788133-f212de80-a196-11e9-9158-8273bd0522ca.PNG)

Memory상의 각각의 주소를 찾아가는데 걸리는 시간은 전부 동일하다.  
=> Random Access Memory (RAM)  

* Array는 Array를 구성하는 각각의 element들이 메모리상에 **연속적으로 위치한다**.
* Linked List는 각각의 데이터들이 메모리상에 **흩어져있다**. 대신, 각각의 데이터는 다른 데이터(들)의 위치 정보를 가지고 있다.

#### 2) 연결성

![vertex](https://user-images.githubusercontent.com/42695954/60788514-dfe57000-a197-11e9-8bc5-644c47e4f29f.PNG)

* Array list에서는 element라는 이름을 사용했지만, linked list와 같이 연결된 element들은 **노드**(node, 마디, 교점의) 혹은 **버텍스**(vertex, 정점, 꼭지점)라고 부른다. --- **연결성**이 강조된 표현
* node를 구현할때, 일반적으로 C언어의 경우 구조체를 이용하고 객체지향언어에서는 객체를 이용한다.
* node는 두 개의 변수를 가지고 있다. (data field & link field)  
link field에는 다음 node가 무엇인지에 대한 정보를 담고 있다. 
* 첫 번째 node의 위치(head 또는 first)를 찾아야 다음 node를 찾아나갈 수 있다.

#### 3) 데이터 탐색/추가/삭제

![tradeoff](https://user-images.githubusercontent.com/42695954/60789805-f17c4700-a19a-11e9-941d-68de27324c0d.PNG)

`trade off`는 어떤 특성이 좋아지면 다른 특성이 나빠지는 상황을 의미한다. Array와 Linked List는 trade off의 좋은 사례라고 할 수 있다.

**3-1) 데이터 탐색 속도 → Array가 우세**  
* Array는 데이터가 메모리상에 연속적으로 존재한다는 특성 때문에 데이터에 대한 접근 속도가 매우 빠르다.
* Linked List의 경우 각 노드가 메모리 공간상에 어디에 위치하는지는 각각의 노드들만 알고있고 사용자는 첫 번째 노드의 위치(head)만 알고 있기 때문에 배열에 비해 데이터에 접근하는데 시간이 많이 소요된다.  
(n개의 데이터가 있는 linked list에서 찾고자 하는 값이 맨 마지막 node에 위치할 경우 n번의 접근을 해야 한다.)

**3-2) 데이터 추가 및 삭제 → Linked List가 우세**  
* Array는 데이터 추가 삭제 시 Array의 최대 크기를 변경할 수 없어 제약이 발생하며,  다른 원소들의 위치를 이동시켜야 하기 때문에 효율성이 떨어진다.
* Linked List는 데이터를 무분별하게 삽입하더라도 그에 맞게 리스트가 자동으로 확장/축소 되기 때문에 메모리를 효율적으로 활용할 수 있으며 링크를 통해 다른 데이터를 참조할 수 있어 데이터 추가/삭제 시 다른 원소들을 움직일 필요가 없다.  
=> 데이터의 수가 dynamic하게 바뀌는 경우 Array보다 Linked List를 활용하는것이 적절하다.

&nbsp; 

---

**Reference**  

* [https://ko.wikipedia.org/wiki/%EC%97%B0%EA%B2%B0_%EB%A6%AC%EC%8A%A4%ED%8A%B8](https://ko.wikipedia.org/wiki/%EC%97%B0%EA%B2%B0_%EB%A6%AC%EC%8A%A4%ED%8A%B8)
* [https://opentutorials.org/module/1335/8821](https://opentutorials.org/module/1335/8821)
* [http://blog.naver.com/PostView.nhn?blogId=kiminhovator&logNo=220327380723&parentCategoryNo=&categoryNo=35&viewDate=&isShowPopularPosts=false&from=postView](http://blog.naver.com/PostView.nhn?blogId=kiminhovator&logNo=220327380723&parentCategoryNo=&categoryNo=35&viewDate=&isShowPopularPosts=false&from=postView)
