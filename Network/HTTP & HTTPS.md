## HTTP와  HTTPS

HTTP, HTTPS 프로토콜을 알아보기 전 URL 주소체계에 대해 알아보자.

온라인 쇼핑몰등 다양한 인터넷 사이트는 모두 저마다 접속 가능한 URL을 가지고 있다.

URL이란 Uniform Resource Locator로 네트워크 상에서 자원이 어디있는지를 알려주기 위한 규약이다.

URL의 구성은 아래와 같다.

![image-20210729160414926](https://user-images.githubusercontent.com/77064907/127737028-5e5bc1f1-9f22-4eb8-88d0-c566fa078d9c.png)

- **프로토콜** : 우리가 가장 많이 사용하는 HTTP는 Hyper Text Transfer Protocol이라는 어려운 이름의 약자인데 쉽게 설명하면 웹브라우저와 웹서버 사이에서 웹 문서와 그것을 구성하는 자원을 전송하기 위한 프로토콜이다. 요즘은 HTTPS를 기본 프로토콜로 쓰는데, (https를 지원하지 않는 웹사이트는 여전히 http를 쓴다) HTTP에 보안이 강화된 버전이다.

- **호스트** : URL에서 웹서버의 위치를 지정한다. 도메인 이름(예: www.example.com)이나 IP 주소(예: 127.0.0.1)를 사용할 수 있다.

  도메인 이름 뒤에 오는 :80는 포트번호다. 웹서버에서 자원을 접근하기 위해 사용하는 관문(gate)을 가리키는데, 표준 HTTP 포트는 80번 HTTPS는 443이다. 표준 포트를 사용한다면 포트번호는 보통 생략한다.

- **경로** : 웹서버에서 자원에 대한 경로다. 실제 물리적 경로가 아니고 웹서버에서 추상화한 경로다(물론 웹서버 설정에서 물리적 경로를 추상화 경로로 사용할 수 있다).

- **매개변수 (Query 혹은 Parameter)** : 웹서버에 보내는 추가 파라메터다. 파라메터 하나는 키와 값으로 이루어져있다. 파라메터가 여러개면 &문자로 구분한다. 키와 값은 = 문자로 구분한다. 웹서버에서 어떤 파라메터를 실제로 지원하는지는 웹서버마다 다르다. 웹서버는 지원하지 않는 파라메터는 무시한다.

- **부분 식별자 (Fragment identifier)** : URL이 지정하는 자원의 세부 부분을 지정할때 쓴다. 부분 식별자가 없어도 그 앞에 오는 URL만으로도 웹의 어떤 자원을 정확히 지정할 수 있다. 부분 식별자는 자원 안에서 어떤 부분을 가르키는 역할을 한다. 부분 식별자가 쓰이는 대표적인 사례는 위키피디어 백과사전이다. 부분 식별자를 세부항목에 대한 책갈피로 쓸 수 있어서 어떤 글에서 특정 항목으로 바로 이동할 수 있다.

  예) https://ko.wikipedia.org/wiki/대한민국#문화 : 위키피디어 “대한민국” 문서에서 “문화” 세부 주제로 바로 갈 수 있다.



## 1. HTTP ( HyperText Transfer Protocol )란?


텍스트 기반의 통신 규약으로 **인터넷에서 데이터를 주고받을 수 있는 프로토콜**이다. 이렇게 규약을 정해두었기 때문에 모든 프로그램이 이 규약에 맞춰 개발해서 서로 정보를 교환할 수 있게 되었다.

클라이언트 즉, 사용자가 브라우저를 통해서 어떠한 서비스를 url을 통하거나 다른 것을 통해서 요청(request)을 하면 서버에서는 해당 요청사항에 맞는 결과를 찾아서 사용자에게 응답(response)하는 형태로 동작한다.

- 요청 : client -> server
- 응답 : server -> client

HTML 문서만이 HTTP 통신을 위한 유일한 정보 문서는 아니다.
Plain text로 부터 JSON 데이터 및 XML과 같은 형태의 정보도 주고 받을 수 있으며, 보통은 클라이언트가 어떤 정보를 HTML 형태로 받고 싶은지, JSON 형태로 받고 싶은지 명시해주는 경우가 많다.

### HTTP 의 문제점

- HTTP 는 평문 통신이기 때문에 도청이 가능하다.
- 통신 상대를 확인하지 않기 때문에 위장이 가능하다.
- 완전성을 증명할 수 없기 때문에 변조가 가능하다.

