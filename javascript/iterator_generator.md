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

`Map.prototype.keys()`, `Map.prototype.values()`, `Map.prototype.entries()` 같은 메서드들은 모두 새로운 이터레이터를 리턴한다. 따라서 `for...of` 메서드를 사용해서 순회할 수 있다.

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

**iterator 이면서 iterable인 객체**를 well-formed iterable 이라고 한다.

> well-formed iterable이 아닌 경우

```js
const iterable = {
  [Symbol.iterator]() {
    let i = 3;
    return {
      next() {
        return i === 0 ? { done: true } : { value: i--, done: false };
      },
      [Symbol.iterator]() {
        return this;
      },
    };
  },
};

for (let element of iterable) {
  console.log(element);
}
```

> 출력결과

```
3
2
1
```

이터러블 객체를 만들어서 `for...of`문으로 순회를 했더니 콘솔에 숫자들이 정상적으로 출력된다.  
하지만 이터레이터를 순회하려고 하면 다음과 같이 에러가 발생한다.

```js
const iterable = {
  [Symbol.iterator]() {
    let i = 3;
    return {
      next() {
        return i === 0 ? { done: true } : { value: i--, done: false };
      },
    };
  },
};

const anotherIterable = iterable[Symbol.iterator]();

anotherIterable.next();
anotherIterable.next();

for (let element of anotherIterable) {
  console.log(element);
}
```

> 출력결과

```
Error: iterator is not iterable --- Chrom devTools console
TypeError: iterator[Symbol.iterator] is not a function --- CodeSandbox
```

Chrome console에서는 `iterator`가 iterable이 아니라는 에러가 발생하고 codeSandbox에서는 보다 명확하게 `iterator[Symbol.iterator]`가 함수가 아니라는 에러가 발생한다.  
둘은 결국 같은 말인데, 현재 `iterator`에는 `[Symbol.iterator]()` 메서드가 존재하지 않아서 이터러블하지 않기 때문에 생긴 에러다.  
이터러블하지 않으니 당연히 `next()` 메서드를 호출할수도 없고 `for...of`로 순회를 할 수도 없다.

<br />

> well-formed iterable로 리팩토링

```js
const iterable = {
  [Symbol.iterator]() {
    let i = 3;
    return {
      next() {
        return i === 0 ? { done: true } : { value: i--, done: false };
      },
      [Symbol.iterator]() {
        return this;
      },
    };
  },
};

const anotherIterable = iterable[Symbol.iterator]();

console.log(anotherIterable === anotherIterable[Symbol.iterator]()); // true

anotherIterable.next();
anotherIterable.next();

for (let element of anotherIterable) {
  console.log(element);
}
```

> 출력결과

```
true
1
```

iterator(=`anotherIterable`)의 `[Symbol.iterator]()` 메서드가 자기자신을 리턴하게되면 변수 `i`의 값을 참조할 수 있는 상태를 유지할 수 있다.  
자기 자신의 상태를 계속해서 기억할 수 있는 well-formed iterable인 셈이다.

```js
let i = 3;

// anotherIterable
{
  next() {
    return i === 0
      ? { done: true }
      : { value: i--, done: false };
  },
  [Symbol.iterator]() {
    return this;
  },
}
```

`anotherIterable`은 well-formed iterable이기 때문에 `next()`를 두 번 호출한 이후의 `i`의 값을 알 수 있고 결과적으로 `for...of`로 순회했을때 `1`만 출력된 것이다.

<br />

web API에 구현된 iterable은 대부분 well-formed iterable이다.  
`document.querySelectorAll()`이 리턴하는 NodeList를 `for...of`로 순회할 수 있는 것 역시 NodeList가 well-formed iterable이기 때문에 가능한 것!

- [well-formed 이터러블의 장점](https://underbleu.com/Functional-programming/well-formed/)

<br />

## 제너레이터 (generator)

제너레이터함수는 이터레이터를 사용해서 자신의 실행을 제어하는 함수이다.

```js
function* generator {
  yield [expression];
  yield [expression];
  yield [expression];
}
// `*` : 애스터리스크
```

- 제너레이터를 호출하면 즉시 실행되지 않고 [제너레이터](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Generator)를 리턴한 뒤 대기한다. 제너레이터함수의 바디는 호출자가 해당 제너레이터의 `next()` 메서드를 호출함에따라 실행된다.
- 제너레이터함수는 `yield` 키워드가 등장하면 호출자에게 제어권을 넘긴다.
- 호출자 역시 `next(value)`와 같이 제너레이터함수에 어떠한 값을 넘길 수 있다.

<br />

```js
function* survey {
  const name = yield 'What is your name?';  // (a)
  const color = yield 'What is your favorite color?'; // (b)
  return `${name}'s favorite color is ${color}!`; // (c)
}

