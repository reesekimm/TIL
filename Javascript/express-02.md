# Express Router 인스턴스로 라우팅하기

> app.js

```js
...
import globalRouter from "./routers/globalRouter";
import userRouter from "./routers/userRouter";
...
// 특정 주소에 요청이 들어왔을 때 라우터 연결
app.use("/", globalRouter);
app.use("/user", userRouter);
...
```

- 라우터도 일종의 미들웨어이다.
- 첫번째 인자로 주소를 받아서 특정 주소에 해당하는 요청이 왔을 때만 미들웨어가 동작하게 할 수 있다.
- use 메서드는 모든 HTTP 메서드에 대해 요청 주소만 일치하면 실행되지만 get, post, put, patch, delete 같은 메서드는 주소뿐만 아니라 HTTP 메서드까지 일치하는 요청일 때만 실행된다.

  ```js
  app.use("/", (req, res, next) => {
    console.log("/ 주소의 요청일 때만 실행됩니다. HTTP 메서드는   없습니다.");
    next();
  });
  app.get("/", (req, res, next) => {
    console.log("GET 메서드 / 주소의 요청일 때만 실행됩니다.");
    next();
  });
  ```

<br />

라우터 파일을 살펴보면

> ./routers/globalRouter.js

```js
import express from "express";

// Router 인스턴스로 route handler (router) 생성
const globalRouter = express.Router();

// home route 정의
globalRouter.use("/", (req, res) => res.send("Home"));
// join route 정의
globalRouter.post("/join", (req, res) => res.send("Join"));
// login route 정의
globalRouter.get("/login", (req, res) => res.send("Log In"));

export default globalRouter;
```

- 라우터에도 app처럼 use, get, post, put, patch, delete 같은 메서드를 붙일 수 있다.
- 라우터는 반드시 요청에 대한 응답을 보내거나 에러 핸들러로 요청을 넘겨야 한다. 응답을 보내지 않으면 브라우저는 계속 응답을 기다리게 된다. 응답을 보낼 때는 res객체에 들어있는 메서드를 사용한다.

  ```
  res.send(버퍼 or 문자열 or HTML or JSON);
  res.sendFile(파일 경로);
  res.json(JSON 데이터);
  res.redirect(주소);
  res.render("템플릿 파일 경로", {변수});
  ```

  > [Response Method 더보기](http://expressjs.com/en/guide/routing.html)

<br />

## 라우터에 미들웨어 장착하기

라우터도 app.use 처럼 라우터 하나에 여러 개의 미들웨어를 장착할 수 있다. 예를들면 실제 라우터 로직이 실행되는 미들웨어 전에 로그인 여부 또는 관리자 여부를 체크하는 미들웨어를 중간에 넣어두는 것이 가능하다.

```js
router.get("/", middleware1, middleware2, middleware3);
```

이때 라우터 안의 미들웨어에서 `next("route")`를 쓰면 나머지 미들웨어들을 건너뛸 수 있다.

```js
router.get(
  "/",
  (req, res, next) => {
    console.log("PASS 1");
    next();
  },
  (req, res, next) => {
    console.log("PASS 2");
    next("route");
  },
  (req, res, next) => {
    console.log("SKIP");
    next();
  }
);
```

두번째 미들웨어에서 `next("route")`를 호출하면 세번째 미들웨어는 실행되지 않는다.

<br />

## 라우터 주소에 변수 넣기

```js
router.get("/users/:id", (req, res) => console.log(req.params, req.query));
```

- `:id`로 명시하면 아이디가 달라져도 동일한 라우터에서 응답을 해줄 수 있다.
- `:id`로 요청이 들어오면 `id`값은 `req.params` 객체에 저장되고 `req.params.id`로 조회할 수 있다. (`:type`이었다면 `req.params.type`으로 조회)
- `:id`와 같은 변수를 포함하는 라우터는 자기보다 뒤에 오는 라우터들에게 영향을 미치기 때문에 일반 라우터보다 뒤에 위치해야 다른 라우터를 방해하지 않는다.
