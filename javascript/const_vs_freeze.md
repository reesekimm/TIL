# const vs Object.freeze()

`const`와 `Object.freeze()` 모두 상태를 immutable하게 만든다는 점에서 비슷해 보이지만,  
각자 immutable하게 만드는 대상이 다르다.

const는 하나의 변수가 하나의 값을 가리키도록 **immutable한 binding**을 만들고,  
Object.freeze()는 인자로 받는 객체의 **값(프로퍼티)을 immutable하게** 만든다.

<br />

조금 더 자세히 살펴보면,

## [const](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const)

- 숫자, 문자열 같은 원시값에도 사용 가능하다.
- **const가 가리키는 객체의 프로퍼티를 자유롭게 변경할 수 있다.**
- **const 변수가 가리키는 값을 재할당 할 수 없다.**

## [Object.freeze()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/freeze)

- 원시값에 사용할 수 없다. 즉, 객체(object, array)만 인자로 받는다.
- **객체를 재할당 할 수 있다.**
- **객체의 프로퍼티에 대해 CRUD가 불가능하다.** ─ enumerability, configurability, writability와 같은 프로퍼티 속성 포함
- 객체의 프로토타입 역시 freeze되어 수정할 수 없다.
- **하지만 nested object는 수정이 가능하다.** (shallow freeze이기 때문!)
- 인자로 받은 객체를 freeze시킨 후 리턴해준다.

<br />

## Examples

```javascript
var ob1 = {
  foo: 1,
  bar: {
    value: 2
  }
};

Object.freeze(ob1);
Object.isFrozen(ob1); // true

const ob2 = {
  foo: 1,
  bar: {
    value: 2
  }
};

ob1.foo = 4; // (frozen) 수정 불가
ob2.foo = 4; // (const) 수정가능

ob1.bar.value = 4; // (frozen) nested된 값은 수정 가능
ob2.bar.value = 4; // (const) 수정 가능

ob1.bar = 4; // (frozen) bar는 obj1의 key이기 때문에 수정 불가
ob2.bar = 4; // (const) 수정가능

ob1 = {}; // (frozen) 재할당 가능
ob2 = {}; // (const) 재할당 불가

Object.isFrozen(ob1); // false --- freeze된 객체를 재할당 할 경우 기존 freeze 처리는 무효화된다.
```

<br />

---

**Reference**

- [MDN - const](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const)
- [MDN - Object.freeze()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/freeze)
- [Stack overflow - Object.freeze() vs const](https://stackoverflow.com/questions/33124058/object-freeze-vs-const)
- [생활코딩 - JavaScript immutability - 7. const vs object freeze](https://www.youtube.com/watch?v=ol239ZUGwHg)
