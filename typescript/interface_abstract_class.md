# 인터페이스 vs 추상 클래스

## 인터페이스

인터페이스란 **객체가 반드시 구현해야할 프로퍼티/메서드의 선언부만 모아놓은 것**이다.  
인터페이스에 명시된 프로퍼티/메서드의 구현은 해당 인터페이스를 실행하는 `변수`, `함수`, `클래스`에서 하며, 인터페이스에 선언된 프로퍼티/메서드를 구현하지 않을 경우 에러가 발생한다.

> 변수와 인터페이스

```ts
interface Todo {
  id: number;
  name: string;
  completed: boolean;
}

const washDishes: Todo = {
  id: 3,
  name: 'Do the dishes',
  completed: false,
};
```

<br />

> 함수와 인터페이스

인터페이스에 함수 파라미터와 리턴값에 대한 타입을 명시해준다.

```ts
interface SearchFunc {
  (source: string, subString: string): boolean;
}

let mySearch: SearchFunc = (src: string, sub: string): boolean => {
  let result = src.search(sub);
  return result > -1;
};
```

- 함수의 파라미터명이 인터페이스에 명시된것과 같지 않아도 된다. (source → src, subString → sub)

<br />

> 클래스와 인터페이스

```ts
interface ClockInterface {
  currentTime: Date;
  setTime(d: Date): void;
}

class Clock implements ClockInterface {
  currentTime: Date = new Date();
  setTime(d: Date) {
    this.currentTime = d;
  }
  constructor(h: number, m: number) {}
}
```

<br />

### 덕 타이핑 (Duck Typing)

타입스크립트는 **인터페이스에 선언된 프로퍼티/메서드를 가지고 있기만 하면 해당 인터페이스를 구현한 것으로 인정**한다. 이것을 덕 타이핑 또는 구조적 타이핑(Structural Typing)이라고 한다.

> 예시 1

```ts
interface Square {
  width: number;
  color: string;
}

const createSquare = (config: Square): void => {
  console.log({ color: config.color, area: config.width });
};

const mySquare = {
  width: 20,
  color: 'red',
  border: '2px',
};

createSquare(mySquare); // { "color": "red", "area": 20 }
```

`mySquare` 객체는 Square 인터페이스에 명시된 모든 프로퍼티를 가지고 있기 때문에 해당 인터페이스를 구현한 것으로 인정되어 `createSquare` 함수에 인자로 전달했을 때 컴파일 에러가 발생하지 않는다.

> 예시 2

```ts
interface Quackable {
  quack(): void;
}

class MallardDuck implements Quackable {
  quack() {
    console.log('Quack!');
  }
}

class Person {
  quack() {
    console.log('q~uack!');
  }
}

function makeNoise(animal: Quackable): void {
  animal.quack();
}

makeNoise(new MallardDuck()); // Quack!
makeNoise(new Person()); // q~uack!
```

`Person` 클래스는 `Quackable`인터페이스를 구현하지 않지만 `Quackable`인터페이스 명시된 **quack** 메서드를 가지고 있기 때문에 인터페이스를 구현한 것으로 인정된다.  
컴파일 에러 없이 makeNoise 함수의 인자로 전달될 수 있다.

<br />

### 인터페이스는 언제, 왜 사용하는 걸까?

1. 컴파일 타임에 객체의 타입을 체크하기 위해 사용한다. → 타입 안정성을 높인다.
2. 객체의 프로퍼티/메서드의 구현을 강제함으로써 객체의 구조를 고정시킨다. → 객체의 유지보수/확장이 쉬워진다.
3. 서로 다른 객체들이 동일한 기능을 수행해야할 때 유용하다. (Duck Typing) → 객체간의 결합도를 낮춘다.

<br />

### 인터페이스의 특징

1. 인터페이스는 런타임에 존재하지 않는다.
   > 컴파일 타임에 타입 안정성을 확보 하기 위한 용도로 쓰여지고 컴파일 이후에는 제거되기 때문이다.  
   > 런타임에 존재하지 않기 때문에 `typeof`를 이용해서 인터페이스의 타입을 알아낼 수 없다.
