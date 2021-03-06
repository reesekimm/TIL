# 자바스크립트 객체 복사

## 1. 얕은 복사 (Shallow Clone) & 깊은 복사 (Deep Clone)

### 얕은 복사

- 원본 객체의 프로퍼티를 전부 복사한 새로운 객체를 생성한 후 새로운 메모리에 할당해준다.
- 원본의 프로퍼티가 객체를 값으로 가지고 있을 경우 값이 아닌 참조(reference)를 복사한다.  
  -> 원본과 사본이 동일한 참조를 공유하기 때문에 원본 수정시 사본이 함께 수정된다.
- 원본의 Property Descriptors는 복사되지 않는다.
- 원본의 프로토타입 체인 또는 `enumarable: false`인 프로퍼티는 복사되지 않는다.

### 깊은 복사

: 원본의 프로퍼티와 그 프로퍼티의 참조를 모두 복사하여 새로운 메모리 공간에 할당한다.

![image](https://user-images.githubusercontent.com/42695954/63681960-d7bdbe80-c831-11e9-8a85-99b1d35c705c.png)

<br />

## 2. 얕은 복사 방법

1. **[Object.assign()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)**

```javascript
var o1 = { name: "kim", score: [1, 2] };
var o2 = Object.assign({}, o1);

o2.score.push(3);
console.log(o1.score); // [1, 2, 3]
```

`Object.assign()`은 첫번째 인자로 받은 `target`객체에 두번째부터 인자로 받은 `source`객체들을 merge 하여 `target`객체를 리턴해준다.  
얕은 복사이기 때문에 `o2.score.push(3);`와 같이 사본의 참조값을 수정하면 `o1.score` 원본이 따라서 수정된다.
![2019-08-26 20;35;43](https://user-images.githubusercontent.com/42695954/63688354-1d828300-c842-11e9-92e9-495d9b1794a6.PNG)

원본이 수정되는 것을 막으려면 `concat()`, `slice()`, `Array.from()`등을 사용해서 참조 경로가 아닌 값을 복사해주는 작업을 추가로 해주어야 한다. (참고로 Object.assign()을 사용할 경우 배열이 가진 특성이 사라진다.)
![2019-08-26 20;35;06](https://user-images.githubusercontent.com/42695954/63688369-2c693580-c842-11e9-81ab-bddf73d76f54.PNG)

<br />

2. **[Spread Operator](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Spread_syntax)**

```javascript
const person1 = { name: "David Walsh", age: 33 };
const person2 = { name: "David Walsh Jr.", role: "kid" };

const merged = { ...person1, ...person2 };

/*
Object {
  "name": "David Walsh Jr.",
  "age": 33,
  "role": "kid",
}
*/
```

중복되는 key에 대해서는 그 key를 가지고 있는 가장 마지막 객체의 값이 덮어쓰여진다. (In the case of a key collision, the right-most (last) object's value wins out)

<br />

**기타**

- [\_.clone()](https://lodash.com/docs/4.17.15#clone)
- [jQuery.extend( target [, object1 ] [, objectN ] )](https://api.jquery.com/jquery.extend/)

<br />

## 3. 깊은 복사 방법

1. **JSON 객체의 메소드 사용**

```javascript
var deepCopy = JSON.parse(JSON.stringify(obj));
```

문자열로 변환했다가 다시 객체로 변환하기 때문에 이전 객체에 대한 참조가 없어지지만, JSON 메소드 자체가 성능면에서 다른 방법에 비해 굉장히 느리기 때문에 주의해야한다.

<br />

2. **재귀**

```javascript
function deepCopy(source) {
  var target = {};
  for (let i in source) {
    if (source[i] != null && typeof source[i] === "object") {
      target[i] = deepCopy(source[i]); // resursion
    } else {
      target[i] = source[i];
    }
  }
  return target;
}
```

재귀적으로 객체 트리를 따라 말단 노드까지 모두 복사해주는 방법

<br />

3. **[Immutable.js](https://immutable-js.github.io/immutable-js/)**

```javascript
import { Map } from "immutable";

const map = Map({ a: 1 });
const newMap = map;
newMap.set("a", 2);

console.log(map.get("a"));
console.log(newMap.get("a"));
```

페이스북에서 만든 오픈소스 라이브러리로, Array, Map 모두 Immutable하게 쓸 수 있게 해준다. 객체의 내부 값을 변경해도 원본 객체의 값은 변화하지 않고 새로운 객체를 리턴해준다.

<br />

**기타**

- [\_.cloneDeep()](https://lodash.com/docs/4.17.15#cloneDeep)
- [jQuery.extend( [deep ], target, object1 [, objectN ] )](https://api.jquery.com/jquery.extend/)

<br />

---

**Reference**

- [Understanding Deep and Shallow Copy in Javascript](https://we-are.bookmyshow.com/understanding-deep-and-shallow-copy-in-javascript-13438bad941c)
- [Javascript:Shallow and Deep Copy](https://mygumi.tistory.com/322)
- [자바스크립트 객체 복사하기](https://velog.io/@ddalpange/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EA%B0%9D%EC%B2%B4-%EB%B3%B5%EC%82%AC%ED%95%98%EA%B8%B0)
- [생활코딩 - JavaScript immutability](https://www.youtube.com/watch?v=HN1-5v81Fzc&list=PLuHgQVnccGMBxNK38TqfBWk-QpEI7UkY8&index=7)
- [Stack Overflow](https://stackoverflow.com/questions/122102/what-is-the-most-efficient-way-to-deep-clone-an-object-in-javascript)
- [Merge Object Properties with the Spread Operator](https://davidwalsh.name/merge-objects)
- [자바스크립트 객체 복제 방법](https://www.daleseo.com/js-objects-clone/)
