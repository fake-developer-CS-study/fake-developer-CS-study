# OSI  7  Layer - Network Layer  (Layer 3)

---

## 역할 및 특징

- 하나의 컴퓨터에서 다른 컴퓨터에게 패킷을 전달하는 호스트간의 전달 책임을 지는 계층

- **세그먼트를 네트워크 패킷으로 분할하고 수신 측에서 패킷을 재조립**

- 물리적 네트워크에서 **최적의 경로를 찾아 패킷을** **라우팅**하는 것 이다.

- 네트워크 계층은 네트워크 주소(일반적으로 인터넷 프로토콜 주소)를 사용하여 패킷을 대상 노드로 라우팅한다.

  

1. Addressing(주소지정)
   - 전 세계적으로 통신하기 위해 각 장치를 구별하기 위해 ip 주소를 사용하게 되는데 이 주소를 **네트워크 계층에서 지정**하게 된다.
     - **DNS** 도메인 네임 시스템(Domain Name System, **DNS**)은 호스트의 도메인 이름을 호스트의 네트워크 주소로 바꾸거나 그 반대의 변환을 수행할 수 있도록 하기 위해 개발되었다. 특정 컴퓨터(또는 네트워크로 연결된 임의의 장치)의 주소를 찾기 위해, 사람이 이해하기 쉬운 도메인 이름을 숫자로 된 식별 번호(IP 주소)로 변환해 준다. 도메인 네임 시스템은 흔히 "전화번호부"에 비유된다. 인터넷 도메인 주소 체계로서 [TCP/IP](https://ko.wikipedia.org/wiki/TCP/IP)의 응용에서, www.example.com과 같은 주 컴퓨터의 도메인 이름을 192.168.1.0과 같은 IP 주소로 변환하고 라우팅 정보를 제공하는 분산형 [데이터베이스](https://ko.wikipedia.org/wiki/데이터베이스) 시스템이다.
2. Packetizing(패킷화)
   - 상위 계층 프로토콜로부터 수신된 패킷에 네트워크 계층 헤더를 덧붙여 새로운 데이터그램(패킷)을 만들어낸다.
   - **인캡슐레이션**(Encapsulation)이라고도 한다.
3. Routing(라우팅)
   - 네트워크층은 패킷이 근원지에서 목적지까지 갈 수 있도록 경로를 라우팅해야한다. 즉 두 호스트가 통신을 위한 길을 찾는 일을 가리킨다. 물리적인 네트워크는 네트워크와 길 안내를 해주는 라우터의 조합이다. 네트워크 층은 가능한 모든 경로 중 가장 좋은 경로를 찾는 역할을 수행한다. 가장 좋은 경로를 정의하는 구체적인 규칙이 필요한데 그것이 라우팅 프로토콜이다.
4. Fowarding(포워딩)
   - 라우팅이 패킷을 옮기는 길을 찾는 일이라면, 패킷을 옮기는 일 자체를 포워딩이라고 한다. 즉 라우터에 패킷이 도착했을 때 라우터가 취하는 행동으로 정의할 수 있다. 라우터가 패킷을 어느 경로로 이동시켜야 할지를 결정하기 위해 의사결정 테이블을 사용하는데 그것이 **포워딩 테이블(forwarding table)** 혹은 **라우팅 테이블(routing table)** 이라 불린다.
5. Fragmenting(단편화)
   - 네트워크 계층은 송신을 위해 패킷을 데이터 링크 계층으로 내려보내는데 일부 데이터 링크 계층 기술이 송신 할 수 있는 메세지 길이를 제한하기 때문에 네트워크 계층에서 보내고자하는 데이터의 크기가 너무 크면, 네트워크 계층은 패킷을 단편화하여 데이터 링크 계층으로 보내고, 단편화된 패킷을 목적지 장비의 네트워크 계층에서 재조합한다. 대표적으로 IP프로토콜이 있다.(IP 단편화)

---



## 데이터 송수신 흐름 중 Network layer에서의 동작

![image-20210826184037639](https://user-images.githubusercontent.com/77064907/131244670-e3bd22bf-1baa-4a5d-bf3f-bb360d9b35f4.png)

- Encapsulation(step 3) -   IP Header를 Segment에 포함시킨다. 여기서 붙는 Header의 정보는 송신 장비와 수신 장비의 IP 정보를 포함하고있다. UDP 프로토콜로 전송 할 경우 3계층에서 Data를 단편화해준다 (기준 : [^MTU] 단위)
- Decapsulation(step 8) -  IP Header를 검사한다. Header 정보 중 Destination MAC address 값이 자신의 MAC 주소와 일치하는지 확인 후 일치하다면 상위계층으로 전송합니다.

## IP Header의 구조 (IPv4 기준)

![image-20210826191317510](https://user-images.githubusercontent.com/77064907/131244685-9a7d0c2e-c078-4758-a009-b893d1cab778.png)

1) Version  (4 bits) - 현재로는 버젼 4 (IPv4)를 사용  

2) Header Length(HLEN) (4 bits) - 헤더의 길이 32비트(4 바이트) 워드 단위로 헤더 길이를 표시

3) Type of Service (ToS) Flag (8 bits)  - 요구되는 서비스 품질을 나타냄(통신효율, 신뢰성의 우선순위를 지정 할 수 있음)

