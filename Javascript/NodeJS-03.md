# [Node JS - 03] Node.js `fs.fileRead()`로 동적인 웹페이지 만들기

> [생활코딩 Node.js](https://www.youtube.com/playlist?list=PLuHgQVnccGMA9QQX5wqj6ThK7t2tsGxjm) 수업 내용 정리

## 1. `fs.fileRead()`로 파일 읽기

Node.js 어플리케이션에서 파일을 읽을때에는 [File System](https://nodejs.org/dist/latest-v10.x/docs/api/fs.html#fs_file_system)(파일처리) 모듈에서 [fs.fileRead()](https://nodejs.org/dist/latest-v10.x/docs/api/fs.html#fs_fs_readfile_path_options_callback)라는 메서드를 호출하면 된다. (fs는 file system의 약자)

<br />

#### fs.readFile(path[, options], callback)

- path에 읽어올 파일의 경로를 입력하면 해당 파일을 [options]의 방식으로 읽은 후 파일을 다 읽으면 callback함수를 호출한다.
- [options]에는 보통 인코딩 방식이 오며 웹에서는 utf8을 주로 사용한다.

<br />

#### 코드 실습

> sample.txt

```
Lorem ipsum dolor sit amet consectetur adipisicing elit. Asperiores non eaque
aliquam nam id et possimus atque ipsum voluptatem alias fugit nostrum eligendi
quisquam enim neque, corrupti sunt repudiandae minus.
```

> fileRead.js

```javascript
// file system 모듈 불러오기
const fs = require("fs");

// 메서드 호출
fs.readFile("sample.txt", "utf8", (err, data) => {
  if (err) throw err;
  console.log(data);
});
```

비동기적 읽기에서 `callback`으로 전달된 함수는 매개변수로 `err`와 `data`를 갖는다. 파일로부터 읽은 데이터 내용이 매개변수 `data`로 전달되어 함수 내에서 접근할 수 있다.

> `node fileRead.js`입력하여 어플리케이션 실행시 콘솔 출력 결과

```
Lorem ipsum dolor sit amet consectetur adipisicing elit. Asperiores non eaque
aliquam nam id et possimus atque ipsum voluptatem alias fugit nostrum eligendi
quisquam enim neque, corrupti sunt repudiandae minus.
```

<br />

## 2. 동적인 웹페이지 만들기

`fs.fileRead()` 메서드를 사용하여 동적인 웹페이지를 만들수 있다.

> 참고자료 - [정적인 웹페이지 vs 동적인 웹페이지](https://titus94.tistory.com/4)

아래 이미지처럼 사용자가 보는 화면은 동일하지만 어플리케이션(main.js)의 코드 구조는 정적일때와 동적일때가 매우 다르다.

> UI  
> : 리스트(HTML, CSS, Javascript) 클릭 시 본문의 타이틀과 내용이 변경되는 구조다.
> <img src="https://user-images.githubusercontent.com/42695954/64851505-7891e380-d652-11e9-94db-a58e465bdac0.PNG" alt="port80" width="500"/>

<br />

> main.js (수정 전 - 정적인 웹페이지)

```javascript
var http = require("http");
var fs = require("fs");
var app = http.createServer(function(request, response) {
  var url = request.url;
  if (request.url == "/") {
    url = "/index.html";
  }
  if (request.url == "/favicon.ico") {
    response.writeHead(404);
    response.end();
    return;
  }
  response.writeHead(200);
  response.end(fs.readFileSync(__dirname + url));
});
app.listen(3000);
```

- HTML, CSS, Javascript html파일을 별도로 관리해준다.
- 통신에 성공할 경우 `\_\_dirname + url`에 위치한 파일을 읽어와서 화면에 보여주는 방식이다.

<br />

> main.js (수정 후 - 동적인 웹페이지)

```javascript
var http = require("http");
var fs = require("fs");
var url = require("url");

var app = http.createServer(function(request, response) {
  var _url = request.url;
  var queryData = url.parse(_url, true).query;
  var title = queryData.id;
  if (_url == "/") {
    title = "Welcome";
  }
  if (_url == "/favicon.ico") {
    response.writeHead(404);
    response.end();
    return;
  }
  response.writeHead(200);
  fs.readFile(`data/${title}`, "utf8", function(err, description) {
    var template = `<!doctype html>
    <html>
    <head>
      <title>WEB1 - ${title}</title>
      <meta charset="utf-8">
    </head>
    <body>
      <h1><a href="/">WEB</a></h1>
      <ul>
        <li><a href="/?id=HTML">HTML</a></li>
        <li><a href="/?id=CSS">CSS</a></li>
        <li><a href="/?id=Javascript">JavaScript</a></li>
      </ul>
      <h2>${title}</h2>
      <p>${description}</p>
    </body>
    </html>
    `;
    response.end(template);
  });
});
app.listen(3000);
```

- 별도로 관리하던 HTML, CSS, Javascript html파일들에서 공통적으로 사용하는 부분을 추출하여 `fs.fileRead()`의 콜백함수에 넣고 렌더링 해준다. (화면에 따라 바뀌어야 하는 부분은 template literal로 처리)
- 이제 템플릿 하나만 수정하면 변경된 UI가 모든 화면에 반영되기 때문에 HTML, CSS, Javascript의 본문에 해당하는 내용만 관리해주면 된다!

<br />

---

참고) `fs.readFileSync(path[, options])`

- 파일을 동기적으로 읽을 때 쓰는 메서드
- path에 위치한 파일을 [options]의 방식으로 읽은 후 문자열을 반환한다. (동기적)

'Sync'가 붙은 것은 동기적 읽기, 붙지 않은 것은 비동기적 읽기이다. 동기적 읽기로 읽게 되면 파일을 읽으면서 다른 작업을 동시에 할 수 없다.
