# TCP와 UDP

`TCP(Transmission Control Protocol)`와 `UDP(User Datagram Protocol)`는 전송 계층(Transport layer)에서 사용되는 프로토콜 입니다. TCP와 UDP는 패킷을 한 컴퓨터에서 다른 컴퓨터로 전달해주는 `IP 프로토콜`을 기반으로 구현되어 있고, 포트 번호를 이용하여 주소를 지정하는것과 데이터 오류검사를 위한 체크섬이 존재하는 두가지 공통점을 가지고 있지만 **정확성(TCP)을 추구할지 신속성(UDP)을 추구할지를 구분**하여 나뉩니다.

> **전송계층**은 TCP/IP에서 IP에 의해 전달되는 패킷의 오류를 검사하고 재전송 요구 등의 제어를 담당하는 계층입니다. 데이터의 전달을 담당한다고 생각하면 됩니다.

> **포트 번호**
> TCP와 UDP는 **포트 번호**라는 숫자를 이용하여 컴퓨터 안의 어떤 서비스(애플리케이션)에게 데이터를 전달하면 좋은지를 식별합니다. 포트 번호는 `0~65535`(16비트 분)까지의 숫자로 되어 있으며, 범위에 따라 용도가 정해져 있습니다. `0~1023`은 **잘 알려진 포트(well-known port)**라고 해서 웹 서버나 메일 서버 등과 같이 일반적인 서버 소프트웨어가 클라이언트의 서비스 요청을 대기할 때 사용합니다. `1024~49151`은 **등록된 포트(registered port)**로, 제조업체의 독자적인 서버 소프트웨어가 클라이언트의 서비스 요청을 대기할 때 사용합니다. `49152~65535`는 **동적 포트(dynamic port)**로, 서버가 클라이언트를 식별하기 위해 사용합니다.



### 들어가기 전 - 프로토콜 데이터 단위(PDU)

네트워크에서 사용하는 Segments같은 단어들이 운영체제의 것과 비슷해서 헷갈릴 수 있지만, 네트워크의 Segments는 `프로토콜 데이터 단위(Protocol Data Unit)` 중 하나 입니다.

**프로토콜 데이터 단위(Protocol Data Unit)는 데이터 통신에서 상위 계층이 전달한 데이터에 붙이는 제어정보**를 뜻 합니다.

모든 계층에서, 우리가 전송하는 데이터를 데이터라고 부르지 않고 **데이터 자체는 동일하지만 각 레이어를 거치면서 헤더 정보가 추가되면서 이름이 달라집니다.**

<img src="https://user-images.githubusercontent.com/79291114/128603954-ee9b8e75-149c-4512-9f55-f4e913d03cda.png" alt="PDU" style="zoom:50%;" />



### 간단한 설명

일단 간단하게 TCP와 UDP를 표현하자면 아래와 같습니다.



**TCP**

<img src="https://user-images.githubusercontent.com/79291114/128603934-944cd9af-dc68-4b03-9e81-fdc70ab9c1c8.png" alt="simple-TCP" style="zoom: 50%;" />



**UDP**

