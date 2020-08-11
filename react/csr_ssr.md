# 클라이언트 사이드 렌더링 & 서버 사이드 렌더링

## 전통적인 웹 앱

전통적인 웹앱은 Round-trip model을 따른다. 브라우저가 첫 로딩 화면을 포함해서 사용자의 행위(페이지 이동, form 제출, 링크 클릭 등)가 발생할때마다 서버에 HTML을 요청하고 응답으로 받아서 표시하는 방식이다.(**서버 사이드 렌더링!**)  
모든 비즈니스 로직과 라우팅 처리, VIEW 생성을 서버가 담당하고, 브라우저는 서버에서 만든 VIEW(HTML)를 출력하는 렌더링 엔진 역할에 충실했다.

<img src="https://i.postimg.cc/9FwNs61v/image.png" alt="MPA" width="500px" />

> Round-trip model을 따르는 전통적인 웹 앱의 동작 방식 [[출처](https://marinhoarthur.com/why-isomorphic-apps)]

<br />

이 방식의 단점은 여러가지가 있지만, 일단 브라우저가 HTML을 요청하고 화면에 표시하기까지 사용자가 무작정 기다려야 하다는 점이다. 화면 전체를 새로 렌더링하는 과정에서 새로고침이 발생하기 때문에 사용자 경험을 해치기도 한다. 서버에 오버헤드가 생는것도 문제다. 위에서 언급했듯이 웹서비스를 제공하는데 필요한 대부분의 프로세스를 서버에서 처리하기 때문이다.

<br />

## SPA의 등장

데스크탑보다 성능이 낮은 모바일의 사용량이 증가하고, 웹서비스가 많은 양의 데이터를 다루게 되면서 트래픽 감소, 빠른 속도, 그리고 좋은 사용성은 매우 중요한 요소가 되었다. 기존의 방식과 다른 접근법이 필요했고 이때 등장한게 바로 SPA(Single Page Application)이다.

SPA는 전통적인 웹앱과 다른 방식으로 동작한다. 최초 로딩시 서버로부터 빈 껍데기 상태의 HTML을 응답받고, 여기에 포함된 번들링된 자바스크립트 파일 정보를 통해 스크립트 파일(SPA)을 요청 한 뒤 파싱하여 화면을 렌더링한다.

<img src="https://i.postimg.cc/MTXhHvbG/image.png" alt="SPA" width="500px" />

> SPA의 동작 방식 [[출처](https://marinhoarthur.com/why-isomorphic-apps)]

<br />

웹앱에 필요한 모든 정적 리소스(모든 페이지)를 최초 로딩시 한 번에 가져오고 (위 이미지에서 `fetch SPA` 부분)
이후에는 사용자 행위에 따라 필요한 데이터만 요청하기 때문에 `전체적인 트래픽을 감소시킬 수 있다`. 화면을 업데이트 할때도 `필요한 데이터를 비동기적으로 요청하여 화면 전체가 아닌 일부분을 업데이트 하기 때문에 사용자와 빠른 인터랙션이 가능하고 결과적으로 더 나은 사용자 경험을 제공`할 수 있게 됐다.

이제 서버는 API를 통해 JSON 파일을 응답해주는 역할에 충실하고, 클라이언트는 VIEW를 그리고 라우팅 처리를 하며, 크리티컬하지 않은 비즈니스 로직도 처리하게 됐다. 역할 분담이 이루어진 것이다.

<br />

## 클라이언트 사이드 렌더링 & 서버 사이드 렌더링

![csr and ssr](https://i.postimg.cc/ZKtrGsb0/image.png)

> [[출처](https://subicura.com/2016/06/20/server-side-rendering-with-react.html#ajax%EA%B0%80-%EC%97%86%EB%8D%98-%EC%8B%9C%EC%A0%88)]

VIEW(HTML)를 클라이언트에서 생성하면 클라이언트 사이드 렌더링이고, 반대로 전통적인 웹앱처럼 서버에서 생성하면 서버 사이드 렌더링이다.

둘의 장단점을 정리해보면 다음과 같다.

<br />

### 클라이언트 사이드 렌더링의 장단점

#### 장점

1. 초기 로딩속도는 상대적으로 느리지만 초기 로딩 이후에는 서버에 다시 HTML을 요청할 필요가 없어서 서버의 부담이 적다.

<br />

#### 단점

1. 서버 사이드 렌더링에 비해 **초기 로딩 속도가 느리다**. 초기 HTML 파싱, 자바스크립트 요청 및 응답 리소스 파싱, (경우에 따라) 추가적인 API 요청/응답이 완료되어야 화면이 렌더링된다. 결국 사용자가 처음으로 콘텐츠를 보는 속도가 느리다는 뜻이다.  
   클라이언트 사이드 렌더링은 브라우저가 자바스크립트를 실행해야만 화면이 보인다. 저사양 기기를 사용하는 사용자일수록 요청한 페이지가 느리게 보일 수밖에 없다. 서비스 사용자 중 저사양 기기를 사용하는 사람이 많거나, 네트워크 인프라가 약한 나라에서 서비스해야 한다면 서버 사이드 렌더링을 고려해야 한다.

2. **SEO(Search Engine Optimization, 검색 엔진 최적화)에 불리하다.** 그런데 구글을 제외한 다른 검색 엔진에서는 자바스크립트를 실행하지 않기 때문에 클라이언트 사이드 렌더링만 하는 사이트는 내용이 없는 사이트와 동일하게 처리된다. 크롤링 봇이 SPA 컨텐츠를 로딩하기 전 빈 HTML만 바라보기 때문.

   ```
   해결 방법

   SEO 이슈는 [prerender](https://prerender.io/)를 이용하면 어느정도 극복 가능하다.
   ```

<br />

### 서버 사이드 렌더링의 장단점

#### 장점

1. **초기 렌더링 성능을 개선할 수 있다.** 서버 사이드 렌더링은 자바스크립트 파일 다운로드가 완료되지 않은 시점에도 HTML상에 사용자가 볼 수 있는 콘텐츠가 있기 때문에 대기시간이 최소화되고 이로 인해 사용자 경험도 향상된다.

2. **SEO에 유리하다.** 검색 엔진이 웹앱의 페이지를 원활하게 수집할 수 있기 때문.

<br />

#### 단점

1. **서버 리소스를 많이 사용한다.** 특히 렌더링 연산에C CPU가 많이 사용돼서 특정 시점에 트래픽이 몰리면 모든 요청을 처리할 수 없다. 높은 트래픽에 대응해야 한다.

   ```
   해결 방법

   - 평상시에는 서버 사이드 렌더링을 하다가 서버 부하가 일정 수준을 넘어가면 클라이언트 사이드 렌더링으로 전환한다. 단, SEO가 중요한 서비스라면 서버 사이드 렌더링을 유지하는게 좋다.

   - 데이터 의존성이 전혀 없는 페이지는 빌드할때 미리 렌더링 해놨다가 요청이 들어왔을 때 정적 페이지를 응답해주면 되기 때문에 서버 리소스를 절약할 수 있다.

   - 데이터 의존성이 낮은 페이지는 서버 사이드 렌더링을 일부만 하는 방식으로 성능 문제를 해결할 수 있다. 데이터 의존성이 있는 부분만 클라이언트 사이드에서 렌더링하도록 설계한다.

   - 데이터 의존도가 높은 페이지더라도 그 데이터가 자주 변하지 않는 페이지라면 서버 사이드 렌더링 결과를 캐싱해서 사용할 수 있다.
   ```

2. 코드의 복잡성이 올라갈 수 있다.

<br />

## 정리

클라이언트 사이드 렌더링이든 서버 사이드 렌더링이든 정말 필요한 일부 페이지에 적용하는 것이 성능, 호환성, 유지보수 측면에서 나은 결과를 보여준다. 프로젝트/서비스 상황에 맞춰 어떤 방식이 더 좋은지 충분히 고민해보고 적재적소에 적용하자.

<br />

---

- [왜 React와 서버 사이드 렌더링인가?](https://subicura.com/2016/06/20/server-side-rendering-with-react.html#ajax%EA%B0%80-%EC%97%86%EB%8D%98-%EC%8B%9C%EC%A0%88)
- [SPA & Routing](https://poiemaweb.com/js-spa)
- [Why Isomorphic Apps](https://marinhoarthur.com/why-isomorphic-apps)
- [Isomorphic JavaScript](https://www.oreilly.com/library/view/building-isomorphic-javascript/9781491932926/ch01.html)
- 실전 리액트 프로그래밍 (인사이트)
- 리액트를 다루는 기술 (길벗)

---

더 알아볼만한 내용

- prerender vs SSR --- SEO 개선에 어떤 방법이 더 좋을까?