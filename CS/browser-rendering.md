1. 임의의 웹사이트를 선정한다.

[브런치](https://brunch.co.kr/)

2. HTML안에 js, css의 위치는 어디에 위치했는가? 왜 그랬을까?

![How browser works](https://i.postimg.cc/wBn5vTxK/image.png)

![DOM and CSSOM are combined to create the Render Tree](https://i.postimg.cc/Y9cSS803/image.png)

- css: `<head>` 태그 내부에 위치

렌더 트리를 구성하기 위해서는 DOM 트리와 CSSOM 트리가 필요하다. DOM 트리는 파싱 중에 태그를 발견할 때마다 순차적으로 구성할 수 있지만 CSSOM 트리는 CSS를 모두 해석해야 구성할 수 있다. 즉, CSSOM 트리가 구성되지 않으면 렌더 트리를 만들지 못하고 렌더링이 차단된다. 이러한 이유로 CSS는 렌더링 차단 리소스라고 하며, 렌더링이 차단되지 않도록 CSS는 항상 HTML 문서 최상단(`<head>`)에 배치한다.

- js: `<body>`의 최하단에 위치

script 태그의 위치를 `<body>` 태그 내부의 최하단에 위치시키는 이유는 다음과 같은 특징들 때문이다.

**(1) 브라우저는 자바스크립트를 동기적으로 처리한다.**

브라우저의 렌더링 엔진(HTML 파서)은 script 태그를 만나면 DOM 생성을 멈추고 자바스크립트 런타임에게 제어 권한을 넘긴다. 제어 권한을 넘겨받은 자바스크립트 런타임은 script 태그 내의 자바스크립트 코드 또는 script 태그의 src attribute에 정의된 자바스크립트 파일을 로드하고 파싱하여 실행한다. 자바스크립트의 실행이 완료되면 다시 렌더링 엔진에게 제어 권한을 넘기고 DOM 생성을 중지했던 시점부터 다시 DOM 생성을 시작한다.<br />이렇게 script 태그를 만날 때마다 DOM 생성을 멈추는 Blocking이 발생하기 때문에 script 태그가 head에 있거나 body의 중간에 위치할 경우 DOM 생성을 지연시켜서 페이지 로딩 시간이 길어질 수 있다.

**(2) 자바스크립트는 브라우저가 script 태그를 만나기 전까지 생성해놓은 DOM에만 접근할 수 있다.**

자바스크립트는 DOM 트리와 CSSOM 트리를 동적으로 변경할 수 있다. 하지만 자바스크립트는 브라우저가 이미 만들어놓은 DOM에만 접근할 수 있기 때문에 만약 DOM이 완성되지 않은 상태에서 자바스크립트가 DOM을 조작한다면 에러가 발생할 수 있다.

> 참고1) `<script>` 태그에 defer나 async 속성을 명시하여 script 태그가 `<head>`나 `<body>` 중간에 포함되어 있더라도 DOM 생성을 멈추지 않게 할 수 있다. 단, 이 속성들은 브라우저 지원 범위가 한정적이므로 사용에 유의한다.

> 참고2) js를 `<head>`에 넣기도 한다.

<br />

3. 화면을 표시하기 위해 어떤 파일들이 다운로드 되는가?

- HTML 파일 (Type: document)
- CSS 파일 (Type: stylesheet)
- XMLHttpRequest API (Type: xhr)
- 각종 이미지 파일 (Type: png, jpeg, gif)
- script 파일
- icon 파일 (Type: x-icon)

<br />

4. 특정 자원의 Request Headers 와 Response Headers의 내용을 분석. (네트워크 탭 활용)

<br />

5. 화면에 보여지기 시작하는 시간은 언제인가?

<img src="https://i.postimg.cc/W4M8GqNS/2020-02-03-17-26-56.png" alt="First Paint" width="500">

**FP(First Paint)**는 흰 화면에서 화면에 무언가가 처음으로 그려지기 시작하는 시점을 말한다. 개발자 도구의 Performance 탭에서 테스트시 8.61초가 소요되었다.

<br />

6. DOMContentLoaded라는 이벤트는 언제 발생하는가? load랑은 어떤 차이점이 있는가?

<img src="https://i.postimg.cc/XYLCzR2C/image.png" alt="Time stamps" width="400">

**DOMContentLoaded event (DCL)**<br />

- HTML, CSS 파싱이 완료되어 DOM 트리와 CSSOM 트리가 준비된 시점 (렌더 트리 만들 준비 완료)
- 많은 자바스크립트 프레임워크가 자체 로직을 실행하기 전에 이 이벤트를 기다림
- 빠른 실행속도가 필요할때 적합

**Document load event**<br />

- HTML 상에 필요한 모든 리소스가 로드된 시점

<br />

7. HTML 파일 응답 받은 이후부터, 모니터화면에 보이기까지의 과정을 설명하고, 이를 암기한다.

![Loading stages of browser](https://i.postimg.cc/Z5qwVzZW/image.png)

1. Parse Stage

- (HTML 파싱 / DOM 트리 생성) 브라우저의 렌더링 엔진은 HTML 마크업을 한줄씩 파싱하면서 태그를 만날때마다 DOM 트리를 생성한다.
- (CSS 파싱)중간에 stylesheet를 만나면 CSSOM 트리를 빌드한다.
- (자바스크립트 파싱)중간에 script 태그를 만나면 자바스크립트 런타임에게 제어권을 넘겨서 자바스크립트를 파싱하도록 한다.

  > 위 과정은 동기적(Synchronous)으로 이루어진다. HTML 입장에서는 자바스크립트/CSS에 의해 HTML 파싱이 중단되는 셈이기 때문에 (블록 상태) 블록 상태의 원인이 되는 자바스크립트/CSS 리소스를 블록 리소스(Block resource) 라고도 한다.

  > 파싱이란

<br />

2. Style Stage

![making render tree](https://i.postimg.cc/bNQ1fCYq/image.png)

- DOM 트리와 CSSOM 트리를 결합하여 렌더 트리를 구성한다.
- 렌더 트리에는 페이지를 렌더링하는데 필요한 노드만 포함된다.

> 렌더링이란

<br />

3. Layout Stage

- 렌더 트리를 이루는 각 노드의 정확한 위치와 크기를 계산한다.
- 노드의 정확한 크기와 위치를 파악하기 위해 루트부터 노드를 순회한다.

<img src="https://i.postimg.cc/yd6NMxbh/2020-02-03-23-59-25.png" alt="layout" width="500">

> 렌더 트리는 자바스크립트에 의해 DOM 트리, CSSOM 트리가 변경될 때 다시 재구성된다. (DOM이 추가/삭제되거나 요소에 기하적인 영향(높이, 넓이, 위치)을 주는 CSS 속성값을 변경하는 경우) 즉, Layout Stage부터 이후 과정을 다시 수행해야 하기 때문에 Layout Stage를 **Reflow** Stage 라고도 한다.

<br />

4. Paint Stage

- 렌더 트리의 각 노드를 화면상의 실제 픽셀로 변환한다.
- 위치와 관계없는 CSS 속성(색상, 투명도, transform 등)을 적용한다.
- 픽셀로 변환된 결과는 포토샵의 레이어처럼 생성되어 개별 레이어로 관리된다. (단, 각각의 엘리먼트가 모두 레이어가 되는 것은 아니다.)

<br />

5. Composite & Render Stage

- Paint Stage에서 생성된 레이어를 합성하여 화면을 업데이트 한다.
- 이 단계가 끝나면 화면에서 웹페이지를 볼 수 있다.

<br />

---

- https://ui.toast.com/fe-guide/ko_PERFORMANCE/#%EB%A6%AC%EC%86%8C%EC%8A%A4-%EC%9A%94%EC%B2%AD-%EC%88%98-%EC%A4%84%EC%9D%B4%EA%B8%B0
- https://d2.naver.com/helloworld/59361
- https://www.html5rocks.com/en/tutorials/internals/howbrowserswork/#Introduction
- https://developers.google.com/web/fundamentals/performance/critical-rendering-path/constructing-the-object-model?hl=en
- https://calendar.perfplanet.com/2012/deciphering-the-critical-rendering-path/
