6계층 그리고 7계층은 흔히들 응용 계층이라 5, 6, 7계층을 통칭하는 것으로 불립니다.

OSI 7계층에서는 6계층은 표현 계층, 7계층은 응용 계층이나 TCP/IP 4 계층에서는 응용 계층이라고 불린다고 볼 수 있습니다.

## 표현 계층 → 6계층

---

표현 계층 ( Presentation Layer ) 은 데이터를 어떻게 표현할지 정하는 계층입니다. 일종의 확장자 라고들합니다. 간단하게 말하면 6계층은 응용프로그램 혹은 네트워크를 위해 데이터를 ‘표현’하는 계층에 해당합니다. 데이터를 안전하게 주고 받기 위해 암호화하고 복호화 하는 과정이 필요한데 이러한 과정이 바로 표현 계층인 6계층에서 이루어집니다. 

여기서 "**데이터의 표현과 암호화 및 코드 간의 번역을 담당"** 이라는 부분에 집중해보겠습니다.

---

시스템 A는 ASCII 코드를 사용한다.

→ 시스템 B는 EBCDIC 코드를 사용한다. 

→ 시스템 A에서 OSI 표준 방식으로 변경하여 처리 후 시스템 B로 전송

→ 시스템 B에서는 EBCDIC 코드를 사용하는 자신의 환경에 맞게 OSI 표준으로 받은 코드를 재구성한다.

---

위와 같이 처리되는 방식을 ASN.1 ( Abstract Syntax Notation 1 ) 이라고 합니다. 이는 통일성을 가지게 하여 계층 간에 다른 표현을 서로 인식하게끔 만들며 데이터 압축 및 암호화 기능을 수행합니다.

데이터 압축 :: 시스템 간 전송되는 데이터의 용량을 줄여주는 것

암호화 :: 평문을 특정 키를 사용하여 암호문으로 만들어, 탈취당하는 경우에 이해가 어렵도록 하는 것

표현 계층에서는 헤더 정보에 데이터 암호화 방식과 압축 방식에 대한 설명을 붙입니다. 대표적으로 ASCII와 UTF-8 프로토콜이 있습니다. 

![Untitled](https://user-images.githubusercontent.com/54073761/132977965-6f568561-5bc2-46ec-a096-fb05dacdc7f9.png)

## 응용 계층 → 7계층

---

사용자와 가장 가까운 계층이 바로 응용 계층입니다. 우리가 사용하는 응용 서비스나 프로세스가 바로 응용계층에서 동작합니다. 구글의 크롬과 같은 브라우저나 스카이프, 아웃룩 등의 응용프로그램이 이 응용 계층에서 동작합니다. HTTP, SMTP, FTP가 대표적인 응용 계층에 해당하는 프로토콜입니다.  7계층 서비스들은 각 고유의 포트번호를 가지고 있다는 것 또한 기억해두면 좋을 것 같습니다.

운영체제는 전송 계층에서 제공하는 API를 활용하여 네트워크 통신이 가능한 API를 제공하는데, 이를 소켓 API라고 합니다. 응용 계층인 7계층에서는 소켓 프로그래밍을 통해 데이터를 송/수신합니다. 여기서, 각 프로그램마다 개별적인 데이터 규격을 만들어 데이터 송/수신에 사용할 수 있으며 인코딩/디코딩 또한 자체적으로 수행할 수 있습니다. 

## TCP/IP 에서의 응용계층

---

궁극적으로는 엔드 유저와 가장 가깝게 위치하고 있는 계층이라고 볼 수 있습니다. 5계층인 세션 계층을 포함하여 표현 계층인 6계층 그리고 응용 계층인 7계층을 일괄적으로 TCP/IP에서 응용 계층이라고 칭합니다. 

![Untitled](https://user-images.githubusercontent.com/54073761/132977964-a1890da9-1b39-448a-b17e-48a89094611e.png)

 " **신뢰성 있는 데이터를 보장 "** 이것이 결국 응용 계층에서의 주된 목표가 아닌가하고 생각해볼 수 있습니다.

앞서 이야기했듯이 응용 계층은 사용자에게 편리한 서비스를 경험 시키고자 응용 환경을 제공하기 위함을 목표로합니다. 소켓이 하나의 예시가 될 수 있듯이 응용 계층의 구현은 프로그램 환경에서 이루어지면서 운영체제에서 제공되는 4번째인 응용 계층의 인터페이스를 사용하여 통신 기능을 구현합니다. 응용 계층은 하나의 서버 프로그램이 여러 엔드 유저들에게 응용 서비스를 제공하는 클라이언트-서버 모델이 대표적입니다.

이때, 가장 우선 비연결형 / 연결형 서비스 중 어떤 방식을 사용할 것인지 우선 택하는 것이 우선되어야 한다.

[#] TCP : 신뢰성 ( 안전 ) 이 높지만 상대적으로 속도가 떨어짐.

[#] UDP : 비연결형으로 전송 속도가 비교적 빠르지만, 데이터 분실이나 비순서 도착등의 단점을 가지고 있다.

### Reference

---

- [https://reakwon.tistory.com/59](https://reakwon.tistory.com/59)
- [https://www.sharedit.co.kr/posts/7482](https://www.sharedit.co.kr/posts/7482)
- [https://leejoongwon.tistory.com/47](https://leejoongwon.tistory.com/47)
- [https://mommoo.tistory.com/107](https://mommoo.tistory.com/107)
- [https://yoeubi.github.io/network/Frontend-TCP-IP-5](https://yoeubi.github.io/network/Frontend-TCP-IP-5)
- [https://freloha.tistory.com/36](https://freloha.tistory.com/36)
- [https://copycode.tistory.com/111](https://copycode.tistory.com/111)