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

<br />

## 라우터 없이 정적인 파일 서비스하기

`express.static`은 express의 built-in 미들웨어로 이미지, 동영상, CSS파일, Javascript파일 같은 정적인 파일을 응답으로 제공할 수 있게 해준다.

> app.js

```js
app.use(express.static("public"));
```

함수의 인자로 정적 파일들이 담겨있는 root 디렉토리를 지정한다. 위 예시에서는 /public 폴더로 지정했고 이 폴더에 있는 정적인 파일들을 아래 경로에서 응답받을 수 있다.

```
http://localhost:3000/images/kitten.jpg
http://localhost:3000/css/style.css
http://localhost:3000/js/app.js
http://localhost:3000/images/bg.png
http://localhost:3000/hello.html
```

참고) express는 root 디렉토리를 기준으로 파일을 검색하기 때문에 디렉토리의 이름(public)은 url에 포함되지 않는다.

<br />

- 경로가 지정되면 express가 root 경로를 탐색해서 파일을 가져오기 때문에 root 경로를 제외한 나머지 경로만 입력해도 된다.

> app.js

```js
app.use(express.static("public"));
app.get("/static", (req, res) =>
  res.send("Hello express.static, <img src='/hi.png'/>")
);
```

```
http://localhost:3000/static/hi.png
```

<br />

- 정적인 파일을 제공할 때 실제로 file system에 존재하지 않는 임의의 경로를 지정해줄수도 있다.

> app.js

```js
app.use("/static", express.static("public"));
```

`/static`이라는 경로를 지정하면, `/static`을 붙인 주소를 통해 해당 파일에 접근할 수 있다.

```
http://localhost:3000/static/images/kitten.jpg
http://localhost:3000/static/css/style.css
http://localhost:3000/static/js/app.js
http://localhost:3000/static/images/bg.png
http://localhost:3000/static/hello.html
```

<br />

- static 미들웨어는 요청에 부합하는 정적 파일을 발견하면 응답으로 해당 파일을 전송한다. 응답에 성공하면 다음에 나오는 라우터를 실행하지 않는다. 반대로 파일을 찾지 못해서 응답에 실패할 경우는 요청을 다음 라우터로 넘긴다. 이렇게 **자체적으로 라우터 기능을 수행**하기 때문에 최대한 위쪽에 배치해서 서버가 불필요한 미들웨어 작업을 수행하는 것을 막는 것이 좋다.

```js
...
app.use(logger("dev"));
app.use(express.static(path.join(__dirname, "public")));
app.use(express.json());
app.use(express.urlencoded({extended: false}));
app.use(cookieParser());
...
```

<br />

---

**참조**

- Node.js 교과서 (길벗)
- [Express-정적파일을 서비스하는 법](https://opentutorials.org/course/2136/11857)
- [Serving static files in Express](http://expressjs.com/en/starter/static-files.html)
