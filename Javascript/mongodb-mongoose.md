# mongoDB & Mongoose

## mongoDB

### mongoDB란?

MongoDB는 C++로 작성된 오픈소스로, 문서지향(**Document-Oriented**)적 Cross-platform 데이터베이스이자 **NoSQL** 데이터베이스의 대표주자이다. 뛰어난 확장성과 성능을 자랑한다.

> **NoSQL** <br />
> Not only SQL의 약자로 기존의 RDBMS의 한계를 극복하기 위해 만들어진 새로운 형태의 데이터 저장소이다. 관계형 DB가 아니므로 RDBMS처럼 고정된 Schema 및 JOIN이 존재하지 않는다.

> **RDBMS** <br />
> Relational DataBase Management System (관계형 데이터베이스 관리 시스템)은 행과 열로 된 2차원의 table로 데이터를 관리하는 데이터베이스 시스템이다. MySQL, Oracle, DB2 등이 RDBMS에 속한다.
>
> |    RDBMS     |      mongoDB      |
> | :----------: | :---------------: |
> |    Table     |    Collection     |
> | Row (Record) |     Document      |
> |    Column    |       Field       |
> |    Index     |       Index       |
> |     Join     | Embedded Document |
> | Foreign Key  |     Reference     |
>
> ![document data model](https://user-images.githubusercontent.com/42695954/65958332-1a566480-e48a-11e9-8f91-794bf24819fe.PNG)

<br />

- [Document](https://docs.mongodb.com/manual/core/document/) <br />
  Document는 mongoDB collection 안에 들어있는 각각의 record이다. 한 개 이상의 **key-value pair**로 이루어져 있어서 JSON 또는 mapping data type들과 비슷하다.
  ![document](https://user-images.githubusercontent.com/42695954/65957703-7ae4a200-e488-11e9-9a86-6b5e51a4556d.PNG)

- [Collection](https://docs.mongodb.com/manual/core/databases-and-collections/#collections) <br />
  Collection은 Document의 그룹이다. RDBMS의 table과 비슷한 개념이지만 RDBMS와 달리 Schema가 없다. 대신 각각의 Document들이 동적인 Schema를 가지고 있어서 같은 Collection 안에 들어있는 Document더라도 각자 다른 Schema를 가지고 다른 데이터(key-value)를 가질 수 있다.
  ![collection](https://user-images.githubusercontent.com/42695954/65957778-b7b09900-e488-11e9-8f5b-d0de6aa8c857.PNG)

- [Database](https://docs.mongodb.com/manual/core/databases-and-collections/#databases) <br />
  Database는 Collection들의 물리적인 컨테이너이다. 각 Database는 파일시스템에 여러파일들로 저장된다.

> Document, Collection, Database의 관계는 아래 이미지와 같다. <br /> <img src="https://user-images.githubusercontent.com/42695954/65965279-50e7ab80-e499-11e9-9417-d0d4ee759897.PNG" alt="diagram" width="500">

<br />

### mongoDB 설치하기 (windows10)

> [📽️ 참고 영상](https://www.youtube.com/watch?v=FwMwO8pXfq0)

#### 1. mongoDB 다운로드

1. [mongoDB 홈페이지](https://www.mongodb.com/download-center/community)에서 Version과 OS 선택 후 msi 파일 다운로드
2. 라이센스 동의 후 Setup type화면에서 'Complete' 선택, 모든 설정을 default로 유지한 후 next 버튼 클릭
3. MongoDB Compass 체크박스에 체크표시(default)후 next 버튼 클릭
4. Install 버튼 클릭하여 설치 후 Finish 버튼 클릭하여 설치 완료

#### 2. 데이터가 저장될 폴더 생성

C:\에 data폴더를 만들고 다시 그 안에 db폴더 만들기

#### 3. mongoDB 실행

윈도우 **cmd** 콘솔에서 다음의 과정을 순차적으로 진행

1. **mongoDB가 설치된 경로로 이동**

```
cd C:\Program Files\MongoDB\Server\4.2\bin
```

2. **mongod 명령어로 mongoDB 실행**

```
C:\Program Files\MongoDB\Server\4.2\bin>mongod
```

- 27017번 포트에서 연결 대기 중이라는 메시지가 떴다면 mongoDB 실행에 성공한 것
- mongoDB를 사용할 일이 있을 때마다 mongod 명령어로 먼저 서버를 실행해야 함

3. **mongo 명령어로 mongoDB 프롬프트에 접속하기**

```
C:\Program Files\MongoDB\Server\4.2\bin>mongo
```

- mongoDB가 설치된 경로에서 새로운 콘솔창을 열어 mongo 명령어 입력
- 프롬프트가 >로 바뀌었다면 성공

<br />

---

mongoDB 설치를 완료했으나 프로젝트 directory에서 mongod가 실행되지 않는다면 다음의 순서대로 환경변수를 추가한다.

1. '내컴퓨터' 우클릭 > '고급시스템설정' > '환경변수(N)'
2. 시스템변수 리스트 중 'Path'를 선택하고 편집을 선택한 후 'Mongodb 설치경로'(C:\Program Files\MongoDB\Server\4.2\bin) 추가
3. vscode 재실행 후 터미널에 'mongod'입력

---

<br />

### [mongoDB Database, Collection, Document CRUD](https://docs.mongodb.com/manual/crud/) 알아보기

<br />

## **Mongoose**

### Mongoose란?

Mongoose는 Node.js를 위한 ODM(Object Document Modeler) 라이브러리이다.<br />
mongoDB는 테이블이 없어서 자유롭게 데이터를 넣을 수 있지만 실수로 잘못된 자료형의 데이터를 넣거나 다른 Document에는 없는 필드의 데이터를 넣을 수도 있어서 데이터의 일관성을 보장할 수 없다는 단점이 있다. mongoose를 쓰게되면 mongoDB에 데이터를 넣기 전 노드 서버 단에서 테이터를 한 번 필터링할 수 있기 때문에 이러한 단점을 어느정도 보완할 수 있다.

<br />

### mongoDB에 Mongoose연결하기

> 1. npm으로 mongoose를 설치한다.

```
$ npm install mongoose
```

> 2. mongoDB와 연결한다.

```js
import mongoose from "mongoose";

// mongoose와 mongoDB 연결
mongoose
  .connect("mongodb://localhost/test", {
    useNewUrlParser: true,
    useFindAndModify: false,
    useUnifiedTopology: true
  })
  .catch(err => console.log("Initial connection error. / ", err));

const db = mongoose.connection;

const handleOpen = () => console.log("Conneted to DB!");
const handleErr = err => console.log(`Error on DB Coeenction to ${err}`);

// '최초' 연결 시 handleOpen 실행
db.once("open", handleOpen);

// 연결 에러 발생 시 handleErr 실행
db.on("error", handleErr);
```

`mongoose.connect("mongoDB 주소", {mongoose tuning options})`를 사용해서 mongoDB와 연결한다. 자세한 문서는 [여기서](https://mongoosejs.com/docs/connections.html) 볼 수 있다.

<br />

### Mongoose Namaing Conventions

<img src="https://user-images.githubusercontent.com/42695954/65875776-e8c69600-e3c2-11e9-90e8-2bd10e256bd2.PNG" alt="naming-convention" width="600"/>

- Collection:

  - 여러개의 Document가 담긴 묶음이다.
  - 여러개의 “entries”와 “rows”가 있다.

- Document:

  - Schema를 토대로 구조화된 데이터를 담고 있다.

- Schema:

  - Document의 Data 구조가 어떻게 생겼는지 JSON 형태로 정의해놓은 것이다.
  - 여러개의 path를 가지고 있다.

- Path:
  - Path는 key-value pair를 의미한다.
  - 각각의 path는 Document의 entry가 된다.
  - row에 있는 하나의 value이다.

<br />

### [Schema](https://mongoosejs.com/docs/guide.html) Example

```js
const newSchema = new mongoose.Schema({
  title:  String,
  author: String,
  body:   String,
  comments: [{ body: String, date: Date }],
  date: { type: Date, default: Date.now },
  hidden: Boolean,
  meta: {
    votes: Number,
    favs:  Number
});
```

> [SchemaTypes](https://mongoosejs.com/docs/schematypes.html) 알아보기

<br />

### [Model](https://mongoosejs.com/docs/models.html)

Model은 Schema에 정의된 데이터 구조대로 Document를 생성하는 생성자(constructor)이다. Model의 instance가 바로 Document다.

<img src="https://user-images.githubusercontent.com/42695954/65960261-0fea9980-e48f-11e9-8106-607eb16dc1ed.PNG" alt="model" width="600">

```js
// Schema 생성
var schema = new mongoose.Schema({
  name: "string",
  size: "string"
});

// Model 정의
var Tank = mongoose.model("Tank", schema);

// Document 생성
var small = new Tank({ size: "small" });
```

Model은 Document의 CRUD를 위한 [API](https://mongoosejs.com/docs/api/model.html)를 제공하기 때문에 API를 사용해서 Document 탐색/수정/삭제하는 것이 가능하다.

```js
var Tank = mongoose.model("Tank", yourSchema);
```

<br />

---

**참조**

- [Advanced Topics in Concurrency and Reactive Programming: MongoDB, Mongoose](https://slideplayer.com/slide/14850099/)
- [MongoDB 강좌 1편: 소개, 설치 및 데이터 모델링](https://velopert.com/436)
- [Node.JS 강좌 11편: Express와 Mongoose를 통해 MongoDB와 연동하여 RESTful API 만들기](https://velopert.com/594)
- Node.js 교과서 (길벗)
- [Documents and Data Models](https://docs.mongodb.com/getting-started/cpp/documents/)
