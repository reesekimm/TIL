# HTTP 

> HTTP에 대해 정리하기 전에, 먼저 인터넷과 웹에 대해 간단하게 개념을 잡고 가면 좋을 것 같다.  
> * [인터넷이란?](http://tcpschool.com/webbasic/intro)  
> * [웹이란?](http://tcpschool.com/webbasic/www)
> * [인터넷과 웹의 역사](https://opentutorials.org/module/1892)

&nbsp; 

## HTTP (Hypertext Transfer Protocol)

HTTP는
* 웹에서 서버와 클라이언트가 데이터를 어떠한 방식으로 주고받아야 하는지 정의해놓은 프로토콜/약속이다.  
* 반드시 지켜야 하는건 아니지만 오랜 시간동안 사용되며 검증된 시스템이다.  
* 어플리케이션 레벨의 프로토콜로 TCP/IP위에서 작동하며 어떤 종류의 데이터든지 전송할 수 있도록 설계되어 있다. HTTP로 보낼 수 있는 데이터는 HTML문서, 이미지, 동영상, 오디오, 텍스트 문서 등 여러종류가 있다.

HTTP는 기본적으로 다음과 같이 동작하는데,  
클라이언트에서 요청(request)를 보내면 서버는 해당 요청을 처리해서 응답(response)한다.

![HTTP_Steps](https://user-images.githubusercontent.com/42695954/60638982-89a4c400-9e5b-11e9-89ee-eb2d7b5d2265.png)

&nbsp; 

## HTTP Request (Client → Server)

![http_request](https://user-images.githubusercontent.com/42695954/60639429-a3dfa180-9e5d-11e9-8f9b-9a3c8bca67a5.png)

&nbsp; 

### Request Methods
* GET : 찾기
* POST : 생성하기
* PUT : 수정하기  
* DELETE : 지우기

> 참고) PATCH  
> PUT과 유사하게 요청된 자원을 수정(UPDATE)할 때 사용하는 메서드.  
> PUT의 경우 리소스 **전체**를 갱신하는 의미지만, PATCH는 리소스의 **일부**를 교체하는 의미로 사용한다.

&nbsp; 

## HTTP Responses (Server → Client)

![http_response](https://user-images.githubusercontent.com/42695954/60639473-c7a2e780-9e5d-11e9-9430-eee12741903b.png)

&nbsp; 

### Response Code (상태 코드)
* 1xx : Continue  (정보) : 요청을 받았으며 프로세스를 계속한다.
* 2xx : Success (성공) : 요청을 성공적으로 인식했고, 승낙했으며, 성공적으로 처리했다.
* 3xx : Redirection (우회) : 요청을 마치기 위해 클라이언트가 추가 작업을 해야하다.
* 4xx : Client Error (클라이언트 오류) : 요청 문법이 잘못되었거나 요청을 처리할 수 없다.
* 5xx : Server Error (서버 오류) : 서버가 명백히 유효한 요청을 수행하지 못했다. 

> 참고) [HTTP 상태 코드](https://ko.wikipedia.org/wiki/HTTP_%EC%83%81%ED%83%9C_%EC%BD%94%EB%93%9C)

&nbsp; 

---

**Reference**  

* [https://www.ntu.edu.sg/home/ehchua/programming/webprogramming/HTTP_Basics.html](https://www.ntu.edu.sg/home/ehchua/programming/webprogramming/HTTP_Basics.html)
* [https://shlee0882.tistory.com/107](https://shlee0882.tistory.com/107)