4) Total Packet Length (16 bits)  - IP 헤더 및 데이터를 포함한 IP 패킷 전체의 길이를 바이트 단위로 길이를 표시

5) Fragment Identifier  (16 bits) - ①각 조각이 동일한 데이터그램에 속하면 같은 일련번호를 공유함

6) Fragmentation Flag  (3 bits) - ②분열의 특성을 나타내는 플래그 

7) Fragmentation Offset (13 bits) - ③조각나기 전 원래의 데이터그램의 8 바이트 단위의 위치

  ※ 위 3개의 필드 (Fragment Identifier,Fragmentation Flag,Fragmentation Offset) (①,②,③)는 IP 단편화(조각화,분열)과 재배열과 관련된 필드임   

8) TTL, Time To Live (8 bits) - IP 패킷 수명 ( 라우터의 남은 수를 나타냄 각 라우터를 거칠 때 마다 감소된다. 값이 0이되면 그 데이터그램은 폐기된다.)

9) Protocol Identifier  (8 bits)  - 어느 상위계층 프로토콜이 데이터 내에 포함되었는가를 보여줌 
        ICMP -> 1,  IGMP -> 2,  TCP -> 6,  EGP -> 8,  UDP -> 17,  OSPF -> 89 등

10) 헤더 체크섬  (16 bits) - 헤더에 대한 오류검출

11) Source IP Address  (32 bits) - 발신처 IP 주소

12) Destination IP Address  (32 bits) - 목적지 IP 주소

13)  IP 헤더 옵션 (선택옵션)  - 경로배정 및 보안 등과 같은 제어 기능에 사용되는 부가 정보



[IPv4 IPv6 헤더 비교]: http://www.ktword.co.kr/test/view/view.php?m_temp1=5185

---

## 라우팅 테이블 

