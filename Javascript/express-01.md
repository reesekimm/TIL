# Express 인트로 및 미들웨어 이해하기

## [Express.js](http://expressjs.com/)란?

<img src="https://miro.medium.com/max/365/1*d2zLEjERsrs1Rzk_95QU9A.png" alt="express" />

Node.js의 핵심 모듈인 http와 [Connect](https://github.com/senchalabs/connect#readme) 컴포넌트를 기반으로 하는 웹 프레임워크이다.

아래 나열한 서버 제작시 겪게되는 불편함들을 해결해주고 동시에 웹 앱에 MVC 형태의 구조를 제공해준다.

- HTTP 요청 본문 파싱
- 쿠키 파싱
- 세션 관리
- URL 경로와 HTTP 요청 메서드를 기반으로 한 복잡한 if 조건을 통해 라우팅을 구성
- 데이터 타입을 토대로 한 적절한 응답 헤더 결정

<br />

## Express 설치하기

- 방법 1: 개별설치

  1. package.json 생성
  2. `npm install express` 입력
  3. 필요에 따라 패키지 추가 설치

  <br />

- 방법 2: Express-generator 사용
  - package.json 및 기본 폴더 구조를 자동으로 만들어줌
  1. `npm install express-generator`
  2. 프로젝트 폴더에서 `express <프로젝트 이름>` 입력하여 익스프레스 프로젝트 생성
  3. `cd <프로젝트 이름> && npm install` 입력하여 새롭게 생성된 <프로젝트 이름> 폴더에 npm 설치
  4. `npm start`로 익스프레스 실행

<br />

## Middleware란?

Express의 작동방식에 따라 서버로 들어오는 각 요청은 정의된 미들웨어와 라우팅에 따라 맨 위에서 시작해 맨 아래까지 처리된다.

> Structure of Express
> <img src="https://devopedia.org/images/article/157/3224.1551338491.png" alt="middleware" width="500"/>

1. 쿠키 정보를 파싱하고, 파싱이 완료되면 다음 단계로 이동한다.
2. URL로부터 매개변수를 파싱하고, 파싱이 완료되면 다음 단계로 이동한다.
3. 사용자가 인증되면(쿠키/세션) 매개변수의 값을 토대로 데이터베이스에서 정보를 가져와 일치하는 것이 있으면 다음 단계로 이동한다.
4. 데이터를 표시하고 응답을 마친다.

즉, 미들웨어는 요청과 응답의 중간(middle)에 위치하여 요청/응답을 조작하여 기능을 추가하거나 나쁜 요청을 걸러내는 역할을 하는 함수라고 할 수 있다.

> app.js

```js
import express from "express";
import cookieParser from "cookie-parser";
import bodyParser from "body-parser";

// express 패키지 호출
const app = express();

...

// middlewares
app.use(logger("dev"));
app.use(cookieParser());
app.use(bodyParser.json());

// routing
app.use("/", globalRouter);  // router
app.use("/user", userRouter);

export default app;
```

- 미들웨어는 `app.use()`메서드의 인자로 전달하여 app에 장착한다.
- 가장 위에 있는 `logger("dev")`부터 시작해서 순차적으로 실행된 후 라우터에서 클라이언트로 응답을 보낸다.
- 라우터나 에러 핸들링 함수 역시 middleware의 한 종류이기 때문에 `app.use()`로 app에 연결해준다.

<br />

## Custom Middleware 만들기

> app.js

```js
...
app.use(function(req, res, next) {
    console.log("I'm a middleware!");
    next();
});

app.use(logger("dev"));
app.use(cookieParser());
app.use(bodyParser.json());
...
```

`http://localhost:3000`에 접속 시 콘솔 출력 결과

> console

```
I'm a middleware!
```

서버가 받은 요청에 의해 미들웨어가 실행되고 라우터까지 전달된다.
주의할 점은 반드시 미들웨어 안에서 `next()`를 호출해야 다음 미들웨어로 넘어간다는것.  
만약 `next()`가 없다면 logger나 cookieParser 등의 미들웨어들이 작동하지 않고 요청의 흐름이 끊어지면서 서버가 응답을 할 수 없게 된다. (브라우저에서는 응답을 기다리고 있기 때문에 로딩 표시가 뜬다.) 그렇기 때문에 `next()`는 미들웨어 및 요청의 흐름을 제어하는 핵심적인 함수라고 할 수 있다.

<br />

## 자주 쓰이는 Middleware들

### 1. [morgan](https://www.npmjs.com/package/morgan)

요청에 대한 정보를 콘솔에 기록해주는 미들웨어

> app.js

```js
...
import morgan from "morgan";
...
app.use(morgan("dev"));
...
```

함수의 인자로 `dev` 대신 `short`, `common`, `combined` 등을 줘서 다른 log를 얻을 수 있다. `dev`인 경우 콘솔에 `GET / 200 51.265 ms - 1539` 같은 log가 찍히는데 순서대로 HTTP요청 메서드(GET) 주소(/) HTTP 상태코드(200) 응답속도(51.265 ms) - 응답바이트(1539)를 뜻한다.

<br />

### 2. [body-parser](https://www.npmjs.com/package/body-parser)

요청의 본문(body)을 해석해주는 미들웨어

- 보통 form 데이터나 AJAX 요청의 데이터를 처리한다.

> app.js

```js
...
import bodyParser from "body-parser";
...
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));
...
```

<br />

### 3. [cookie-parser](https://www.npmjs.com/package/cookie-parser)

요청의 쿠키를 해석해주는 미들웨어

> app.js

```js
...
import cookieParser from "cookie-parser";
...
app.use(cookieParser());
...
```

`cookie-parser`에 의해 해석된 쿠키들은 `req.cookie` 객체에 들어간다.

---

**참조**

- Node.js 교과서 (길벗)
- [Express.js란 무엇인가?](https://wikibook.co.kr/article/what-is-expressjs/)
- [Node.JS 강좌 09편: Express 프레임워크 사용해보기](https://velopert.com/294)
