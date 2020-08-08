# Ajax

기본적으로 HTTP프로토콜은 클라이언트쪽에서 Request를 보내고 Server쪽에서 Response를 받으면 이어졌던 연결이 끊기게 되어있다. 그래서 화면의 내용을 갱신하기 위해서는 다시 request를 하고 response를 하면서 페이지 전체를 갱신하게 된다. 하지만 **페이지 전체를 다시 로드할 때 불필요한 자원낭비/시간낭비가 발생한다.**

![synchronous](https://user-images.githubusercontent.com/42695954/60721386-b1ce1900-9f68-11e9-8bac-b27cb230cc43.png)

&nbsp; 

## Ajax란?
Ajax(Asynchronous JavaScript and XML)는 자바스크립트를 이용해서 비동기적(Asynchronous)으로 서버와 브라우저가 데이터를 교환할 수 있는 통신 방식을 의미한다.

Ajax는 HTML **페이지 전체가 아닌 일부분만 갱신**할수 있도록 XML HttpRequest객체를 통해 서버에 request 한다. 페이지 전체를 로드하여 렌더링할 필요가 없고 갱신이 필요한 일부만 JSON이나 XML형태로 로드하여 갱신하면 되므로 **빠른 퍼포먼스와 부드러운 화면 표시 효과를 기대할 수 있다.**

![asynchronous](https://user-images.githubusercontent.com/42695954/60720188-ed66e400-9f64-11e9-8ec3-55db3aeab38c.png)

&nbsp; 

### 비동기(Async) 방식이란?
비동기식 처리 모델(Asynchronous processing model 또는 Non-Blocking processing model)은 병렬적으로 태스크를 수행한다. 즉, 태스크가 종료되지 않은 상태라 하더라도 대기하지 않고 다음 태스크를 실행한다. 예를 들어 서버에서 데이터를 가져와서 화면에 표시하는 태스크를 수행할 때, 서버에 데이터를 요청한 이후 서버로부터 데이터가 응답될 때까지 대기하지 않고(Non-Blocking) 즉시 다음 태스크를 수행한다.

> 참고) [동기식 처리 모델 vs 비동기식 처리 모델](https://poiemaweb.com/js-async)

&nbsp; 

### Ajax는 그 자체가 특정한 기술은 아니다.  
하지만 HTML (또는 XHTML, Cascading Style Sheets, Javascript, The Document Object Model, XML, XSLT, XMLHttpRequest object) 를 비롯한 기존의 여러 기술을 사용하는 *새로운 접근법*이라고 할 수 있다.  
이러한 기술들을 Ajax 모델을 통해 결합하면, 웹 응용 프로그램을 빠르게 만들 수 있으며, 전체 웹 페이지를 다시 불러 오지 않은 채로 점진적 또는 부분적으로 사용자 인터페이스와 페이지 내용을 갱신할 수 있다.  
Ajax를 써서 더 빠르고, 사용자가 취하는 동작이나 요구 (예를 들면 검색어 입력, 지도 스크롤, 새로운 위치 선택, 축척 조정 등)에 더 잘 응답하는 응용 프로그램을 만들 수 있다.

&nbsp; 

## Ajax의 장단점

**장점**  
1. 웹페이지의 속도 향상
2. 서버의 처기 완료될 때까지 기다리지 않고 처리 가능
3. 서버에 데이터만 전송하면 되므로 전체적인 코드의 양이 줄어듦
4. 기존 웹에서는 불가능했던 다양한 UI가 가능함

**단점**  
1. 히스토리 관리가 안됨 (보안에 취약)
2. 연속으로 데이터를 요청하면 서버 부하가 증가할 수 있음
3. XMLHttpReqeust를 통해 통신을 하는 경우, 사용자에게 아무런 진행정보가 주어지지 않음 → 요청이 완료되지 않았는데 사용자가 페이지를 떠날 수 있음

&nbsp; 

## Jquery와의 시너지
Ajax하면 Jquery에 대한 설명을 빼놓을 수 없다. 일반 Javascript만으로 Ajax를 하게되면 코드량도 많아지고 브라우저별로 구현방법이 다른 단점이 있는데, Jquery를 이용하면 더 적은 코드량과 동일한 코딩방법으로 대부분의 브라우저에서 같은 동작을 할 수 있게 되기 때문이다. Jquery로 Ajax를 사용하면, HTTP Get방식과 HTTP Post방식 모두를 사용하여 원격 서버로부터 데이터를 요청할 수 있다. 

&nbsp; 

---

**Reference**  
* [https://poiemaweb.com/js-ajax](https://poiemaweb.com/js-ajax)
* [http://documentation.microfocus.com/help/index.jsp?topic=%2Fcom.microfocus.silkperformer.doc%2FSILKPERF-9072F816-BDWLT-AJAXOVERVIEW-CON.html](http://documentation.microfocus.com/help/index.jsp?topic=%2Fcom.microfocus.silkperformer.doc%2FSILKPERF-9072F816-BDWLT-AJAXOVERVIEW-CON.html)
* [https://coding-factory.tistory.com/143](https://coding-factory.tistory.com/143)