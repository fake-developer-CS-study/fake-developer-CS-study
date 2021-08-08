## REST

---

### REST는 무엇인가 ?..

Representational State Transfer 의 약자를 Rest라고 하는데, 소프트웨어 아키텍처의 한 종류입니다.

클라이언트 사이드와 서버 사이드의 통신 방식중 하나입니다. 

WWW(World Wide Web) 에 존재하는 리소스에 URI를 매핑하여 해당 리소스를 활용하는 것으로, 자원을 정의하고 자원에 대한 주소를 지정하는 방법론을 REST라 칭합니다.

REST의 형식을 따른 시스템을 RESTful API라고 합니다.

`이름이 샘킴인 유저의 정보를 생성한다.`

라는 행위에 대해서 아래와 같이 처리됩니다.

```json
POST, /users/
{  
   "users":{  
      "name":"SAM KIM"
   }
}
```

HTTP URI : 자원을 명시

HTTP Method : 자원에 대한 CRUD 명령

ROA( Resource Oriented Architecture )로서 설계의 중심에 Resource가 있고, Method를 통해 아키텍처를 처리. 

장점

- 러닝 커브가 비교적 낮다.
- HTTP 프로토콜 표준을 지키며 범용성을 넓힌다.

단점

- PUT / DELETE를 지원하지 않는 케이스가 있다.

### REST의 특성

---

- Uniform Interface

REST를 느슨한 결합 구조를 가지고 있습니다. 이는 엄격하게 관리 구조가 아니기에, HTTP 표준을 따르며 사용가능한 플랫폼이라면 REST 아키텍처를 사용가능하게 되어있습니다.

ㄴ HTTP + JSON조합이 될 수도 있고, HTTP + XML 조합이 될 수도 있습니다.

- Stateless

REST의 가장 큰 특징중 하나는 상태를 유지하지 않는다는 것입니다. 사용자 혹은 Client Context를 서버 쪽에서 유지하지 않는다는 것입니다. HTTP Session과 같은 컨텍스트 저장소에 상태정보를 저장하지 않습니다. 

상태 정보를 저장하지 않으면 각 API 서버는 들어오는 요청만을 들어오는 메시지로만 처리하면 되며, 세션과 같은 컨텍스트 정보를 신경쓸 필요가 없기 때문에 구현이 단순해진다

- Cacheable

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/68f74436-12a0-41e2-a730-e8f27adbcfe2/스크린샷_2021-08-03_오전_8.17.32.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/68f74436-12a0-41e2-a730-e8f27adbcfe2/스크린샷_2021-08-03_오전_8.17.32.png)

HTTP 프로토콜 이라는 기존의 웹표준을 사용함에 따라, REST는 기존 웹의 인프라를 그대로 사용할 수 있습니다. 

그 중 하나의 강점으로는 HTTP 기반의 캐슁 기능을 사용할 수 있다는 것입니다. 웹 서비스에서 SELECT 쿼리가 서비스 쿼리의 다수를 차지하는 시점에 HTTP의 리소스들을 웹캐쉬 서버등에 캐슁하는 것은 용량이나 성능 면에서 많은 장점을 가져다줍니다. 

[+] Last-Modified, E-Tag를 이용하여 캐슁 구현 가능.

위 사진에서 Client가 HTTP GET을 “Last-Modified” 값과 함께 보냈을 때, 컨텐츠가 변화가 없으면 REST 컴포넌트는 “304 Not Modified”를 리턴하면 Client는 자체 캐쉬에 저장된 값을 사용하게 됩니다.

- Self-descriptive

리소스와 메서드를 이용해서 어떤 메서드에 무슨 행위를 하는지를 알 수 있으며, 또한 메시지 포맷 역시 JSON을 이용해서 직관적으로 이해가 가능한 구조입니다. 주석을 달 수는 없게한 이유가 여기에 있다고도 하는데요, URI / Method 만을 사용해서 메시지의 의도를 직관적으로 표현해야하는 것이 특징이기도 합니다. 

- Client-Server Architecture

REST 서버는 API를 제공하고, 제공된 API를 이용해서 비즈니스 로직 처리 및 저장을 책임집니다. 이는 Client와 Server 각각의 R&R을 확실하게 분리하여 개발 관점에서 클라이언트와 서버에서 개발해야 할 내용들이 명확하게 되고 서로의 개발에 있어서 의존성이 줄어들게 해줍니다.

### REST 안티 패턴

---

- GET/POST Tunneling

Method에 의해 명확한 행위를 구분짓지 않는 것은 REST의 안티 패턴에 해당합니다. 

- Not Using Self-descriptiveness

메시지를 이용해 직관적으로 이해할 수 있도록 설계하지 않은 경우를 뜻합니다.

- Not Using HTTP Response Code

에러 메시지에 대한 에러코드를 분기하지 않고 성공 Response Code와 함께 내려 준다거나, 명확한 에러코드 분기가 이루어지지 않은 경우를 뜻합니다.

## RESTful API

---

REST 하게 프론트-백단 통신이 이뤄지는 것을 RESTful하다고 하지않나.. 추측합니다.

HTTP Method에는 CRUD( Create, Read, Update, Delete ) 에 맞춰 아래와 같은 메서드들이 존재합니다.

- Create  ⇒ POST
- Read     ⇒ Get
- Update ⇒ PUT, PATCH
- Delete  ⇒ DELETE

---

### Rule

---

1. /를 마지막에 넣지 않는다. 

ex) {domain}/users/

1. _ 대신 -를 사용한다. ( But, Python처럼 - 사용이 불가한 경우 ? )
2. 소문자를 사용한다. ( CamelCase 사용 불가 )
3. method는 URI에 포함하지 않는다. 

ex) /users/1/put → put : /users/1

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/aeac2c4d-236b-401b-bbb4-12ab0e83ece3/스크린샷_2021-08-03_오후_3.28.46.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/aeac2c4d-236b-401b-bbb4-12ab0e83ece3/스크린샷_2021-08-03_오후_3.28.46.png)

### 

### Reference )

- [https://hckcksrl.medium.com/rest란-c602c3324196](https://hckcksrl.medium.com/rest%EB%9E%80-c602c3324196)