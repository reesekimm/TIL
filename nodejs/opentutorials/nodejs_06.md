# [Node JS - 06] `fs.readdir()`로 파일 목록 가져오기

> [생활코딩 Node.js](https://www.youtube.com/playlist?list=PLuHgQVnccGMA9QQX5wqj6ThK7t2tsGxjm) 수업 내용 정리

기존코드는 카테고리가 추가/삭제될 때 마다 템플릿을 함께 수정해줘야 하는 번거로움이 있다. (`<ul>`태그로 감싼 리스트 부분)

> main.js

```javascript
// ...
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

그렇다면 글목록이 변경 됐을 때 템플릿의 리스트 부분이 자동으로 바뀌게 할 순 없을까?  
이때 쓸 수 있는 메서드가 바로 `fs.readdir()`이다. 특정 directory 안에 들어있는 파일 리스트를 가져올 때 쓸 수 있다.

```
fs.readdir(path[, options], callback)
```

- 첫번째 인자에는 `fs.readdir()`을 호출한 파일로부터 directory까지의 상대경로를 따져서 문자열로 넣어준다.
- [options]에는 보통 인코딩 방식이 오며 웹에서는 utf8을 주로 사용한다.
- callback은 파라미터로 `error`와 `files`를 갖고, `files`에는 directory에 들어있는 파일 리스트가 배열의 형태로 전달된다.

<br />

**Example**

> readdir.js

```javascript
var testFolder = "../data"; // 목록이 포함된 폴더의 상대경로
var fs = require("fs");

fs.readdir(testFolder, function(error, fileList) {
  console.log(fileList);
});
```

> 콘솔 출력 결과

```
[ 'CSS', 'HTML', 'Javascript' ]
```

data폴더에 들어있던 파일들의 이름이 문자열의 형태로 배열에 담겨서 리턴된다.

<br />

`fs.readdir()`을 사용해서 기존 코드를 개선해보자.

```javascript
const http = require("http");
const fs = require("fs");
const url = require("url");
const listHTML = fileList => {
  var list = "<ul>";
  var i = 0;
  while (i < fileList.length) {
    list += `<li><a href="/?id=${fileList[i]}">${fileList[i]}</a></li>`;
    i++;
  }
  list = list + "</ul>";
  return list;
};
const templateHTML = (title, list, body) => `
  <!doctype html>
    <html>
    <head>
    <title>WEB1 - ${title}</title>
    <meta charset="utf-8">
    </head>
    <body>
    <h1><a href="/">WEB</a></h1>
    ${list}
    ${body}
    </body>
    </html>
  `;

var app = http.createServer(function(request, response) {
  let _url = request.url;
  let queryData = url.parse(_url, true).query;
  let pathname = url.parse(_url, true).pathname;
  if (pathname === "/") {
    if (queryData.id === undefined) {
      fs.readdir("./data", function(error, fileList) {
        var title = "Welcome";
        var description = "Hello, Node.js";
        var list = listHTML(fileList);
        var template = templateHTML(
          title,
          list,
          `<h2>${title}</h2>${description}`
        );
        response.writeHead(200);
        response.end(template);
      });
    } else {
      fs.readdir("./data", function(error, fileList) {
        var list = listHTML(fileList);
        fs.readFile(`data/${queryData.id}`, "utf8", function(err, description) {
          var title = queryData.id;
          var template = templateHTML(
            title,
            list,
            `<h2>${title}</h2>${description}`
          );
          response.writeHead(200);
          response.end(template);
        });
      });
    }
  } else {
    response.writeHead(404);
    response.end("Not found");
  }
});
app.listen(3000);
```

`fs.readdir()`의 callback에서 파일 리스트를 받아서 리스트를 업데이트하고 `fs.readFile()`로 업데이트된 리스트와 파일의 내용을 담은 HTML 템플릿을 브라우저에 전달하는 구조이다.  
이제 data 폴더에 들어있는 파일의 변경사항에 따라 브라우저 화면상에 리스트와 본문의 내용이 자동으로 업데이트 된다:)
