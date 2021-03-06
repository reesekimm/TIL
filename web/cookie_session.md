# 쿠키 & 세션

HTTP에는 `비연결성(Connectionless)`과 `비상태성(Stateless)` 이라는 특징이 있다.

- 비연결성 : 모든 요청마다 연결/해제의 과정을 거친다. = 요청에 따른 응답을 받으면 연결이 끊어진다.
- 비상태성 : 연결이 끊어지면 어떠한 상태 정보도 남지 않는다.

모든 요청을 일회성으로, 독립적으로 처리함으로써 **서버의 자원을 절약**할 수 있다.  
이러한 특징들로 인해 서버는 사용자를 기억하거나 식별할 수 없고 매 요청마다 새로운 사용자로 인식한다.

하지만 현대의 웹사이트들은 더 나은 사용자 경험을 제공하고, 사용자 개개인별로 특화된 서비스를 제공하고 싶어한다.  
이를 위해서는 HTTP의 비연결성과 비상태성을 보완해서 사용자를 식별할 수 있어야 하는데, 이때 필요한 것이 바로 쿠키와 세션이다.

<br />

## 쿠키 (Cookie)

<br />

- 웹사이트에 접속할때 생성되는 정보를 담은 임시 파일
- 사용자/서버의 정보를 담은 데이터 조각

<br />

### 개념/특징

- 서버로부터 발급받아서 브라우저에 저장해놓고 서버에 요청을 보낼때마다 함께 보낸다. (정확히는 브라우저가 지정한 컴퓨터의 메모리 또는 하드디스크에 저장됨)
- 브라우저는 쿠키를 통해 서버를 식별하고, 서버는 쿠키를 통해 사용자를 식별한다.
- key, value로 구성
  - `Cookie: name="Brian Totty";`
  - `Cookie: id="234131"; domain="joes-hardware.com";`
- 브라우저마다 저장되는 쿠키가 다르다. 서버는 브라우저가 달라지면 다른 사용자로 인식한다.

<br />

### 사용 목적

서버가 사용자를 식별하는데 사용됨

- 세션관리 (사용자 상태정보 관리) : 아이디, 비밀번호, 접속 시간, 장바구니, 등
- 개인화된 데이터 제공
- 트래킹 : 사용자의 행동 / 웹사이트 사용 패턴을 기록

<br />

### 사용 예시

> 로그인 없이 사용자 정보/상태를 유지하는 기능들

- 로그인 상태 유지 (자동 로그인)
- 일주일간 다시 보지 않기
- 최근 검색한 상품을 광고에서 추천
- 장바구니 보관

<br />

### 단점

- 사용자단에 저장되는 데이터이기 때문에 쉽게 변조하거나 지우거나 가로챌 수 있다. -> 보안에 취약 -> 민감한 개인정보를 담는 것은 위험하다.
- 브라우저에서 쿠키를 거부하는 기능을 설정할 경우 쿠키의 목적인 브라우저-서버 연결을 지속시키는 기능을 수행할 수 없게 된다.

<br />

## 세션 (Session)

- 사용자가 브라우저를 통해 서버와 접속해있는 `상태`
  - 사용자가 브라우저를 통해 서버에 접속한 시점부터 브라우저를 종료함으로써 서버와의 연결을 끝내는 시점까지를 하나의 단위로 보고 이를 세션이라고 지칭
- 사용자와 서버 사이의 연결 상태를 유지시키는 `기술`
- 사용자 `정보`
  - 서버에서 가지고 있는 사용자 정보 (아이디, 비밀번호, 이메일, 장바구니, ...)

<br />

### 세션/쿠키 방식

![session cookie flow](https://i.postimg.cc/6Qgx1xBQ/image.png)

> [[출처](https://techbriefers.com/how-to-work-with-session-and-cookies-in-codeigniter/)]

1. 사용자가 브라우저를 통해 로그인 한다.
2. 서버는 사용자 세션을 생성하고 해당 사용자 세션과 연결되는 세션ID를 쿠키로 만든다. (=`세션쿠키`)
3. 로그인 성공시 세션쿠키를 브라우저에 전달한다.
4. 사용자는 이후 매 요청마다 세션쿠키를 함께 보낸다.
5. 서버는 이 쿠키를 통해 사용자를 식별하고 사용자 정보를 얻는다.
6. 사용자에게 개인화된 데이터를 제공한다.

<br />

### 특징 / 장점

- 세션은 쿠키에 의존적이다. 세션ID를 쿠키로 만들어서 세션쿠키를 통해 사용자를 식별하기 때문.
- 세션쿠키에는 세션ID값만 담겨 있고 중요한 사용자 정보(비밀번호, 접속시간 등)는 포함되어있지 않기 때문에 일반 쿠키 보다 보안상 안전하다.
- 서버측에서는 세션쿠키 하나 만으로도 사용자를 식별할 수 있다. (아이디, 비밀번호 등을 일일히 매칭하지 않아도 된다.)

<br />

### 단점

- 세션을 저장하기 위한 추가적인 저장 공간이 필요하다. (서버 자원 소모)
- 서버 또는 세션DB의 부하가 높아진다.
- HTTP 요청 중에 세션쿠키를 가로채서 서버에 요청을 보낼 경우 사용자 정보(세션)를 훔칠 수 있다. (세션 하이잭킹)
  > 해결책
  >
  > - HTTPS를 사용 -> 쿠키를 훔치더라도 안의 정보를 읽기 힘들게 한다.
  > - 세션쿠키에 유효시간을 넣는다.

<br />

## 쿠키 vs 세션

<br />

### 공통점

- 사용자를 식별하기 위해 사용되는 수단이다.
- 사용자 정보가 담겨 있다.

<br />

### 차이점

쿠키와 세션을 `사용자 정보`의 관점에서 비교/정리 해보면

<br />

#### 쿠키는

- 사용자의 하드디스크/메모리에 저장
- 자동완성, 팝업 일주일간 보지 않기 등 사용자 편의를 제공하기위해 사용
- 지워지거나 변조되거나 가로채여도 큰 지장이 없는 수준의 사용자 정보를 담는다.

<br />

#### 세션은

- 서버에 저장
- 비밀번호, 결제정보 등 노출되면 안되는 중요한 개인정보를 담는다.

<br />

#### 지속쿠키 vs 세션쿠키

쿠키는 크게 지속쿠키(persistent cookie)와 세션쿠키(session coockie) 두 가지로 나눌 수 있다.  
위에서 말하는 '쿠키'는 지속쿠키이다.

- **지속쿠키** ─ 하드디스크에 저장되어 사용자가 브라우저를 닫거나 컴퓨터를 재시작 하더라도 삭제되지 않고 남아있다.
- **세션쿠키** ─ 사용자가 브라우저를 닫으면 삭제된다.

둘의 차이는 파기되는 시점뿐이다.  
쿠키에 만료일을 나타내는 파라미터(Expires, Max-Age, Discard)가 설정/포함되어 있으면 지속쿠키이고 그렇지 않으면 세션쿠키이다.

<br />

---

- [[web] 쿠키(cookie)와 세션(session)의 개념/차이/용도/작동방식](https://devuna.tistory.com/23)
- [쉽게 알아보는 서버 인증 1편(세션/쿠키 , JWT)](https://tansfil.tistory.com/58)
- [서버 기반 인증, 토큰 기반 인증 (Session, Cookie / JSON Web Token)](https://dooopark.tistory.com/6)
- HTTP 완벽 가이드 (인사이트)
