# Symbol

## Intro

- ES6부터 도입된 **원시값**이다. 즉, immutable하다.
- 모든 객체가 유일한 값인 것 처럼 모든 symbol 역시 유니크한 값이다.
  항상 유일하다는 점을 제외하면 문자열이나 숫자처럼 원시값의 특징을 모두 가지고 있다.
- symbol은 `Symbol()` 생성자를 호출해서 만들 수 있다.

  - `new` 키워드는 사용하지 않는다.
  - `( )` 안에 Symbol에 대한 간단한 description을 추가할 수 있다.

  ```js
  const PEACH = Symbol('One of my favorite fuites!');
  const ORANGE = Symbol('One of my favorite fuites!');

  PEACH == ORANGE; // false
  PEACH === ORANGE; // false
  ```

- symbol의 타입은 'symbol'이다.
  ```js
  const sym1 = Symbol();
  console.log(typeof sym1); // 'symbol'
  ```

<br />

## Symbol은 언제 쓰는 걸까?

symbol의 use-case는 다양하지만 가장 주된 사용처는 *객체 프로퍼티의 식별자(identifier)*로 사용하는 것이다.  
symbol은 항상 유니크한 값이기 때문에 객체의 키값으로 사용했을때 프로퍼티 충돌에 대한 걱정 없이 항상 새로운 프로퍼티를 생성할 수 있기 때문!

<br />

## Symbol 사용법

### 1. symbol 프로퍼티 get, set, delete 하기

```js
const sym1 = Symbol('profile');

const person = {
  name: 'Kyle',
  [sym1]: 22,
};

console.log(person[sym1]); // 22

const sym2 = Symbol('hello');
person[sym2] = 'world';

console.log(person.sym2); // undefined
console.log(person[sym2]); // 'world'

delete person[sym2];
console.log(person[sym2]); // undefined
```

객체에 symbol을 key값으로 가지는 프로퍼티(symbol-keyed property)를 추가하거나 참조할때는 대괄호를 사용하면 된다.

> ❗ `object.name`처럼 dot notation은 사용 불가!

일반적인 프로퍼티와 마찬가지로 `delete` 키워드를 사용해서 프로퍼티를 삭제할 수 도 있다.

<br />

### 2. symbol을 key값으로 가지는 프로퍼티(symbol-keyed property) 조회하기

```js
const sym1 = Symbol('profile');

cosnt person = {
  name: 'Kyle',
  [sym1]: 22
};

console.log(person); // {name: 'kyle'}
console.log(person[sym1]); // 22
```

symbol-keyed property는 객체를 조회했을때는 숨겨져 있다가 명시적으로 참조할때만 접근이 가능하다. symbol의 private한 특성은 **어플리케이션의 [meta data](https://ko.wikipedia.org/wiki/%EB%A9%94%ED%83%80%EB%8D%B0%EC%9D%B4%ED%84%B0)를 저장할때 유용하다**.

---

symbol은 기본적으로 충돌을 피하도록 디자인되었기 때문에 symbol은 iterable 하지 않다.  
그래서 일반적으로 객체의 키값을 가져올때 사용하는 `for...in` loop나 `Object.keys(obj)`, `Object.getOwnPropertyNames(obj)`를 사용하면 symbol key를 가져올 수 없다.

대신 아래의 메서드를 사용하면 된다.

- [`Object.getOwnPropertySymbols(obj)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertySymbols)
- [`Reflect.ownKeys(obj)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Reflect/ownKeys)

<br />

### 3. symbol 만들기

symbol을 만드는 3가지 방법

- [`Symbol()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol/Symbol)  
  호출할때마다 유니크한 key값을 가지는 symbol을 생성.
- [`Symbol.for(key)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol/for)  
  이미 존재하는 symbol들(symbol registry) 중에 인자로 전달한 'key'값을 가지는 symbol이 있으면 true를 반환하고, 없으면 새로운 symbol을 생성해서 symbol registry에 추가한다.

  ```js
  const sym1 = Symbol.for('age');
  const sym2 = Symbol.for('age');

  console.log(sym1 == sym2); // true
  ```

  `sym1`과 `sym2`는 'age'라는 동일한 key값을 가지는 *shared symbols*이다.

- [`Symbol.iterator`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol/iterator)

<br />

### 4. Unique symbols vs Shared symbols

기본적으로 모든 symbol은 유니크한 값이지만 위에서 잠깐 등장했던 `Symbol.for(key)`을 사용해서 같은 key값을 갖는 shared symbol을 만들 수도 있다.

```js
const sym1 = Symbol.for('age'); // (1)

const person = {
  name: 'Kyle',
};

function makeAge(person) {
  const ageSymbol = Symbol.for('age'); // (2)
  person[ageSymbol] = 27;
}

makeAge(person); // (3)

console.log(person[sym1]); // 27 (4)
```

1. `sym1`은 'age'라는 key값을 가지며 전역에서 참조할 수 있는 shared symbol이다.
2. `makeAge` 함수 스코프 내에서 생성한 `ageSymbol`도 `sym1`과 마찬가지로 'age'라는 key값을 가지는 shared symbols이다.
3. `makeAge`를 호출해서 person 객체에 `ageSymbol` 프로퍼티를 추가했다.
4. 전역에서 `person[sym1]`에 접근해서 정상적으로 프로퍼티의 값을 출력했다. 이는 `sym1`과 `ageSymbo`가 동일한 key값을 가지는 shared symbols이기 때문에 가능하다.

만약 `sym1`과 `ageSymbo`를 `Symbol()`로 생성한다면 각각 유니크한 symbol이기 때문에 (4)의 콘솔 출력 결과는 `undefined`가 될 것이다.

<br />

---

Reference

- [ES6 In Depth: Symbols](https://hacks.mozilla.org/2015/06/es6-in-depth-symbols/)
- [Symbol](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol)
