# [Node JS - 04] Node.js 콘솔 환경에서 입력값 전달하기

> [생활코딩 Node.js](https://www.youtube.com/playlist?list=PLuHgQVnccGMA9QQX5wqj6ThK7t2tsGxjm)을 토대로 학습한 내용을 정리한다.

> **학습 내용**  
> 콘솔 환경에서 앱을 실행할 때 입력 값을 전달하는 방법을 알아본다.

<br />

## `process.argv`

#### process.argv

- Node.js.의 [process 객체](https://nodejs.org/docs/latest/api/process.html#process_process_argv)는 프로그램을 실행 했을떄 만들어지는 프로세스 정보를 다루는 객체이다. 굉장히 많은 메서드들을 가지고 있는데, 그중에서 **`process.argv`는 프로세스를 실행 할때 전달되는 인자에 대한 정보를 알려준다.**

- Return (Array)

  1. Node.js 런타임의 절대경로
  2. 실행시킨 파일(어플리케이션)의 절대경로
  3. 전달된 인자(들)

- 참고로 argv는 argument Vector의 약자로 가변적인 갯수의 문자열을 의미한다.

<br />

#### Example 1

> sample.js

```javascript
console.log(process.argv);
```

> 콘솔 입력

```
$ node sample.js hello js 123 [0, 1] {front:"end"} true false
```

> 출력결과

```
[ 'C:\\Program Files\\nodejs\\node.exe',
  'c:\\Users\\nanhyun.kim\\Desktop\\nodejs\\sample.js',
  'hello',
  'js',
  '123',
  '[0,',
  '1]',
  '{front:end}',
  'true',
  'false' ]
```

**알 수 있는 점**

1. index 0 : Node.js 런타임의 절대경로  
   index 1 : 실행시킨 파일의 절대경로  
   index 2~ : 콘솔의 command line에 입력했던 인자들이 차례대로 출력된다.
2. 배열안의 요소들은 모두 string이다.
3. 콘솔에 `[0, 1]`을 입력했을때 `[0,`과 `1]`이 출력된 것을 보아 띄어쓰기 1칸을 기준으로 인자를 구분한다.

그런데 만약 함수를 전달한다면?

```
node sample.js function test() {console.log(1)}
```

```
[ 'C:\\Program Files\\nodejs\\node.exe',
  'c:\\Users\\nanhyun.kim\\Desktop\\nodejs\\sample.js',
  'function',
  'test()',
  '{console.log(1)}' ]
```

함수 역시 string으로 변환되어 리턴된다.

<br />

#### Example 2

그렇다면 `process.argv`를 언제 쓰는걸까?  
우리는 **`process.argv`를 통해 프로그램에 전달된 인자 정보에 접근하고 이 인자를 제어함으로써 프로그램이 내가 원하는 동작(output)을 하게 만들 수 있다.**

> sample.js

```javascript
var args = process.argv;

console.log(args[2]);
console.log("A");
console.log("B");

if (args[2] === "1") {
  console.log("C1");
} else {
  console.log("C2");
}

console.log("D");
```

> 콘솔 입력

```
node sample.js 1
```

> 출력 결과

```
'A'
'B'
'C1'
'D'
```

<br />

---

#### 참고자료

- [How do I pass command line arguments to a Node.js program?](https://stackoverflow.com/questions/4351521/how-do-i-pass-command-line-arguments-to-a-node-js-program)
- [[node.js] 프로세스 객체 간단하게 살펴보기](https://kgh940525.tistory.com/entry/nodejs%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4-%EA%B0%9D%EC%B2%B4-%EA%B0%84%EB%8B%A8%ED%95%98%EA%B2%8C-%EC%82%B4%ED%8E%B4%EB%B3%B4%EA%B8%B0)
- [[Node] 프로세스 객체 process](https://zzdd1558.tistory.com/168)
- [Node.js 기본 - 전역객체 (console, process, exports)](https://reysion.tistory.com/42)