*위 세 가지는 다른 암호화하지 않은 프로토콜에도 공통되는 문제점들이다.*

### TCP/IP 는 도청 가능한 네트워크이다.

TCP/IP 구조의 통신은 전부 통신 경로 상에서 엿볼 수 있다. 패킷을 수집하는 것만으로 도청할 수 있다. 평문으로 통신을 할 경우 메시지의 의미를 파악할 수 있기 때문에 암호화하여 통신해야 한다.

#### 보안 방법

1. 통신 자체를 암호화 `SSL(Secure Socket Layer)` or `TLS(Transport Layer Security)`라는 다른 프로토콜을 조합함으로써 HTTP 의 통신 내용을 암호화할 수 있다. SSL 을 조합한 HTTP 를 `HTTPS(HTTP Secure)`이라고 부른다.
2. 콘텐츠를 암호화 한다. 말 그대로 HTTP 를 사용해서 운반하는 내용인, HTTP 메시지에 포함되는 콘텐츠만 암호화하는 것이다. 암호화해서 전송하면 받은 측에서는 그 암호를 해독하여 출력하는 처리가 필요하다.



## **2. HTTPS란?**

HyperText Transfer Protocol over Secure Socket Layer, HTTP over TLS, HTTP over SSL, HTTP Secure 등으로 불리는 HTTPS는 HTTP에 데이터 암호화가 추가된 프로토콜이다. HTTPS는 HTTP와 다르게 443번 포트를 사용하며, 네트워크 상에서 중간에 제3자가 정보를 볼 수 없도록 공개키 암호화를 지원하고 있다.

### 암호화 기법



### symmetric-key (대칭키)