const it = survey(); // (1)

it.next(); // (2) { value: 'What is your name?', done, false }
it.next('Kyle'); // (3) { value: 'What is your favorite color?', done: false }
it.next('yellow'); // (4) { value: 'Kyle's favorite color is yellow!', done: true }

for (const info of it) {
  console.log(info); // (5)
}
```

> 출력결과

```
What is your name?
What is your favorite color?
```

1. 제너레이터함수는 제너레이터를 리턴하고 일시 정지한 상태로 기다린다.
2. 호출자는 `next()`를 호출하면서 제너레이터함수에 `undefined`를 넘긴다.  
   그러면 제너레이터함수는 `(a)`행에 있는 `yield`표현식이 리턴하는 값 표현식(값)인 'What is your name?'을 호출자에게 넘기고 일시정지 한다.
   > - yield 표현식의 값은 다음에 호출될 `next()`의 매개변수이다.
   > - 제너레이터의 `next()`가 리턴하는 객체는 `value`, `done` 두 개의 프로퍼티를 가진다.
   >   - `value` : `yield` 표현식이 반환'할' 값. `next()`의 인자.
   >   - `done` (boolean) : 제너레이터함수 바디에 있는 모든 `yield` 표현식의 실행 여부를 표시하는 값
3. 호출자가 'Kyle'이라는 값을 제너레이터함수에 넘기면 해당 값은 `yield` 표현식의 반환값이 되고 `name` 변수가 이 값을 가리키게 된다. --- **`(a)`행 resolved**  
   제너레이터함수는 다시 호출자에게 'What is your favorite color?'를 넘긴 뒤 일시정지 한다.
4. 호출자가 'yellow'라는 값을 제너레이터함수에 넘긴다. 해당 값은 `yield` 표현식의 반환값이 되고 `color` 변수는 이 값을 가리키게 된다. --- **`(b)`행 resolved**  
   제너레이터함수는 그 다음 `(c)`행에서 `return`문을 만난다. 'Kyle's favorite color is yellow!'를 리턴하고 멈춘다. --- **`(c)행 resolved**
   > - `return`문을 사용하면 위치와 관계 없이 `done`은 `true`가 되고 `value`는 `return`문이 리턴하는 값이 된다.
   > - 반대로 `yield`의 경우 위치와 관계 없이 제너레이터함수의 실행을 종료시키지 않는다. (설령 함수 바디의 맨 마지막에 위치하더라도)
5. `for...of`문으로 제너레이터를 순회하면 `return`문이 리턴하는 값은 출력되지 않는다. `done`이 `true`이면 `value` 프로퍼티가 무시되기 때문!

<br />

---

<br />

- 제너레이터함수가 리턴하는 제너레이터는 iterator protocol과 itarable protocol을 따른다.

  ```js
  /* 제너레이터는 iterator protocol을 따른다. */

  typeof generator.next; // "function"
  generator.next(); // {done: boolean, value: any}
  ```

  ```js
  /* 제너레이터는 itarable protocol을 따른다. */

  typeof generator[Symbol.iterator]; // "function"
  const iterator = generator[Symbol.iterator]();
  typeof iterator.next(); // {done: boolean, value: any}
  ```

- 제너레이터는 이터러블이자 이터레이터이다. 즉, **well-formed iterable**이다.
  ```js
  generator === generator[Symbol.iterator](); // true
  ```
- 제너레이터함수로 어떠한 값/상태를 이터러블하게 만들고 순회/제어 할 수 있다는 점에서 유의미하다.

<br />

---

Reference

- [Iteration protocols](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols)
- [Iterators and generators](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Iterators_and_Generators)
- [function\*](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function*)
- 러닝 자바스크립트 (한빛미디어)
- [Redux-Saga: 제너레이터와 이펙트](https://meetup.toast.com/posts/140)