![simple-UDP](https://user-images.githubusercontent.com/79291114/128603935-ba1a90b3-0fd8-4a43-b8ee-b5652bb646e2.png)



두 그림을 보면 **신뢰성이 요구되는 애플리케이션에서는 TCP**를 사용하고 **간단한 데이터를 빠른 속도로 전송하고자하는 애플리케이션에서는 UDP**를 사용하는 것이 좋아보입니다.





## TCP(Transmission Control Protocol)

연결형 서비스를 지원하는 프로토콜로써, 호스트 간 신뢰성 있는 데이터 전달과 흐름제어를 가능하게 합니다. 일반적으로 TCP와 IP를 함께 사용하는데, IP가 데이터의 배달을 담당한다면 TCP는 전송하는 데이터의 추적 및 관리를 해줍니다. TCP는 네트워크에 연결된 컴퓨터에서 실행되는 프로그램 간에 **일련의 옥텟(데이터, 메세지, 세그먼트라는 블록 단위)를 안정적으로, 순서대로, 에러없이 교환**할 수 있게 합니다. 프로토콜 번호는 6 입니다.

> **옥텟** : 8개의 비트가 한데 모인 것을 말합니다. 초기 컴퓨터들은 1 바이트가 꼭 8 비트만을 의미하지 않았으므로, 8 비트를 명확하게 정의하기 위해 **옥텟**이라는 용어가 필요했던 것입니다. 그러나 요즘에는 **바이트하고 같은 의미**가 되었습니다.



<img src="https://user-images.githubusercontent.com/79291114/128604054-9180a86c-76a8-4176-bf6e-485a7cf779fa.png" alt="TCP" style="zoom:80%;" />



### 특징

- **연결형 서비스** : 연결형 서비스로 가상 회선 방식을 제공합니다.
  - 3-way handshaking 과정을 통해 연결을 설정
  - 4-way handshaking 을 통해 연결을 해제

> **가상 회선 방식** : 패킷이 지나갈 경로에 가상의 회선을 배정하는 방식입니다. 가상회선을 통과하는 패킷들은 모두 같은 경로를 거치므로 패킷의 전송 순서를 계속 유지하여 목적지에 도착하게 됩니다. 

- **바이트 스트림을 통한 연결** : 데이터의 경계를 구분하지 않습니다.

- **흐름제어(Flow control)** : 데이터 처리 속도를 조절하여 수신자의 버퍼 오버플로우를 방지합니다.
  - 송신하는 곳에서 감당이 안되게 많은 데이터를 빠르게 보내 수신하는 곳에서 문제가 일어나는 것을 방지
  - 수신자가 `윈도우크기(Window Size)` 값을 통해 수신량을 설정

- **혼잡제어(Congestion control)** : 네트워크 내의 패킷 수가 넘치게 증가하지 않도록 방지합니다.
  - 정보의 소통량이 과다하면 패킷을 조금만 전송하여 혼잡 붕괴 현상이 일어나는 것을 방지

- **신뢰성이 높은 전송(Reliable transmission)**
  - **Dupack-based retransmission** : 정상적인 상황에서는 ACK 값이 연속적으로 전송되어야 합니다. 그러나 ACK값이 중복으로 올 경우 패킷 이상을 감지하고 재전송을 요청합니다.
  - **Timeout-based retransmission** : 일정 시간동안 ACK 값이 수신을 못할 경우 재전송을 요청합니다.

- **전이중, 점대점 방식**
  - **전이중 (Full-Duplex)** : 전송이 양방향으로 동시에 일어날 수 있습니다.
  - **점대점 (Point to Point)** : 각 연결이 정확히 2개의 종단점(소켓)을 가지고 있습니다.
  - 멀티캐스팅이나 브로드캐스팅을 지원하지 않습니다.

> **유니 캐스팅** : 1:1로 데이터를 전달하는 통신 방식
> **멀티 캐스팅** : UDP를 기반으로 하나 이상의 송신자들이 특정한 하나 이상의 수신자들에게 패킷을 전송하는 통신 방식 (데이터를 수신 받기를 원하는 특정한 호스트들에게만 전송)
> **브로드 캐스팅** : UDP를 기반으로 자신의 호스트가 속해 있는 네트워크 전체를 대상으로 패킷을 전송하는 일대다 통신 방식 (데이터를 수신할 필요가 없는 호스트들에게도 데이터가 전송)



#### 추가 설명 - 흐름 제어 해결 방법

**Stop and Wait** : 매번 전송한 패킷에 대해 확인 응답을 받아야만 그 다음 패킷을 전송하는 방법

![Stop-and-Wait](https://user-images.githubusercontent.com/79291114/128603937-154bb5c0-d9b9-4489-bc04-cfea316083d9.png)



**Sliding Window** (Go Back N ARQ)

- 수신측에서 설정한 윈도우 크기만큼 송신측에서 확인 응답없이 세그먼트를 전송할 수 있게 하여 데이터 흐름을 동적으로 조절하는 제어 기법

- 목적 : 전송은 되었지만, acked를 받지 못한 byte의 숫자를 파악하기 위해 사용하는 protocol

- LastByteSent - LastByteAcked <= ReceivecWindowAdvertised
  - (마지막에 보내진 바이트 - 마지막에 확인된 바이트 <= 남아있는 공간) == (현재 공중에 떠있는 패킷 수 <= sliding window)
  
- **동작방식** : 먼저 윈도우에 포함되는 모든 패킷을 전송하고, 그 패킷들의 전달이 확인되는대로 이 윈도우를 옆으로 옮김으로써 그 다음 패킷들을 전송

- **Window** : TCP/IP를 사용하는 모든 호스트들은 송신하기 위한 것과 수신하기 위한 2개의 Window를 가지고 있습니다. 호스트들은 실제 데이터를 보내기 전에 `3- way handshaking`을 통해 수신 호스트의 receive window size에 자신의 send window size를 맞추게 됩니다.

  ![Sliding-Window](https://user-images.githubusercontent.com/79291114/128603936-19ecd28a-80ef-4b84-b09c-90df123e4e4a.png)



#### 추가 설명 - 오류 제어

오류 제어는 오류 검출과 재전송을 포함합니다. ARQ(Automatic Repeat Request) 기법을 사용해 프레임이 손상되었거나 손실되었을 경우, 재전송을 통해 오류를 복구합니다. ARQ 기법은 위의 흐름 제어 기법과도 관련되어 있습니다.

> **ARQ(Automatic Repeat Request)** : ARQ(Automatic Repeat Request)란 통신회선에서 착오가 발생하면 수신측은 착오의 발생을 송신측에 알리고, 송신측은 착오가 발생한 block을 재전송하는 방식으로 검출 후 재전송이라 합니다.

**Stop and Wait ARQ**

- 송신측에서 1개의 프레임을 송신하고, 수신측에서 수신된 프레임의 에러 유무 판단에 따라 ACK or NAK를 보내는 방식입니다.
- 식별을 위해 데이터 프레임과 ACK 프레임은 각각 0,1 번호를 번갈아가며 부여합니다.
- 수신측이 데이터를 받지 못했을 경우, NAK를 보내고 NAK를 받은 송신측은 데이터를 재전송합니다.
- 만약, 데이터나 ACK가 분실되었을 경우, 일정 간격의 시간을 두고 타임아웃이 되면, 송신측은 데이터를 재전송합니다.



**Go-Back-n ARQ(슬라이딩 윈도우)**

- 전송된 프레임이 손상되거나 분실된 경우 그리고 ACK 패킷의 손실로 인한 TIME_OUT이 발생한 경우, 확인된 마지막 프레임 이후로 모든 프레임을 재전송합니다.
- 슬라이딩 윈도우는 연속적인 프레임 전송 기법으로 전송측은 전송된 모든 프레임의 복사본을 가지고 있어야 하며, ACK와 NAK 모두 각각 구별해야 합니다.
- ACK : 다음 프레임을 전송
- NAK : 손상된 프레임 자체 번호를 반환



**재전송 되는 경우**

**NAK 프레임을 받았을 경우**

- 만약, 수신측으로 0~5까지의 데이터를 보냈다고 가정합니다.
- 수신측에서 데이터를 받았음을 확인하는 ACK 프레임을 중간 중간 보내게 되며, ACK 프레임을 확인한 전송측은 계속해서 데이터를 전송합니다.
- 그러나 만약 수신측에서 데이터 오류 프레임 2를 발견하고 NAK2를 전송측에 보냅니다.
- NAK2를 받은 전송측은 데이터 프레임 2가 잘못되었다는 것을 알고 데이터를 재전송합니다.

> GBn(Go-Back-n) ARQ의 재전송은 NAK(n)를 받아 n 데이터 이후의 모든 데이터를 재전송합니다.



**전송 데이터 프레임의 분실**

- GBn ARQ의 특징은 확인된 데이터 이후의 모든 데이터 프레임 재전송과 수신측의 폐기입니다.

- 수신측에서 데이터 1을 받고 다음 데이터로 3을 받게 된다면 데이터 2를 받지 못했으므로 수신측에서는 데이터 3을 폐기하고 데이터 2를 받지 못했다는 NAK2를 전송측에 보냅니다.

- NAK를 받은 전송측은 NAK(n) 데이터로부터 모든 데이터를 재전송하며 수신측은 기존에 받았던 데이터 중 NAK(n)으로 보냈던 대상 데이터 이후의 모든 데이터를 폐기하고 재전송 받습니다.

  <img src="https://user-images.githubusercontent.com/79291114/128603931-e73a1467-d82b-4eb1-8734-3af779cdd9fd.png" alt="GBn-ARQ-Frame-Lost" style="zoom: 67%;" />



**지정된 타임 아웃 내의 ACK 프레임 분실(Lost ACK)**

- 전송측은 분실된 ACK를 다루기 위해 타이머를 가지고 있습니다.
- 전송측에서는 이 타이머의 타임 아웃 동안 수신측으로부터 ACK 데이터를 받지 못했을 경우, 마지막 ACK된 데이터부터 재전송합니다.

<img src="https://user-images.githubusercontent.com/79291114/128603930-36574e51-df4c-4c11-b864-d1e901cea495.png" alt="GBn-ARQ-Frame-damage" style="zoom:67%;" />



**SR(Selective-Reject) ARQ**

- GBn ARQ의 확인된 마지막 프레임 이후의 모든 프레임을 재전송하는 단점을 보완한 기법입니다.
- SR ARQ는 손상된, 손실된 프레임만 재전송합니다.
- 그렇기 때문에 별도의 데이터 재정렬을 수행해야 하며, 별도의 버퍼를 필요로 합니다.
- 수신측에 버퍼를 두어 받은 데이터의 정렬이 필요합니다.

<img src="https://user-images.githubusercontent.com/79291114/128604216-28c4bd70-f609-4d31-b39d-6dd4c997a057.png" alt="Selective-Reject-ARQ" style="zoom:80%;" />



#### 추가 설명 - 혼잡 제어 해결 방법

송신측의 데이터는 지역망이나 인터넷으로 연결된 대형 네트워크를 통해 전달되는데, 만약 한 라우터에 데이터가 몰릴 경우, 자신에게 온 데이터를 모두 처리할 수 없게 됩니다. 이런 경우 호스트들은 또 다시 재전송을 하게되고 결국 혼잡만 가중시켜 오버플로우나 데이터 손실을 발생시키게 됩니다. 따라서 이러한 네트워크의 혼잡을 피하기 위해 송신측에서 보내는 데이터의 전송속도를 강제로 줄이게 되는데, 이러한 작업을 혼잡제어라고 합니다.

흐름제어가 송신측과 수신측 사이의 전송속도를 다루는데 반해, 혼잡제어는 호스트와 라우터를포함한 보다 넓은 관점에서 전송 문제를 다루게 됩니다.



![Congestion-Control](https://user-images.githubusercontent.com/79291114/128603927-4d01df03-7554-4f69-8189-0466d5ea7829.png)

**AIMD(Additive Increase / Multiplicative Decrease)**

- 처음에 패킷을 하나씩 보내고 이것이 문제없이 도착하면 window 크기(단위 시간 내에 보내는 패킷의 수)를 1씩 증가시켜가며 전송하는 방법
- 패킷 전송에 실패하거나 일정 시간을 넘으면 패킷의 보내는 속도를 절반으로 줄입니다.
- 공평한 방식으로, 여러 호스트가 한 네트워크를 공유하고 있으면 나중에 진입하는 쪽이 처음에는 불리하지만, 시간이 흐르면 평형상태로 수렴하게 되는 특징이 있습니다.
- 문제점은 초기에 네트워크의 높은 대역폭을 사용하지 못하여 오랜 시간이 걸리게 되고, 네트워크가 혼잡해지는 상황을 미리 감지하지 못합니다. 즉, 네트워크가 혼잡해지고 나서야 대역폭을 줄이는 방식입니다.



**Slow Start (느린 시작)**

- AIMD 방식이 네트워크의 수용량 주변에서는 효율적으로 작동하지만, 처음에 전송 속도를 올리는데 시간이 오래 걸리는 단점이 존재했습니다.
- Slow Start 방식은 AIMD와 마찬가지로 패킷을 하나씩 보내면서 시작하고, 패킷이 문제없이 도착하면 각각의 ACK 패킷마다 window size를 2배로 늘려줍니다. 
- 전송속도는 AIMD와 다르게 지수 함수 꼴로 증가합니다. 대신에 혼잡 현상이 발생하면 window size를 1씩 떨어뜨리게 됩니다.
- 처음에는 네트워크의 수용량을 예상할 수 있는 정보가 없지만, 한번 혼잡 현상이 발생하고 나면 네트워크의 수용량을 어느 정도 예상할 수 있습니다.
- 그러므로 혼잡 현상이 발생하였던 window size의 절반까지는 이전처럼 지수 함수 꼴로 창 크기를 증가시키고 그 이후부터는 완만하게 1씩 증가시킵니다.



**Fast Retransmit (빠른 재전송)**

- 빠른 재전송은 TCP의 혼잡 조절에 추가된 정책입니다.
- 패킷을 받는 쪽에서 먼저 도착해야할 패킷이 도착하지 않고 다음 패킷이 도착한 경우에도 ACK 패킷을 보내게 됩니다.
- 중간에 하나가 손실되면 송신 측에서는 순번이 중복된 ACK 패킷을 받게 됩니다. 이것을 감지하는 순간 문제가 되는 순번의 패킷을 재전송 해줄 수 있습니다.
- 중복된 순번의 패킷을 3개 받으면 재전송을 하게 됩니다. 약간 혼잡한 상황이 일어난 것이므로 혼잡을 감지하고 window size를 줄이게 됩니다.

![Fast-Retransmit](https://user-images.githubusercontent.com/79291114/128603928-00bbc52d-755b-4a6b-b6b1-f75da965b328.jpg)



**Fast Recovery (빠른 회복)**

- 혼잡한 상태가 되면 window size를 1로 줄이지 않고 반으로 줄이고 선형증가시키는 방법입니다. 이 정책까지 적용하면 혼잡 상황을 한번 겪고 나서부터는 순수한 AIMD 방식으로 동작하게 됩니다.



#### TCP의 연결 및 해제 과정

한 가지 주의할 점은 클라이언트와 서버 대신 `Active Close`와 `Passive Close`라는 표현을 사용한 것인데, 반드시 서버만 `CLOSE_WAIT` 상태를 갖는 것은 아니기 때문입니다. 서버가 먼저 종료하겠다고 `FIN`을 보낼 수 있고, 이런 경우 서버가 `FIN_WAIT1` 상태가 됩니다. 따라서, 클라이언트와 서버가 아닌 **Active Close**(또는 Initiator, 기존 클라이언트)와 **Passive Close**(또는 Receiver, 기존 서버)정도로 표현하는 것이 정확합니다.



##### TCP state (netstat 명령어를 통해 확인가능)

- **LISTEN** : Passive Close의 데몬이 떠서 접속 요청을 기다리는 상태
- **SYN-SENT :** 로컬의 Active  Close 어플리케이션이 원격 호스트에 연결을 요청한 상태
- **SYN_RECEIVED(RCVD) :** Passive Close가 원격 Active  Close로부터 접속 요구를 받아 Active  Close에게 응답을 하였지만 아직 Active  Close에게 확인 메시지는 받지 않은 상태
- **ESTABLISHED :** 3 way-handshaking 이 완료된 후 서로 연결된 상태
- **FIN-WAIT1, FIN-WAIT2 :** Passive Close에서 연결을 종료하기 위해 Active Close에게 종결을 요청하고 회신을 받아 종료하는 과정의 상태 (에러 발생으로 인해 Time Out 되면 스스로 연결을 종료)
- **CLOSE-WAIT** : Close실행을 하지 않고, TCP 포트를 사용중인 프로세스에게 종료 명령을 내리고 Close 명령을 실행할 때까지 대기 하는 상태 (처리해야 할 통신이 끝날 때 까지 기다리는 상태)
- **TIME-WAIT :** 연결은 종료되었지만 분실되었을지 모를 느린 세그먼트를 위해 당분간 소켓을 열어두고 있는 상태
- **CLOSING :** 흔하지 않지만 주로 확인 메시지가 전송도중 분실된 상태 
- **CLOSED :** 완전히 종료



##### TCP Connection (3-way handshake)

TCP 통신을 위한 네트워크 연결은 3-way handshake 이라는 방식으로 연결됩니다. 3-way handshake 방식은 서로의 통신을 위한 관문(port)을 확인하고 연결하기 위하여 3번의 요청/응답 후에 연결이 되는 것을 말합니다. (이 과정에서 가장 많은 시간이 소요되어 UDP방식보다 속도가 느려지는 주요 원인으로 지목됩니다.)

![3-way-handshake](https://user-images.githubusercontent.com/79291114/128603922-a6afd602-affd-433d-a970-806cd1fe2534.png)

1. Active Close에서 Passive Close에 연결 요청(`SYN-SENT`)을 하기위해 `SYN` 데이터를 보냅니다.
2. Passive Close에서 해당 포트는 `LISTEN` 상태에서 `SYN` 데이터를 받고 `SYN_RCV`로 상태가 변경됩니다.
3. 그리고 요청을 정상적으로 받았다는 대답(`ACK`)와 Active Close도 포트를 열어달라는 `SYN` 을 같이 보냅니다.
4. Active Close에서는 `SYN+ACK` 를 받고 `ESTABLISHED`로 상태를 변경하고 Passive Close에 `ACK` 를 전송합니다.
5. ACK를 받은 Passive Close는 상태가 `ESTABLSHED`로 변경됩니다.

**위와 같이 3번의 통신이 정상적으로 이루어지면, 서로의 포트가 ESTABLISHED 되면서 연결됩니다.**



##### TCP Disconnection (4-way handshake)

![4-way-handshake](https://user-images.githubusercontent.com/79291114/128603924-9b5a9099-a0ef-4ee4-93ab-b6ce13112fb5.png)

1. 먼저 `close()`를 실행한 Active Close가 FIN을 보내고 `FIN_WAIT1` 상태로 대기합니다.
2. Passive Close는 `CLOSE_WAIT`으로 바꾸고 응답 ACK를 전달합니다. 동시에 해당 포트에 연결되어 있는 Active Close에게 close()를 요청합니다.
3. ACK를 받은 Active Close는 상태를 `FIN_WAIT2`로 변경합니다.
4. `close()` 요청을 받은 Passive Close는 종료 프로세스를 진행하고 `FIN`을 Active Close에 보내 `LAST_ACK` 상태로 바꿉니다.
5. FIN을 받은 Active Close는 ACK를 Passive Close에 다시 전송하고 `TIME_WAIT`으로 상태를 바꿉니다. `TIME_WAIT`에서 일정 시간이 지나면 `CLOSED`됩니다. ACK를 받은 Passive Close도 포트를 `CLOSED`로 닫습니다.



### TCP 헤더 정보

응용 계층으로부터 데이터를 받은 TCP는 `헤더`를 추가한 후에 이를 IP로 보냅니다. 헤더에는 아래 표와 같은 정보가 포함됩니다.

<img src="https://user-images.githubusercontent.com/79291114/128603939-45a60889-3db9-4871-a76d-5398c522f750.png" alt="TCP-Header" style="zoom:80%;" />



| **필드**                                        | **크기(bits)** | **내용**                                                     |
| ----------------------------------------------- | -------------- | ------------------------------------------------------------ |
| 송신자(Source), 수신자(Destination)의 포트 번호 | 각 16          | TCP로 연결되는 가상 회선 양단의 송수신 프로세스에 할당되는 포트 주소 |
| 시퀀스 번호 (Sequence Number)                   | 32             | 송신자가 지정하는 순서 번호, 전송되는 바이트 수를 기준으로 증가<br />SYN = 1 : 초기 시퀀스 번호. ACK 번호는 이 값에 1을 더한 값<br />SYN = 0 : 현재 세션의 이 세그먼트 데이터의 최초 바이트 값의 누적 시퀀스 번호 |
| 응답 번호 (ACK Number)                          | 32             | 수신 프로세스가 제대로 수신한 바이트 수를 응답하기 위해 사용 |
| 데이터 오프셋 (Header Length, Data Offset)      | 4              | TCP 세그먼트의 시작 위치를 기준으로 데이터의 시작 위치를 표현(TCP 헤더의 크기) |
| 예약 필드(Reserved)                             | 4              | 사용을 하지 않지만 나중을 위한 예약 필드이며 0으로 채워져야 함 |
| 제어 비트(Flag Bit)                             | 8              | SYN, ACK, FIN 등의 제어 번호                                 |
| 윈도우 크기(Window)                             | 16             | 수신 윈도우의 버퍼 크기를 지정할 때 사용. 0이면 송신 프로세스의 전송 중지 |
| 체크섬(Checksum)                                | 16             | TCP 세그먼트에 포함되는 프로토콜 헤더와 데이터에 대한 오류 검출 용도 |
| 긴급 위치(Urgent Pointer)                       | 16             | 긴급 데이터를 처리하기 위함, URG 플래그 비트가 지정된 경우에만 유효 |
| 옵션(Option)                                    | 0~40bytes      | 세그먼트의 최대 사이즈를 지정하는 등 추가 옵션이 있을 경우 표시 |

 

### TCP 제어비트 (Flag Bit) 정보

| 종류 | 내용                                                         |
| ---- | ------------------------------------------------------------ |
| ACK  | 응답 번호 필드가 유효한지 설정할때 사용하며 상대방으로부터 패킷을 받았다는 걸 알려주는 패킷. Active  Close가 보낸 최초의 SYN 패킷 이후에 전송되는 모든 패킷은 이 플래그가 설정되어야 함 |
| SYN  | 연결 설정 요구. 동기화 시퀀스 번호. 양쪽이 보낸 최초의 패킷에만 이 플래그가 설정되어 있어야 합니다. TCP에서 세션을 성립할 때 가장먼저 보내는 패킷, 시퀀스 번호를 임의적으로 설정하여 세션을 연결하는 데에 사용되며 초기에 시퀀스 번호를 보내게 됩니다. |
| PSH  | 수신 애플리케이션에 버퍼링된 데이터를 상위 계층에 즉시 전달할 때 사용 |
| RST  | 연결의 리셋이나 유효하지 않은 세그먼트에 대한 응답용으로 사용 |
| URG  | 긴급 위치를 필드가 유효한지 설정 (긴급한 데이터는 다른 데이터에 비해 우선순위가 높음) |
| FIN  | 세션 연결을 종료시킬 때 사용되며 더 이상 전송할 데이터가 없을 때 연결 종료 의사 표시 |

- 제어비트는 해당 위치의 bit가 1이면 해당 패킷이 어떠한 내용을 담고 있는 패킷인지를 나타냅니다.

- **SYN(Synchronize Sequence Number) / 000010**
  - 연결 설정. Sequence Number를 랜덤으로 설정하여 세션을 연결하는 데 사용하며, 초기에 Sequence Number를 전송합니다.
- **ACK(Acknowledgement) / 010000**
  - 응답 확인. 패킷을 받았다는 것을 의미합니다.
  - Acknowledgement Number 필드가 유효한지를 나타냅니다.
  - 양단 프로세스가 쉬지 않고 데이터를 전송한다고 가정하면 최초 연결 설정 과정에서 전송되는 첫 번째 세그먼트를 제외한 모든 세그먼트의 ACK 비트는 1로 지정된다고 생각할 수 있습니다.
- **FIN(Finish) / 000001**
  - 연결 해제. 세션 연결을 종료시킬 때 사용되며, 더 이상 전송할 데이터가 없음을 의미합니다.



#### ACK 제어비트

- ACK는 송신측에 대하여 **수신측에서 긍정 응답**으로 보내지는 전송 제어용 비트
- ACK 번호를 사용하여 패킷이 도착했는지 확인합니다.
  - 송신한 패킷이 제대로 도착하지 않았으면 **재송신**을 요구합니다.

<img src="https://user-images.githubusercontent.com/79291114/128603926-ae630d66-ac71-4b4d-9c0e-07923ebf23ba.png" alt="ACK-Control-Bit" style="zoom:67%;" />



#### 처음 SYN 패킷을 보낼 때 Sequence Number를 난수로 사용하는 이유

Connection을 맺을 때, 사용하는 포트(port)는 유한 범위 내에서 사용하고 시간이 지남에 따라 재사용합니다. 따라서 이전에 사용한 포트 번호를 재사용할 가능성이 있습니다. 따라서 **Sequence Number가 순차적인 숫자로 전송된다면 수신측는 이전의 Connection으로부터 전송되는 패킷으로 인식할 수 있기 때문에 난수로 초기 Sequence Number를 설정**합니다.



*보통 구글링을 하면 위와 같은 설명이 많은데, 뭔가 와닿지가 않아서 외국 사이트도 찾아본 결과 아래와 같은 설명들을 볼 수 있었습니다. 개인적으로는 아래의 설명이 조금 더 와닿았던 거 같습니다.*



각 TCP 연결에는 두 개의 초기 Sequence Number(ISN)가 있습니다. 하나는 클라이언트에서 생성되고 다른 하나는 서버에서 생성됩니다. **랜덤으로 Sequence Number(ISN)를 사용하면 공격자가 새 연결을 위한 다음 ISN을 예측하고 새 세션을 가로채는 것을 방지**할 수 있습니다. 원하는 경우 트래픽 클래스당 임의 추출을 사용하지 않도록 설정할 수 있습니다.

예를 들어 설명하자면, 순차적으로 1,2,3,4 Sequence Number를 생성하게 되면 3이 실행되는 중에 4의 Sequence Number를 예측해서 악용할 수 있는 상황을 랜덤으로 Sequence Number를 생성함으로써 방지하는 것입니다.



## UDP(User Datagram Protocol)

비 연결형 서비스를 지원하는 프로토콜로써, TCP와 비교하여 호스트 간 완전성 또는 신뢰성이 없는 데이터를 전달합니다. 그러나 가상 회선을 굳이 확립할 필요가 없고 유연한 특성이 있으므로 효율적인 응용 계층의 데이터 전송이 필요한 곳에 적합합니다. 프로토콜 번호는 17 입니다.

<img src="https://user-images.githubusercontent.com/79291114/128604023-3626c9b8-efff-4d9a-ac79-6f832d571796.png" alt="UDP" style="zoom:80%;" />



### 특징

- **비 연결형 서비스** : 연결 절차를 거치지 않고 일방적으로 데이터를 전송합니다.
  - 정보를 주고 받을 때 정보를 보내거나 받는다는 신호절차를 거치지 않습니다.

- **데이터그램을 통한 연결** : 데이터의 경계를 구분합니다.

- **혼잡 제어와 흐름 제어 지원하지 않음** : UDP는 데이터 재전송과 데이터 순서 유지를 위한 작업을 하지 않습니다.

- **신뢰성 없는 전송**

UDP는 체크섬을 제외한 특별한 오류 검출 및 제어가 없기 때문에, UDP를 사용하는 프로그램 쪽에서 오류 제어 기능을 스스로 갖추어야 합니다.

> **데이터그램(Datagram)** : 사용자의 순수한 message를 다르게 부르는 말입니다.



### UDP를 사용하는 이유

간단하게 만약 전화를 하고 있다 다고 가정한다면, `"여", "보", "세", "요"`라는 4개의 데이터를 전송했는데, `"세"`를 못받았다고 다시 보내달라고 하면 `"여보요세"`가 될 것 입니다. 하지만 이런 경우에는 그냥 "여보X요"로 전달하는게 나은 상황이 될 수 있습니다. 

대표적으로 DNS가 UDP를 사용하고 있습니다. DNS는 Application layer protocol인데, 모든 Application layer protocol은 TCP, UDP 중 하나의 Transport layer protocol을 사용해야 합니다. 생각해보면, DNS는 해당 주소를 정확히 판별해야 하기 때문에 신뢰성이 있어야 할 것 같은데 왜 UDP를 사용할까요? 그 이유는 아래와 같습니다.

1. TCP가 3-way handshake를 사용하는 반면, UDP는 connection 을 유지할 필요가 없음
2. DNS request는 UDP 데이터그램에 꼭 들어갈 정도로 작음
   - DNS query는 single UDP request와 server로부터의 single UDP reply로 구성되어 있음
3. UDP는 신뢰성이 없지만, 신뢰성은 application layer에서 추가될 수 있음 (Timeout 추가나, resend 작업을 통해)

**DNS는 UDP를 53번 port에서 사용**



#### DNS에서  TCP를 사용하는 경우

Zone transfer 을 사용해야하는 경우에는 TCP를 사용해야 합니다. 또, 데이터가 512 bytes를 넘거나, 응답을 못받은 경우 TCP를 사용하게 됩니다.

> **Zone Transfer** : DNS 서버 간에 DNS 데이터베이스를 복제하는데 사용되는 방법



### UDP의 헤더 정보

UDP의 헤더는 고정 크기의 8바이트만 사용합니다. 따라서 TCP와 비교하여 헤더의 처리에 적은 리소스가 필요합니다.

<img src="https://user-images.githubusercontent.com/79291114/128603941-a79c200d-bde9-43b5-8800-1e6b1ea3df30.png" alt="UDP-Header" style="zoom:80%;" />

| **필드**                             | **크기** | **내용**                                 |
| ------------------------------------ | -------- | ---------------------------------------- |
| 송신자의 포트 번호(Source port)      | 16       | 데이터를 보내는 어플리케이션의 포트 번호 |
| 수신자의 포트 번호(Destination port) | 16       | 데이터를 받을 어플리케이션의 포트 번호   |
| 데이터의 길이(Length)                | 16       | UDP 헤더와 데이터의 총 길이              |
| 체크섬(Checksum)                     | 16       | 데이터 오류 검사에 사용                  |



## TCP vs UDP

### 공통점

| **TCP와 UDP의 공통점**              |
| ----------------------------------- |
| 포트 번호를 이용하여 주소를 지정    |
| 데이터 오류 검사를 위한 체크섬 존재 |

 

### 차이점

|                    | **TCP**                              | **UDP**                                                   |
| ------------------ | ------------------------------------ | --------------------------------------------------------- |
| **연결방식**       | 연결형 서비스                        | 비 연결형 서비스                                          |
| **패킷 교환 방식** | 가상 회선 방식(바이트 스트림)        | 데이터그램(메시지) 방식                                   |
| **전송 순서**      | 데이터가 도착하는 전송 순서 보장     | 데이터가 도착하는 전송 순서가 바뀔 수 있음                |
| **수신 여부 확인** | 수신 여부를 확인함                   | 수신 여부를 확인하지 않음                                 |
| **통신 방식**      | 1:1 통신만 가능                      | 1:1 / 1:N / N:N 통신 모두 가능                            |
| **신뢰성**         | 높음                                 | 낮음                                                      |
| **속도**           | 느림                                 | 빠름                                                      |
| **데이터 단위**    | 세그먼트                             | 데이터그램(메시지)                                        |
| **예시**           | HTTP, Email, File transfer 에서 사용 | DNS, Broadcasting (도메인, 실시간 동영상 서비스에서 사용) |

> **스트림** : 데이터의 흐름을 말합니다. 컴퓨터 네트워크에서 스트림 개념은 조금 독특합니다. 단말기 A에서 스트림 형태로 송신하고 단말기 B에서 그 스트림을 수신하도록 가정하면 이때 단말기 A에서 스트림에 넣은 데이터가 단말기 B에서 꺼낸 것과 완전히 같지 않을 수 있습니다. 하지만 단말기 A에서 보낸 것을 모두 이어 보면 단말기 B에서 꺼낸 것을 이은 것과 같습니다.

> **메시지** : 스트림과 달리 메시지는 자체적으로 데이터 시작과 끝을 구별합니다. 보내는 쪽에서 aaa, bbb, ccc를 보내면 받는 쪽에서는 aaa, bbb, ccc를 받습니다.



---

참고 : [https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/Network](https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/Network)

[https://gyoogle.dev/blog/computer-science/network/%ED%9D%90%EB%A6%84%EC%A0%9C%EC%96%B4%20&%20%ED%98%BC%EC%9E%A1%EC%A0%9C%EC%96%B4.html](https://gyoogle.dev/blog/computer-science/network/%ED%9D%90%EB%A6%84%EC%A0%9C%EC%96%B4%20&%20%ED%98%BC%EC%9E%A1%EC%A0%9C%EC%96%B4.html)

[https://gyoogle.dev/blog/computer-science/network/UDP.html](https://gyoogle.dev/blog/computer-science/network/UDP.html)

[https://github.com/WooVictory/Ready-For-Tech-Interview/blob/master/Network/TCP.md](https://github.com/WooVictory/Ready-For-Tech-Interview/blob/master/Network/TCP.md)

[https://github.com/WooVictory/Ready-For-Tech-Interview/blob/master/Network/UDP.md](https://github.com/WooVictory/Ready-For-Tech-Interview/blob/master/Network/UDP.md)

[https://github.com/WooVictory/Ready-For-Tech-Interview/blob/master/Network/3%20way%20handshake.md](https://github.com/WooVictory/Ready-For-Tech-Interview/blob/master/Network/3%20way%20handshake.md)

[https://velog.io/@hidaehyunlee/TCP-%EC%99%80-UDP-%EC%9D%98-%EC%B0%A8%EC%9D%B4](https://velog.io/@hidaehyunlee/TCP-%EC%99%80-UDP-%EC%9D%98-%EC%B0%A8%EC%9D%B4)

[https://coding-factory.tistory.com/614](https://coding-factory.tistory.com/614)

[https://www.stevenjlee.net/2020/06/29/%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-tcp-%EC%99%80-udp-tcp-vs-udp/](https://www.stevenjlee.net/2020/06/29/%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-tcp-%EC%99%80-udp-tcp-vs-udp/)

[https://hack-cracker.tistory.com/111](https://hack-cracker.tistory.com/111)

[https://coding-factory.tistory.com/613](https://coding-factory.tistory.com/613)

[https://tech.kakao.com/2016/04/21/closewait-timewait/](https://tech.kakao.com/2016/04/21/closewait-timewait/)

[https://movefast.tistory.com/36](https://movefast.tistory.com/36)

[https://github.com/WeareSoft/tech-interview/blob/master/contents/network.md#tcp%EC%99%80-udp](https://github.com/WeareSoft/tech-interview/blob/master/contents/network.md#tcp%EC%99%80-udp)