![image-20210826192848979](https://user-images.githubusercontent.com/77064907/131244780-ba1bd83a-8fc2-4699-b978-e9564121d4bb.png)

라우팅 테이블은 라우터가 어떤 경로를 찾을 때 사용하는 것이고, 이것은 사용하는 라우터의 프로토콜에 따라 달라지며, 또 라우터는 항상 최적의 경로를 찾아 이것을 라우팅 테이블에 유지하고 있다는 것이다. 

라우터는 자기의 라우팅 테이블에다가 어떤 지도 정보를 가져다 놓는다. 길을 찾아주고, 또 그 길이 끊어지면 다른 길을 찾아주는 역할을 한다. 

 라우터가 여러 가지 정보를 종합해서 얻어낸 네트워크에 대한 지도이다. 즉 그림에서 보는 것처럼 어떤 목적지에 가기 위해서는 어떤 경로를 이용해서 가야 된다라고 써놓은 정보이다.

라우터에게 라우팅 테이블은 네비게이션과도 같다.

---

## IP Fragmentation (IP 단편화)

패킷을 전송하고자 하는데 패킷의 크기가 MTU(Maximum Transmission Unit)을 초과하면 한번에 전송할 수 없다.

패킷을 MTU 이하의 조각으로 분할하는 것을 단편화(Fragmentation), 분할된 조각을 단편(Fragment)이라고 한다.

   즉, 거치는 라우터 마다 전송에 적합한 데이터링크계층 프레임으로 변환이 필요하다.

**IPv4 에서의 단편화**

- IPv4에서는 발신지 뿐만 아니라 중간 라우터에서도 IP 단편화가 가능하다.

**IPv6 에서의 단편화**

- IPv6에서는 IP 단편화가 발신지에서만 가능하다.

- IPv6에서는 라우팅 처리 효율을 높이기 위해, 가급적 IP 단편화를 필요없도록 한다.

- Ipv4와 달리, 기본 헤더 상에 단편화 제어 관련 필드를 두지 않고, 단편화 확장 헤더를 통해 단편화한다.

**재조립(Reassembly)은 항상 최종 수신지에서만 가능하다.**



**MTU(Maximum Transmission Unit)**
MTU는 최대 전송 단위로서 TCP/IP Network 등과 같이 패킷 또는 프레임 기반의 네트워크에서 전송될 수 있는 최대 크기의 패킷 또는 프레임을 가리키며 대개 옥텟을 단위로 사용한다.

MTU가 너무 크면

- 큰 크기의 패킷을 처리할 수 없는 라우터를 만나면 재전송을 해야하는 경우가 생길 수 있다.

MTU가 너무 작으면

- 상대적으로 헤더 및 송수신 확인에 따르는 오버헤드가 커진다.



### **단편화 예시**

- 4000바이트의 패킷을 전송하려고 하는데 MTU가 1500이다.
- 패킷은 아래와 같이 분할된다.

| 순서 | 페이로드 | 헤더 | More Flag | offset |
| ---- | -------- | ---- | --------- | ------ |
| 1    | 1480     | 20   | 1         | 0      |
| 2    | 1480     | 20   | 1         | 185    |
| 3    | 1020     | 20   | 0         | 370    |

- 각 단편 모두 헤더를 가져야 하기 때문에 최대 1480 Bytes로 분할 될 수 있다.

- **플래그(Frag)**

  - 데이터그램의 상태나 진위를 나타내기 위한 변수

  - 데이터그램의 헤더에 들어가는 플래그는 D와 M으로 구성

  - D : Do Not Fragment (D값이 1이면 단편화를 하지 않고, 0이면 단편화를 한다.)

  - M : More Fragment (M값이 1이면 마지막 단편이 아니고, 0이면 마지막 단편이다.)

- **단편 옵셋(Fragment Offset)**

  - 단편화 되기 전 데이터 시작점으로 부터의 차이

  - 전체 데이터그램에서 단편에 포함된 데이터의 시작 위치

  - 8 바이트 단위로 표시 (전체길이 필드가 16비트라서, 단편옵셋도 16비트 여야 하지만 3비트를 플래그 표시에 사용함으로 남은 13비트로 16비트를 표한하기 위해)

  - 첫번째 단편의 옵셋 값은 0 (첫번째 단편이므로 시작 위치가 0 임)

  - 두번째 단편부터는 이전에 단편화된 길이를 8로 나누어 계산 (첫번째가 800 바이트 였다면, 두번째 단편화 옵셋 값은 100 )

  

Ref

---

https://toastfactory.tistory.com/280 

https://limjunho.github.io/2021/04/19/IP-Fragmentation.html



**각주**

---

[^MTU]: MTU(Maximum Transmission Unit) 데이터링크에서 하나의 프레임 또는 패킷에 담아 운반 가능한 최대 크기
