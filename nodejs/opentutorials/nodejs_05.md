# [Node JS - 05] Not Found 구현하기

> [생활코딩 Node.js](https://www.youtube.com/playlist?list=PLuHgQVnccGMA9QQX5wqj6ThK7t2tsGxjm)

기본적으로 동적인 웹페이지는 query string의 값에 따라 요청받은 데이터를 사용자에게 제공한다. 하지만 사용자가 존재하지 않는 경로로 접근했을 경우에는 즉, query string의 값이 유효하지 않은 경우에는 응답이 불가능 하기 때문에 Not Found 오류를 전송한다.

#### 코드실습

1. 사용자의 요청에 따른 url 정보를 확인한다.

> main.js

```javascript
var http = require("http");
var fs = require("fs");
var url = require("url");

var app = http.createServer(function(request, response) {
  var _url = request.url;
  var queryData = url.parse(_url, true).query;
  var title = queryData.id;

  // url 정보 확인
  console.log(url.parse(_url, true));

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
    response.writeHead(200);
    response.end(template);
  });
});
app.listen(3000);
```

<br />

- 유효한 경로(`/?id=HTML`)일 경우 `console.log(url.parse(_url, true));` 출력 결과

```
Url {
  ...
  query: [Object: null prototype] { id: 'HTML' },
  pathname: '/',
  path: '/?id=HTML',
  href: '/?id=HTML' }
```

> path : query string을 포함한 전체 path를 보여준다.
> pathname : query string을 제외한 path만을 보여준다. ('/')

<br />

- 유효하지 않은 경로(`/asdf`)일 경우 `console.log(url.parse(_url, true));` 출력 결과

```
Url {
  ...
  query: [Object: null prototype] {},
  pathname: '/asdf',
  path: '/asdf',
  href: '/asdf' }
```

> 유효하지 않은 경로로 접근하면 `pathname`이 `path`와 동일한 값을 가진다. 따라서 `pathname`을 체크해서 사용자가 올바른 경로로 접근했는지 체크할 수 있다.

<br />

2. `pathname` 체크해서 "Not Found" 출력하기

```javascript
var http = require("http");
var fs = require("fs");
var url = require("url");

var app = http.createServer(function(request, response) {
  var _url = request.url;
  var queryData = url.parse(_url, true).query;
  var pathname = url.parse(_url, true).pathname;
  var title = queryData.id;

  if (pathname === "/") {
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
      response.writeHead(200);
      response.end(template);
    });
  } else {
    response.writeHead(404);
    response.end("Not Found");
  }
});
app.listen(3000);
```