2. [인터페이스끼리 상속 및 다중상속이 가능하다.](https://www.typescriptlang.org/docs/handbook/interfaces.html#extending-interfaces)
3. [인터페이스는 클래스도 상속받을 수 있다.](https://www.typescriptlang.org/docs/handbook/interfaces.html#interfaces-extending-classes)

<br />

## 추상 클래스

추상 클래스는 하나 이상의 추상 프로퍼티/메서드를 포함하면서 구현 메서드도 포함할 수 있는 클래스이다.

> - 추상 메서드: 선언부만 있는 메서드
> - 구현 메서드: 실제 구현 내용을 포함하는 메서드

인터페이스처럼 자식 클래스에게 구현을 강제하고 싶은 기능을 부모 클래스(추상 클래스)의 메서드로 만들어준다. 이때 **자식 클래스는 추상 클래스를 상속받으면서 추상 클래스에 명시된 추상 프로퍼티/메서드를 반드시 구현해야 한다**.

```ts
abstract class Bird {
  abstract name: string;
  abstract habitat: string;
  abstract flySound(sound: string): void;
  fly(): void {
    this.flySound('파닥파닥');
  }
}

class Sparrow extends Bird {
  constructor(public name: string, public habitat: string) {
    super();
  }

  // method overriding
  flySound(sound: string) {
    console.log(`${this.name}가 ${sound}거리며 날아갑니다.`);
  }
}

const wildSparrow = new Sparrow('참새', '우리 동네');
wildSparrow.flySound('짹짹'); // 참새가 짹짹거리며 날아갑니다.
```

추상 클래스는 `new` 키워드를 사용하여 직접 인스턴스를 생성하는게 불가능하다.

```ts
abstract class Department {
  constructor(public name: string) {}

  printName(): void {
    console.log('Department name: ' + this.name);
  }

  abstract printMeeting(): void;
}

class AccountingDepartment extends Department {
  constructor() {
    super('Accounting and Auditing');
  }

  printMeeting(): void {
    console.log('The Accounting Department meets each Monday at 10am.');
  }

  generateReports(): void {
    console.log('Generating accounting reports...');
  }
}

let department: Department;
department = new Department(); // Error: 추상 클래스의 인스턴스는 생성할 수 없음!

department = new AccountingDepartment();

department.printName();
department.printMeeting();
department.generateReports(); // Error: 'generateReports' 프로퍼티는 type 'Department'에 존재하지 않음!
```

<br />

### 추상 클래스는 언제, 왜 사용하는 걸까?

상속을 구현할때 사용한다.

- 객체간에 공통적으로 반복사용되는 기능이 있을 경우 공통적으로 사용되는 코드를 모두 추상 클래스에 모아둠으로써 중복을 제거할 수 있다.
- 자식 클래스는 부모 클래스의 모든 기능을 사용할 수 있다. → 코드의 재사용성
- 자식 클래스는 부모 클래스를 확장하여 기능을 추가할 수 있다. → 코드의 확장성

> **단점**  
> 자식 클래스는 반드시 부모 클래스의 모든 추상 프로퍼티/메서드를 구현해야 제대로 동작할 수 있고, 부모(추상)클래스 역시 자식 클래스 없이 구현이 불가능하기 때문에 **객체간의 결합도가 매우 높다**.

<br />

---

### Reference

- [What is the difference between interface and abstract class in Typescript?](https://stackoverflow.com/a/50115567)
- [Interfaces](https://www.typescriptlang.org/docs/handbook/interfaces.html)
- [인터페이스](https://poiemaweb.com/typescript-interface)
- 타입스크립트 퀵스타트 (루비페이퍼)
- [TypeScript: 인터페이스(Interface)](https://hyunseob.github.io/2016/10/17/typescript-interface/)
