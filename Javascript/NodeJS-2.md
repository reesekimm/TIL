# [Node JS - 02] Node.js URL로 입력된 값 사용하기

> [생활코딩 Node.js](https://www.youtube.com/playlist?list=PLuHgQVnccGMA9QQX5wqj6ThK7t2tsGxjm) 수업 내용 정리

## 1. URL의 이해

자바스크립트를 이용해서 Node.js가 가지고 있는 기능을 호출하면 웹어플리케이션을 Node.js로 만들 수 있게 된다.
Node.js로 웹어플리케이션을 구현하는 아주 중요한 테크닉 중 하나인 `URL`을 살펴본다.

<img src="https://user-images.githubusercontent.com/42695954/64776590-73219400-d593-11e9-87a6-80a17b213022.PNG" alt="url" width="500"/>

- protocol (통신규약)

  - 사용자가 서버에 접속할 때 어떤 방식으로 접속할 것인가에 대한 부분
  - HTTP(Hyper Text Transfer Protocol)은 웹브라우저와 웹서버가 서로 데이터를 주고 받기 위해서 만든 통신 규약
  - 만약 통신 규약으로 FTP를 쓴다면, http 대신 ftp가 되어 있을 것

- host (domain)

  - 인터넷에 접속되어 있는 각각의 컴퓨터를 가리키는 주소

- port

  - 한 대의 컴퓨터 안에 여러 개의 서버가 있을 경우 클라이언트가 어떤 서버와 통신할지가 애매해진다.
    <img src="https://user-images.githubusercontent.com/42695954/64784401-0a431780-d5a5-11e9-897e-4949ea5664f2.PNG" alt="port" width="500"/>
  - 이때 클라이언트에게 어떤 서버를 연결해줄지 결정하는게 바로 포트이다.
    <img src="https://user-images.githubusercontent.com/42695954/64785836-2dbb9180-d5a8-11e9-8bb6-bb38d045fa86.PNG" alt="port80" width="500"/>
  - 컴퓨터에는 0 ~ 65535번까지의 포트라는 문이 있다.
  - 웹서버를 실행시킬 때는 80번 포트에 웹서버를 실행시키게 된다. 즉, 웹서버가 80번 포트를 리스닝하게 만든다.
  - 사용자가 `http://a.com:80`을 입력하고 엔터를 치게 되면 웹브라우저는 `http://a.com`에 해당하는 컴퓨터에게 80번 포트에 연결하고 싶다는 요청을 보낸다. 그리고 컴퓨터가 80번 포트에서 리스닝하고 있는 웹서버를 호출하면서 해당 웹서버가 응답할 수 있게 되는 것이다.
  - 80번 포트는 HTTP 웹서버가 리스닝하기로 전세계적으로 약속되어 있기 때문에 생략 가능하다. -> `http://a.com:80`이 아니라 `http://a.com`만 입력해도 컴퓨터가 사용자를 80번 포트의 웹서버에 연결시켜줄 수 있다.

- path

  - 컴퓨터 안에 있는 어떤 디렉토리에 어떤 파일인지에 대한 경로

- query string
  - 쿼리 스트링을 변경함으로써 웹서버에게 요청을 보낼 수 있다.
  - 쿼리 스트링은 `?`로 시작하며 각각의 값은 `&`으로 구분하며 값의 이름과 값은 `=`을 쓰기로 약속되어 있다.

<br />

## 2. Node.js URL로 입력된 값 사용하기

<img src="https://user-images.githubusercontent.com/42695954/64786396-4d9f8500-d5a9-11e9-9d83-612ef7f0b1a5.PNG" alt="port80" width="500"/>

사용자가 URL을 통해 웹어플리케이션에 접속했을 때, 웹서버는 쿼리 스트링에 따라 적당한 컨텐츠를 보여주게 된다. 그렇다면 웹어플리케이션은 쿼리 스트링을 어떻게 알 수 있을까?

#### - main.js라는 Node.js 어플리케이션이 쿼리스트링을 알아내는 방법

```javascript
var http = require("http");
var fs = require("fs");
var url = require("url");

var app = http.createServer(function(request, response) {
  var url = request.url;
  if (url == "/") {
    url = "/index.html";
  }
  if (url == "/favicon.ico") {
    response.writeHead(404);
    response.end();
    return;
  }
  response.writeHead(200);
  response.end(fs.readFileSync(__dirname + url));
});
app.listen(3000);
```

- line 4의 url 안에 있는 값을 분석해서 쿼리스트링을 추출할 수 있음
  - `nodejs url parse query string`으로 구글링한 [결과](https://stackoverflow.com/questions/8590042/parsing-query-string-in-node-js) 참조하여 코드 수정

```javascript
var http = require("http");
var fs = require("fs");
var url = require("url");

var app = http.createServer(function(request, response) {
  var _url = request.url;
  var queryData = url.parse(_url, true).query;
  console.log(queryData);
  if (_url == "/") {
    _url = "/index.html";
  }
  if (_url == "/favicon.ico") {
    response.writeHead(404);
    response.end();
    return;
  }
  response.writeHead(200);
  response.end(fs.readFileSync(__dirname + _url));
});
app.listen(3000);
```

- line 3에 `url` 모듈 추가 후 기존 line 4에 있던 `url`을 전부 `_url`로 수정
- 브라우저 주소창에 `http://localhost/3000/?id=HTML`입력 후 엔터를 치면 `console.log(queryData);`의 결과값으로 콘솔에 { id: HTML } 객체가 출력됨
- 이제 `queryData` 객체의 프로퍼티에 접근해서 쿼리 스트링을 알아낼 수 있음
