# Client & Server

---

![client_and_server](https://user-images.githubusercontent.com/42695954/60592146-f62bae80-9dda-11e9-9a53-978b8b697a4f.png)

## ● Client (클라이언트)
네트워크를 이용하여 서버측에 자료 또는 서비스 요청을 의뢰하는 주체  

예)  
* 웹브라우저
* 어플리케이션
* 하드웨어

&nbsp; 

## ● Server (서버)
네트워크상에서 어떠한 자료 또는 서비스에 대한 접근을 관리하는 컴퓨터  
클라이언트의 요청에 응답하여 정보를 제공한다.  

예)  
* 웹서버
* 하드웨어

&nbsp; 

## ● IP(Internet Protocol) Address (IP 주소)
> protocol : 약속, 규약  

인터넷에 연결되어 있는 장치들(PC, 스마트폰, 태블릿, 서버 등)은 각각의 장치를 식별할 수 있는 고유한 주소를 가지고 있는데, 이를 IP라고 한다.  

예)  
* 115.68.24.88
* 192.168.0.1

&nbsp; 

## ● Domain (도메인)
IP 주소는 전부 숫자로 되어있기 때문에 사람이 이해하고 기억하기 어렵다.  
때문에 각각의 IP 주소에 이름을 부여할 수 있게 했는데, 이것을 도메인이라고 한다.  

예)  
* naver.com → 220.95.233.172
* daum.net → 114.108.157.19

도메인은 컴퓨터의 이름과 최상위 도메인으로 구성되어 있다. 예를들면 아래와 같다.

* opentutorials.org
> * opentutorials : 컴퓨터의 이름  
> * org : 최상위 도메인 - 비영리단체

* daum.co.kr
> * daum : 컴퓨터의 이름  
> * co : 국가 형태의 최상위 도메인을 의미  
> * kr : 대한민국의 NIC에서 관리하는 도메인을 의미

&nbsp; 

---

참고) [URI vs URL vs URN](https://mygumi.tistory.com/139)

---

&nbsp; 

## ● DNS (Domain Name Servers / Domain Name System)
우리가 어떤 사이트(서버)에 접속하기 위해서는 사실 IP 주소가 필요하다.  
그런데 위에서 언급한 것 처럼 IP주소는 전부 숫자로 이루어져있어 사람이 기억하기 힘들다.  
이때 필요한게 바로 DNS인데, 우리가 브라우저에 www.google.com과 같은 도메인 주소를 입력하면 DNS는 해당 도메인을 IP로 변환해서 사용자에게 전달해주는 역할을 한다.  
일종의 주소록인 셈.  
DNS가 있기 때문에 우리는 도메인만 알아도 원하는 사이트에 접속할 수 있다.  

&nbsp; 

**< 브라우저 주소창에 www.google.com을 입력하고 엔터를 눌렀을 때 벌어지는 과정 >**

![dns](https://user-images.githubusercontent.com/42695954/60587344-2d945e00-9dcf-11e9-8455-d7d8334e6e74.PNG)

1. 사용자가 인터넷 브라우저 주소창에 주소(www.google.com)를 입력한다.

2. 브라우저(클라이언트)가 ISP의 DNS 서버에 www.google.com의 IP 주소를 요청한다.  
> ISP(Internet Service Provider) : SKT, KT, LG U+ 같은 통신 서비스 제공 업체

3. ISP DNS에 www.google.com의 IP 정보가 없다면 다른 DNS 서버에 문의한다.

4. 요청받은 www.google.com의 IP 주소(173.194.115.96)를 사용자에게 넘겨준다.

5. 넘겨받은 IP 주소로 Http로 통신을 하기 위해 요청(Request) 메시지를 보낸다.


---

**Reference**  

* [https://opentutorials.org/course/228/1450](https://opentutorials.org/course/228/1450)
* [http://kimkitty.net/archives/291](http://kimkitty.net/archives/291)
* [http://blog.naver.com/PostView.nhn?blogId=ljsun4336&logNo=220640947315](http://blog.naver.com/PostView.nhn?blogId=ljsun4336&logNo=220640947315)