![img](https://mblogthumb-phinf.pstatic.net/MjAxODExMjZfMjQw/MDAxNTQzMTk4OTUwODA3.a87jPoqAAg6gLgwq2Axu_Z_AwHTupAA7yR_2rL6JgbMg.mF0Ubd05wndMkThOaisppnm7DsdMY0haRSU8wWO-ZXEg.PNG.chodahi/image.png?type=w800)

하나의 비밀키를 양쪽 (Client - Server) 가 모두 같이 사용하는 방식

- **대칭키의 단점**
  - A와 B, 두 사람만 통신을 하고 있는데 C과 D도 통신이 필요해졌다고 가정해보자.
  - 만약 C과 D가 기존에 A와 B이 공유했던 대칭키(비밀키)를 사용하게 된다면,
  - A가 B에게 전달하는 메시지를 C 또는 D가 복호화 가능하게 된다.
  - 이는 문제가 될 수 있다.
  - 따라서, 대칭키는 데이터를 전송하는 쌍마다 추가로 필요하게된다.
  - **대칭키 알고리즘은 빠르고 기밀성은 좋지만, 키 관리가 어렵다**
  - **그래서 나온 방법이 공개키 알고리즘이며, 또 다른 이름은 비대칭키 알고리즘이다.**



**비대칭 키 (공개키/개인키)**

![img](https://mblogthumb-phinf.pstatic.net/MjAxODExMjZfMjQ4/MDAxNTQzMTk5MjkxMjg5.hgyDX0eeJFfTy-LzLODlotK_ebFEMyJlwbxL7c-AkUMg.dVdhZiAGB2DFjXYhH3VNrODn66GgvtZXo_FPSUS0JGsg.PNG.chodahi/image.png?type=w800)

- 암호화 할 때 사용하는 키와 복호화 할 때 사용하는 키가 다른 경우를 말한다. AES
  타인에게 절대 노출되어서는 안되는 개인키(비밀키)를 토대로 만든 공개키가 쌍을 이룬 형태이다.
  2개의 키 사용
   \- 공개키(Public Key) : 사람들에게 공개된 키이며 정보를 암호화 할 수 있다. 
   \- 개인키(Private Key) : 사용자만 알고 있는 암호를 풀 수 있는 키 

- **문제점**

  - 위에서 가정한 조건은 A와 B가 각자의 개인키와 공개키를 가지고 있고

  - A가 B에게 메세지를 전송한다고 했을 때, B이 자신의 공개키를 내려줬다고 가정한다.

  - 하지만 여기에는 허점이 존재한다.

  - B가 내려준 공개키가 정말 B의 공개키인지! 위조는 아닌지! A는 알 수가 없다.

  - **CA를 통해 이 문제를 해결한다.**

    

## **SSL**(Secure Socket Layer)

`HTTPS`는 SSL 의 껍질을 덮어쓴 HTTP 라고 할 수 있다. 즉, HTTPS 는 새로운 애플리케이션 계층의 프로토콜이 아니라는 것이다. HTTP 통신하는 소켓 부분을 `SSL(Secure Socket Layer)` or `TLS(Transport Layer Security)`라는 프로토콜로 대체하는 것 뿐이다. HTTP 는 원래 TCP 와 직접 통신했지만, HTTPS 에서 HTTP 는 SSL 과 통신하고 **SSL 이 TCP 와 통신** 하게 된다.

SSL 프로토콜은 Netscape 사에서 웹 서버와 브라우저 사이의 보안을 위해 만들어졌다. CA(Certificate Authority)라 불리는 서드 파티로부터 서버와 클라이언트의 인증을 하는데 사용된다.

**애플리케이션 서버를 운영하는 기업은 CA를 통해 인증서를 만든다.**

1. 애플리케이션 서버를 운영하는 기업은 HTTPS 적용을 위해 공개키와 개인키를 만든다.
2. 신뢰할 수 있는 CA 기업을 선택하고 인증서 생성을 요청한다.
3. CA는 서버의 공개키, 암호화 방법 등의 정보를 담은 인증서를 만들고 해당 CA의 개인키로 암호화하여 서버에 제공한다.
4. 클라이언트가 SSL로 암호화된 페이지(https://)를 요청시 서버는 인증서를 전송한다.
5. 인증서에 포함된 내용
   - 서버측 공개키(public key)
   - 공개키 암호화 방법
   - 인증서를 사용한 웹서버의 URL
   - 인증서를 발행한 기관 이름

**클라이언트와 서버의 통신 흐름 과정**

1. 클라이언트가 SSL로 암호화된 페이지를 요청한다.
2. 서버는 클라이언트에게 인증서를 전송한다.
3. 클라이언트는 인증서가 신용이 있는 CA로부터 서명된 것인지 판단한다. 브라우저는 CA 리스트와 해당 CA의 공개키를 가지고 있다. 공개키를 활용하여 인증서가 복호화가 가능하다면 접속한 사이트가 CA에 의해 검토되었다는 것을 의미한다. 따라서 서버가 신용이 있다고 판단한다. 공개키가 데이터를 제공한 사람의 신원을 보장해주는 것으로 이러한 것을 **전자 서명** 이라고 한다.
4. 클라이언트는 CA의 공개키를 이용해 인증서를 복호화하고 서버의 공개키를 획득한다.
5. 클라이언트는 서버의 공개키를 사용해 랜덤 대칭 암호화키, 데이터 등을 암호화하여 서버로 전송한다.
6. 서버는 자신의 개인키를 이용해 복호화하고 랜덤 대칭 암호화키, 데이터 등을 획득한다.
7. 서버는 랜덤 대칭 암호화키로 클라이언트 요청에 대한 응답을 암호화하여 전송한다.
8. 클라이언트는 랜덤 대칭 암호화키를 이용해 복호화하고 데이터를 이용한다.

![img](https://user-images.githubusercontent.com/33534771/75338777-a1d9bf80-58d2-11ea-9754-809110475c89.png)

## **3. HTTP vs HTTPS**


HTTP는 암호화가 추가되지 않았기 때문에 보안에 취약한 반면, HTTPS는 안전하게 데이터를 주고받을 수 있다.

하지만 HTTPS를 이용하면 암호화/복호화의 과정이 필요하기 때문에 HTTP보다 속도가 느리다. (암호화/복호화 과정중에는 많은 CPU 리소스를 요한다.) 

또한 HTTPS는 인증서를 발급하고 유지하기 위한 추가 비용이 발생한다.

그렇다면 언제 HTTP를 쓰고, 언제 HTTPS를 쓰는 것이 좋겠는가?

개인 정보와 같은 민감한 데이터를 주고 받아야 한다면 HTTPS를 이용해야 하지만, 단순한 정보 조회 등만을 처리하고 있다면 HTTP를 이용하면 된다.



------
**Ref** 

url이란 - https://www.betterweb.or.kr/blog/url%EC%9D%B4%EB%9E%80/ 



HTTP와 HTTPS의 차이점 -

https://mangkyu.tistory.com/98

https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/Network#http%EC%99%80-https



SSL - https://www.websecurity.digicert.com/security-topics/what-is-ssl-tls-https

https://github.com/WooVictory/Ready-For-Tech-Interview/blob/master/Network/HTTP%2C%20HTTPS.md



암호화 프로토콜 - https://wooono.tistory.com/106





