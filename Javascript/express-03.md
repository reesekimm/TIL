# 템플릿 엔진 - Pug

## 템플릿 엔진

HTML은 정적인 언어이기 때문에 표현하고 싶은 데이터를 전부 코드로 넣어주어야 한다. 1000개의 페이지를 위해 1000개의 HTML이 필요하고 만약 1000개의 페이지에 공통으로 들어가는 header나 footer에 수정사항이 생겼다면 1000번에 걸쳐서 동일한 작업을 해야 한다.

이러한 불필요한 작업을 하지 않도록 도와주는 툴이 바로 템플릿 엔진이다. 템플릿 엔진은 자바스크립트를 사용해서 반복되는 HTML 코드를 효율적으로 처리하고 렌더링할 수 있게 해준다.

템플릿 엔진을 쓰는 이유

- 코드의 반복을 최소화할 수 있다.
- Node.js 재실행 없이 페이지 새로고침만으로도 수정 내용을 바로 적용할 수 있다.
- 화면의 내용을 프로그래밍적으로 제어할 수 있다.

<br />

## [Pug](https://pugjs.org/api/getting-started.html)

<img src="https://cdn.freebiesupply.com/logos/large/2x/pug-logo-png-transparent.png" width="200" />

Node.js에서 사용하는 대표적인 템플릿 엔진 Pug는 문법이 간단해서 코드의 양을 획기적으로 줄여주기 때문에 꾸준한 인기를 얻고 있다.

### 1. 기본 세팅

`app.set()`을 사용해서 템플릿 엔진 관련 설정을 해준다.

> app.js

```js
...
app.set("views", path.join(__dirname, "templetes"));
app.set("view engine", "pug");
...
```

- `app.set`의 `"view"` 프로퍼티는 템플릿 파일들이 위치한 폴더를 지정한다. `res.render`메서드가 지정 폴더 기준으로 템플릿 엔진을 찾아서 렌더링 한다. default값으로 지정된 폴더는 `process.cwd() + '/views'`로 Node.js 명령을 호출한 디렉토리에 위치한 /views 폴더이다.

- `"view engine"` 프로퍼티는 어떤 템플릿 엔진을 사용할지를 나타낸다.

<br />

### 2. 문법 특징

> pug 파일

```pug
doctype html
html(lang='en')
  head
    title Pug
    script(type='text/javascript').
      foo = true;
      bar = function () {};
      if (foo) {
      bar(1 + 5)
      }
  body
    h1 Pug - node template engine
    #container.col
      p You are amazing
      p
        | Pug is a terse and simple
        | templating language with a
        | strong focus on performance
        | and powerful features.
```

> pug에서 렌더링된 html 파일

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>Pug</title>
    <script type="text/javascript">
      foo = true;
      bar = function() {};
      if (foo) {
        bar(1 + 5);
      }
    </script>
  </head>
  <body>
    <h1>Pug - node template engine</h1>
    <div id="container" class="col">
      <p>You are amazing</p>
      <p>
        Pug is a terse and simple templating language with a strong focus on
        performance and powerful features.
      </p>
    </div>
  </body>
</html>
```

**pug 문법의 특징**

1. 들여쓰기로 부모태그와 자식태그를 구분한다.
2. 닫는 태그가 없다.
3. 태그를 입력하지 않으면 자동으로 div를 생성해준다.
4. id는 `#id명`, class는 `.클래스명` 으로 입력한다.
5. 속성은 ( )괄호 안에 넣는다.

> [pug 문법 더 알아보기](https://pugjs.org/language/attributes.html)

<br />

### 3. 템플릿에 변수 전달하기

1. [res.locals](http://expressjs.com/en/api.html#res.locals) 로 전역 변수 만들기

여러 템플릿에 공통으로 들어가는 내용이 있다면 해당 내용이 변경됐을 때 모든 템플릿을 일일이 수정해주어야 하는 번거로움이 발생한다. 이러한 불편함을 없애주는게 바로 res.local 메서드인데, 템플릿들이 공통으로 렌더링 하는 값을 전역 변수에 저장해두고 렌더링 시 즉시 참조할 수 있게 해준다.

> app.js

```js
...
app.use(function (req, res, next) {
  res.locals.siteName = "Pug";
  next()
});
...
app.use("/", globalRouter);
app.use("/users", userRouter);
...
```

> index.pug

```pug
doctype html
html
  head
    title #{siteName}
  body
    p Hello
```

app.js에 `res.locals`로 전역변수를 만들어주는 커스텀 미들웨어를 추가한다. `res.locals.변수명 = 값`의 형태로 입력한다. 이 미들웨어는 모든 라우터가 참조할 수 있도록 라우팅 전에 위치하도록 한다.

템플릿 파일에는 `#{변수명}`을 입력해준다. 이제 미들웨어에서 지정해준 값이 `#{변수명}`을 포함하는 모든 템플릿에 일괄적용된다.

<br />

2. `res.render`로 직접 전달하기

템플릿별로 다른 정보를 전달할 때는 `res.render`의 인자에 해당 정보를 객체 형태로 넣어준다.

> controller.js

```js
export const index = (req, res, next) =>
  res.render("index", { pageTitle: "Home" });
```

> index.pug

```
doctype html
html
  head
    title #{pageTitle} | #{siteName}
  body
    p Hello
```

`res.render()`에 `{ 변수명: 값 }`의 형태로 정보를 전달하면 템플릿마다 자신이 전달받은 값을 렌더링하게 된다.

<br />

---

**참고**

- Node.js 교과서 (길벗)
- [익스프레스 템플릿(Jade, Pug), Express template](https://www.zerocho.com/category/NodeJS/post/578c64621e3613150037d3b3)
- [Pug 문법 정리 요약 (템플릿 엔진)](https://jeong-pro.tistory.com/65)
- [Express API reference](http://expressjs.com/en/api.html#app.set)
- [how to render common variables from app.js to all routes in express](https://stackoverflow.com/questions/29026650/how-to-render-common-variables-from-app-js-to-all-routes-in-express)
