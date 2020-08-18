# 이터레이터 & 제너레이터

## 이터레이터 (iterator)

어떤 객체가 `이터러블(iterable)하다`는건 다음의 두 가지 조건을 충족한다는 뜻이다.

1. 객체 또는 해당 객체의 prototype chain 객체들 중 하나가 [`[Symbol.iterator]()`](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Symbol/iterator) 메서드를 가지고 있다.  
   (= [iterable protocol](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols)을 따른다)
2. `[Symbol.iterator]()` 메서드는 [iterator protocol](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols)을 따르는 객체(=**이터레이터**)를 리턴한다. 이터레이터 객체는 `next()` 메서드를 가지고 있고 `next()` 메서드는 `{ done, value }` 객체를 리턴한다.

```js
const arr = [1, 2, 3];

console.log(typeof arr[Symbol.iterator]); // function

const iterator = arr[Symbol.iterator]();
console.log(iterator); // Array Iterator

iterator.next(); // { value: 1, done: false }
iterator.next(); // { value: 2, done: false }
iterator.next(); // { value: 3, done: false }
iterator.next(); // { value: undefined, done: true }
iterator.next(); // { value: undefined, done: true }
```

이터레이터는 책갈피에 비유할 수 있다. `next()`를 호출하면 책을 한 페이지씩 읽으면서 지금 읽고있는 페이지의 내용을 알려준다. 대신 한 번 읽고 나면 이전 페이지로 되돌아가지 않는다.

```js
const arr = [1, 2, 3, 4, 5];

const iterator = a[Symbol.iterator]();

iterator.next(); // { value: 1, done: false }
iterator.next(); // { value: 2, done: false }

for (const el of iterator) console.log(el); // arr가 아닌 iterator를 순회한다.
```

> 출력결과

```
3
4
5
```

`next()`를 호출하여 2까지 이동하고 나면 iterator를 순회할때 나머지 3, 4, 5만 출력되는걸 알 수 있다.  
여기서 추가적으로 알 수 있는건, **이터레이터도 이터러블 객체**라는 점이다.

<br />

### Built-in 이터러블 객체

`Sring`, `Array`, `Map`, `Set`, `TypedArray` 객체는 모두 [built-in 이터러블 객체](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols#Built-in_iterables)이기 때문에 [`for...of`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...of)로 객체를 순회하거나 [spread syntax](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax)를 사용할 수 있다.

`Map.prototype.keys()`, `Map.prototype.values()`, `Map.prototype.entries()` 같은 메서드들은 모두 새로운 이터레이터를 반환한다. 따라서 `for...of` 메서드를 사용해서 순회할 수 있다.

```js
let myMap = new Map();
myMap.set(0, 'zero');
myMap.set(1, 'one');

for (let [key, value] of myMap) {
  console.log(key + ' = ' + value);
}
// 0 = zero
// 1 = one

for (let key of myMap.keys()) {
  console.log(key);
}
// 0
// 1

for (let value of myMap.values()) {
  console.log(value);
}
// zero
// one

for (let [key, value] of myMap.entries()) {
  console.log(key + ' = ' + value);
}
// 0 = zero
// 1 = one
```

<br />

### 이터레이터는 언제 유용할까?

1. 객체에 `[Symbol.iterator]()` 메서드를 추가함으로써 이터러블하게 만들고 요소를 순회하면서 반복적인 작업을 할 수 있다.
2. 기존의 `[Symbol.iterator]()` 메서드를 override해서 원하는 동작을 반복하도록 만들 수 있다.

<br />

#### `[Symbol.iterator]()` 메서드 override 해보기

```js
const arr = [1, 2, 3];

console.log(arr[Symbol.iterator]); // values() { [native code] }

for (let element of arr) {
  console.log(element);
}
```

> 출력결과

```
1
2
3
```

Array 객체는 기본적으로 `[Symbol.iterator]()`를 가지고 있는 built-in 이터러블 객체이기 때문에 `for...of` 문을 사용해서 순회할 수 있다.

이러한 `arr` 배열 객체의 `[Symbol.iterator]()` 메서드를 override해서 다른 동작을 반복하게끔 만들 수 있다.

`[Symbol.iterator]()`메서드를 커스터마이징 할때는 **[iterator protocol](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols)을 따르는 함수**를 작성하기만 하면 된다.

> **iterator protocol**
>
> - `[Symbol.iterator]()` 메서드는 zero-argument 함수로, **인자 없이 호출**한다.
> - `[Symbol.iterator]()` 메서드는 `next()` 메서드가 들어있는 객체를 리턴한다.
> - `next()` 메서드 역시 **인자 없이 호출**되며, 객체를 리턴한다. 이 객체에는 `done`, `value` 두 개의 프로퍼티가 **반드시** 포함되어 있어야 한다.
> - `done` (boolean) : 다음에 출력할 값(next value)이 있으면 `false`, 없으면 `true`
> - `value` : 이터레이터에 의해 리턴되는 값

```js
const arr = [1, 2, 3];

arr[Symbol.iterator] = function () {
  let nextValue = 10;
  return {
    next: function () {
      nextValue++;
      return {
        value: nextValue,
        done: nextValue > 15 ? true : false,
      };
    },
  };
};

for (let element of arr) {
  console.log(element);
}
```

> 출력결과

```
11
12
13
14
15
```

이제 각각의 요소에 접근하는 기본 동작이 아니라 `nextValue`라는 변수에 할당된 초기값을 기준으로 `done`이 `true`가 될 때까지 1씩 증가시켜주는 동작을 반복한다.

`done`이 `true`가 되는 조건을 제어함으로써 내가 원하는 동작을 언제까지 수행할지도 결정할 수 있다.

<br />

### well-formed iterable

iterator 이면서 iterable인 객체를 well-formed iterable 이라고 한다.

> well-formed iterable이 아닌 경우

```js
const arr = [1, 2, 3, 4, 5];

arr[Symbol.iterator] = function () {
  let nextValue = 10;
  return {
    next: function () {
      nextValue++;
      return {
        value: nextValue,
        done: nextValue > 15 ? true : false,
      };
    },
  };
};

const iterator = arr[Symbol.iterator]();

iterator.next();
iterator.next();

for (let element of iterator) {
  console.log(element);
}
```

> 출력결과

```
Error: iterator is not iterable
```

`iterator`가 iterabl이 아니라는 에러 메시지와 함께 `for...of`로 순회를 할 수가 없다.

<br />

> well-formed iterable로 리팩토링

```js
const arr = [1, 2, 3, 4, 5];

arr[Symbol.iterator] = function () {
  let nextValue = 10;
  return {
    next: function () {
      nextValue++;
      return {
        value: nextValue,
        done: nextValue > 15 ? true : false,
      };
    },
    [Symbol.iterator]() {
      return this;
    },
  };
};

const iterator = arr[Symbol.iterator]();

console.log(iterator === iterator[Symbol.iterator]()); // true

iterator.next();
iterator.next();

for (let element of iterator) {
  console.log(element);
}
```

> 출력결과

```
true
13
14
15
```

web API에 구현된 iterable은 대부분 well-formed iterable이다.

- [well-formed 이터러블의 장점](https://underbleu.com/Functional-programming/well-formed/)

<br />

## 제너레이터 (generator)

<br />

---

Reference

- [Iteration protocols](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols)
- [Iterators and generators](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Iterators_and_Generators)
