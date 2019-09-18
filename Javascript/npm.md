# npm

## npm (Node Package Manager)

- **Node** : Node.js로 만들어진
- **Package** : 패키지(모듈)를
- **Manager** : 관리해주는 프로그램

> 패키지란?
>
> - npm에 업로드된 모듈이다.
> - 모듈은 특정한 기능을 하는 함수나 변수들의 집합으로 자체로도 하나의 프로그램이면서 다른 프로그램의 부품으로 사용할 수 있다.
> - 개발효율성과 유지보수 용이성을 높여준다.
> - 모듈이 다른 모듈을 사용할 수도 있는데, 이러한 관계를 의존관계라고 한다. (dependency)
> - 좀 더 정확히 얘기하면 npm에 올라온 모든 모듈을 패키지라고 할 순 없다. `package.json`파일을 포함하는 모듈만 패키지이다.  
>   (참조: [About packages and modules](https://docs.npmjs.com/about-packages-and-modules))

<br />

## npm이 하는 일

1. **패키지 저장소 역할**  
   패키지를 npm에 배포, 공개할 수 있다.
2. **패키지 관리를 위한 CLI(Command line interface) 제공**  
   간단한 명령어로 패키지설치, 삭제, 버전관리, 의존성관리가 가능하다.

<br />

## npm 설치하기

- [node.js](https://nodejs.org/en/) 설치 시 자동으로 설치된다.
- node.js는 npm을 사용하기 위해서 꼭 필요하다.

<br />

## npm 사용하기

```
npm init
```

- package.json 파일 생성

  - 서비스에 필요한 패키지를 하나씩 추가하다 보면 패키지 수가 100개를 훌쩍 넘어 버리게 된다. 그리고 사용할 패키지는 저마다 고유한 버전이 있으므로 어딘가에 기록해두어야 한다. 같은 패키지라도 버전별로 기능이 다를 수 있으므로 동일한 버전을 설치하지 않으면 문제가 생기기 때문. 패키지의 버전관리를 위해서 필요한 파일이 바로 `package.json`이다.
  - 따라서 프로젝트를 시작할때 package.json부터 만들고 시작하는것이 좋다.
  - 패키지를 설치/업데이트하면 해당 패키지의 이름과 버전이 자동으로 `package.json`에 기록된다.

<br />

```
npm install
```

- `package.json`에 기록되어있는 모든 패키지 설치
  - `package.json`만 있으면 `npm install`로 모든 패키지를 다시 설치할 수 있음
- `npm install 패키지@버전`
  - 특정 패키지의 특정한 버전만 설치
- `npm install 패키지1 패키지2 패키지3 ...`
  - 여러 개의 패키지를 동시에 설치
- `npm install url`
  - 특정한 저장소에 있는 패키지를 설치
  - 주로 Github에만 있는 패키지를 설치할 때 사용
- `npm install -g 패키지이름`
  - 패키지를 글로벌에 설치
  - 이 프로젝트뿐만 아니라 다른 프로젝트도 해당 패키지를 사용 가능
  - 글로벌에 설치한 패키지는 `package.json`에 기록되지 않음

<br />

```
npm outdated
```

- 업데이트할 수 있는 패키지가 있는 확인

<br />

```
npm update
```

- 업데이트 가능한 모든 패키지를 업데이트

<br />

```
npm uninstall 패키지이름
```

- 패키지 제거
- 해당 패키지가 `node_modules`폴더와 `package.json`에서 삭제됨

<br />

```
npm info 패키지이름
```

- 패키지 세부 정보 확인 (package.json의 내용, 의존관계, 설치 가능한 버전정보 등)

<br />

```
npm adduser
```

- npm 로그인 (npm 공식사이트에서 가입한 계정 사용)
- 패키지 배포 시 로그인 필요

<br />

```
npm whoami
```

- 로그인한 사용자가 누구인지 알려줌
- 로그인 상태가 아닌 경우 에러 발생

<br />

```
npm logout
```

- 로그인한 계정에서 로그아웃

<br />

```
npm version 버전
```

- 버전 위치에 원하는 버전을 넣으면 `package.json`의 버전을 올려줌
- major, minor, patch 옵션을 사용할 수도 있음 --- [참고](https://docs.npmjs.com/cli/version)

<br />

```
npm deprecate 패키지이름@버전 "메시지"
```

- 해당 패키지를 설치할 때 경고 메시지를 띄우게 함
- 자신의 패키지에만 사용할 수 있음

<br />

```
npm publish
```

- 자신이 만든 패키지를 배포

<br />

```
npm unpublish
```

- 배포한 패키지를 제거
- 24시간 이내에 배포한 패키지만 제거 가능 (의존성 때문)

<br />

기타 다른 명령어는 [CLI Commands](https://docs.npmjs.com/cli-documentation/cli)에서 확인 가능

---

**참조**

- Node.js 교과서 (길벗)
- [npm이란?](http://itnovice1.blogspot.com/2019/01/js-npm.html)
- [모듈화와 npm(node package manager)](https://poiemaweb.com/nodejs-npm)
