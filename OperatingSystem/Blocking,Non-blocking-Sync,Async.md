# Blocking vs Non-Blocking / Sync vs Async

`동기(Synchronous), 비동기(Asynchronous)` 그리고 `블로킹(Blocking), 논블로킹(Non-Blocking)`은 운영체제를 배울 때 많이 나오기도 하고 프로그램을 개발할 때 중요한 개념 중 하나입니다. 기초 프로그래밍을 배우고 응용 파트인 병렬 프로그래밍을 익힐 때 나오는 개념이고 익히기 쉽지 않은 개념이기도 합니다.



Synchronous와 Asynchronous 개념과 Blocking과 Non-Blocking은 혼용하여 사용하는 경우가 있지만 엄연히 서로 다른 개념입니다. 흔히 `Synchronous == Blocking`, `Asynchronous == Non-Blocking`으로 헷갈리기도 합니다. 하지만 실제로는 두 개념은 서로 크게 연관관계가 없는 별개의 개념입니다.

Blocking/Non-Blocking은 **작업의 대상**이 2개 이상일 때, 하나의 작업이 끝날 때까지 기다렸다가 다음 작업을 진행하면 `Blocking`이라고 부르고, 다른 작업의 진행여부와 관계없이 자신의 작업을 계속할 수 있다면 `Non-Blocking`이라고 부릅니다.

반면 Synchronous/Asynchronous는 **작업을 수행하는 주체**가 2개 이상일 때, 작업의 시간(시작, 종료 등)을 서로 맞춘다면 `Synchronous`라고 부르고, 서로 작업의 시간이 관계없다면 `Asynchronous`라고 부릅니다.

두 개념이 서로 바라보는 관점이 다르기 때문에 `Synchronous/Blocking, Synchronous/Non-Blocking, Asynchronous/Blocking, Asynchronous/Non-Blocking`의 다양한 조합이 가능합니다.

아직은 이해가 안갈 수도 있지만 아래의 자세한 설명을 계속 보게되면 이해할 수 있을 것이라고 생각합니다.



## Blocking vs Non-Blocking

Blocking/Non-Blocking은 **직접 제어할 수 없는 대상을 처리하는 방법**에 따라 나눕니다. 즉, `작업의 제어권`에 관한 개념인 것입니다. 

직접 제어할 수 없는 대상은 대표적으로 **I/O, 멀티쓰레드 동기화**가 있습니다.



### Blocking

Blocking은 직접 제어할 수 없는 **대상의 작업이 끝날 때까지 제어권을 넘겨주지 않는 것**입니다. 예를 들면, 호출하는 함수가 I/O를 요청했을 때 I/O처리가 완료될 때까지 아무 일도 하지 못한 채 기다리는 것을 말합니다.

Thread의 관점으로 본다면, 요청한 작업이 끝날 때까지(return 값을 받을 때까지) 계속 대기하며 한 Thread를 계속 사용/대기 합니다.

<img src="https://user-images.githubusercontent.com/79291114/122675246-8acf8700-d213-11eb-9546-c075d8ee38ba.png" alt="blocking" style="zoom:80%;" />



### Non-Blocking

Non-Blocking은 Blocking과 반대되는 개념입니다. 직접 제어할 수 없는 **대상에게 제어권을 넘겨주었다가 대상 작업을 실행시킨 후 바로 제어권을 다시 받아 자신의 작업을 계속 진행하는 것**입니다.  예를 들면, 호출하는 함수가 I/O를 요청한 후 I/O처리 완료 여부와 상관없이 바로 자신의 작업을 할 수 있는 것을 말합니다.

Thread의 관점으로 본다면, 하나의 Thread가 여러 개의 I/O 처리를 가능하게 합니다.



**어떻게 하나의 Thread가 여러 개의 I/O를 처리할 수 있을까요??**

그 이유는 입출력 장치들의 I/O연산은 I/O 컨트롤러가 담당하고, 컴퓨터 내에서 수행되는 연산은 메인 CPU가 담당하여 **입출력 장치와 메인 CPU는 동시 수행이 가능**하기 때문입니다. 

**참고 링크**

[CPU와 I/O 연산](https://chogyujin.github.io/2019/03/19/2.2-CPU%EC%99%80-IO-%EC%97%B0%EC%82%B0/)

<img src="https://user-images.githubusercontent.com/79291114/122675248-8b681d80-d213-11eb-9dbe-25c3411c9c65.png" alt="non-blocking" style="zoom:80%;" />





## Synchronous vs Asynchronous

Synchronous/Asynchronous은 **여러 작업들의 시간을 맞추는 방법**에 따라 나눕니다. 즉, `작업의 흐름`에 관한 개념인 것입니다. 



### Synchronous

Synchronous는 작업을 수행하는 두 개 이상의 주체(함수, 어플리케이션 등)가 **서로 동시에 수행하거나, 동시에 끝나거나, 끝나는 동시에 시작할 때를 의미**합니다. 시작과 종료를 동시에 하거나, 하나의 작업이 끝나는 동시에 다른 주체가 작업을 시작하면 이는 Synchronous 작업이라고 볼 수 있습니다.

> 이 때, 시간을 맞추기 위하여 **작업이 진행 중인 주체를 주기적으로 검사하는 작업을 polling**이라고 합니다.

<img src="https://user-images.githubusercontent.com/79291114/122675254-8c00b400-d213-11eb-9405-d495649b3f38.png" alt="sync" style="zoom:80%;" />



### Asynchronous

Asynchronous는 작업을 수행하는 두 개 이상의 주체가 **서로의 시작, 종료시간과는 관계 없이 별도의 수행 시작/종료시간을 가지고 있을 때를 의미**합니다. 다른 주체가 하는 작업이 자신의 작업 시작, 종료시간과는 관계가 없을 때 Asynchronous라고 부를 수 있습니다.



**Synchronous에서는 polling을 통해 종료시간을 검사하고 결과를 반환받지만, Asynchronous에서는 어떻게 종료시간을 알고 결과를 반환받을 수 있을까요??**

Asynchronous에서 작업이 완료된 주체는 **callback 함수를 통하여 작업이 완료되었다고 알려줌으로써 결과를 반환**받을 수 있습니다.

>  **callback 함수는 다른 함수의 매개변수로 함수를 전달하고, 어떠한 이벤트가 발생한 후 매개변수로 전달한 함수가 다시 호출되는 것을 의미**합니다.

<img src="https://user-images.githubusercontent.com/79291114/122675245-8acf8700-d213-11eb-84ea-12f5db0811f3.png" alt="async" style="zoom:80%;" />





## Blocking / Non-Blocking 과 Synchronous / Asynchronous의 조합

Blocking / Non-Blocking은 `작업의 제어권`에 관한 개념이고, Sync / Async는 `작업의 흐름`에 관한 개념이기 때문에 다양하게 조합할 수 있습니다.

<img src="https://user-images.githubusercontent.com/79291114/122675247-8acf8700-d213-11eb-8294-a55472d01f5a.png" alt="combi" style="zoom: 67%;" />



### Synchronous+ Blocking

Synchronous+ Blocking은 Synchronous의 조건인 `두 개 이상 작업의 시작시간, 종료시간이 같거나 시작과 동시에 종료할 것`, 그리고 Blocking의 조건인 `다른 작업이 작업을 시작하면 제어권을 넘겨주어 끝날 때까지 기다리는 것` 을 만족해야 합니다.

<img src="https://user-images.githubusercontent.com/79291114/122675249-8b681d80-d213-11eb-81b4-381f6b6b50c6.png" alt="sync,blocking" style="zoom: 80%;" />

#### 예시

1. 현재 주체가 작업을 진행하던 중, `Blocking`되어 다른 주체의 작업을 실행하게 되면 현재 주체는 아무 것도 하지 않고 기다립니다.
2. 다른 주체가 작업이 끝나면 현재 주체가 계속 작업을 이어나갑니다. (`Synchronous`)

현재 주체는 다른 주체가 작업하는 동안 아무것도 하지 않기 때문에 비효율적입니다.

Synchronous이며 Blocking인 작업의 예는 아래와 같습니다.

- JDBC를 이용해 DB에 쿼리 질의를 날릴 때
- 현재 메서드에서 다른 메서드를 호출하여 결과값을 받아온 후 현재 메서드를 계속 실행할 때



### Synchronous+ Non-Blocking

Synchronous+ Non-Blocking은 Synchronous의 조건인 `두 개 이상의 작업의 시작시간, 종료시간이 같거나 시작과 동시에 종료할 것`, 그리고 Non-Blocking의 조건인 `다른 작업이 작업을 시작해도 제어권을 넘겨주지 않고 기다리지 않는 것` 을 만족해야 합니다.

<img src="https://user-images.githubusercontent.com/79291114/122675252-8c00b400-d213-11eb-9118-d0a404ba478e.png" alt="sync,non-blocking" style="zoom:80%;" />

#### 예시

1. 현재 주체가 작업을 진행하던 중, 다른 주체가 작업을 실행하게 되면 `Non-Blocking`이여서 현재 주체는 다른 주체와 상관없이 계속 작업을 이어나가게 됩니다.
2. 하지만 `Synchronous`이기 때문에 `polling`을 통해 다른 주체가 작업이 끝났는지 계속해서 검사합니다.

현재 주체가 다른 주체를 계속해서 검사해, 작업 효율이 좋지 않은 편입니다.



### Asynchronous+ Non-Blocking

Asynchronous+ Non-Blocking은 Asynchronous의 조건인 `두 개 이상의 작업의 시작시간, 종료시간을 맞추지 않는 것`, 그리고 Non-Blocking의 조건인 `다른 작업이 작업을 시작해도 제어권을 넘겨주지 않고 기다리지 않는 것` 을 만족해야 합니다.

<img src="https://user-images.githubusercontent.com/79291114/122675244-8a36f080-d213-11eb-9905-20f3a94c4dcb.png" alt="async,non-blocking" style="zoom:80%;" />

#### 예시

1. 현재 주체가 작업을 진행하던 중, 다른 주체가 작업을 실행하게 되면 `Non-Blocking`이여서 현재 주체는 다른 주체와 상관없이 계속 작업을 이어나가게 됩니다.
2. 다른 주체의 작업이 완료되면 `Asynchronous`이기 때문에 다른 주체가 `callback` 함수를 통해 작업이 끝났다고 알려줍니다.
3. `callback`함수가 호출되면, 다른 주체의 결과를 반환받고 현재 주체는 계속해서 작업을 진행할 수 있습니다. 

현재 작업이 방해받지 않고, 다른 주체에게 작업을 맡겨놓고 계속해서 작업을 이어나갈 수 있기 때문에 해야 할 작업이 대규모이고, Synchronous가 필요하지 않을 때 효과적입니다.

Asynchronous이며 Non-Blocking인 작업의 예는 아래와 같습니다.

- 대규모 사용자에게 푸시메세지 전송
- 다양한 외부 API를 한번에 호출할 때



### Asynchronous+ Blocking

Asynchronous+ Blocking은 Asynchronous의 조건인 `두 개 이상의 작업의 시작시간, 종료시간을 맞추지 않는 것`, 그리고 Blocking의 조건인 `다른 작업이 작업을 시작하면 제어권을 넘겨주어 끝날 때까지 기다리는 것`을 만족해야 합니다.

Asynchronous+ Blocking은 개발자가 유도해서 이 상황을 만드는 것보다 **Asynchronous+ Non-Blocking 작업을 실행하였지만 자기도 모르게 Blocking 작업을 실행했을 때 이러한 결과**가 나옵니다. (대표적으로 Node.js + MySQL 조합이 있다고 합니다.)

<img src="https://user-images.githubusercontent.com/79291114/122675242-899e5a00-d213-11eb-9abf-13b79024745e.png" alt="async,blocking" style="zoom:80%;" />

#### 예시

1. 현재 주체가 작업을 진행하던 중, 다른 주체가 작업을 실행하게 되면 `Non-Blocking`이여서 현재 주체는 다른 주체와 상관없이 계속 작업을 이어나가게 됩니다. (`Asynchronous+ Non-Blocking` 작업)
2. 현재 주체가 작업을 진행하다가 다른 주체의 `Blocking 메서드`를 실행해, 다른 주체의 `Blocking 메서드` 작업이 끝날 때까지 현재 주체는 아무 것도 하지 않고 기다립니다.
3. `Blocking 메서드` 작업이 끝나면 현재 주체가 계속 작업을 이어나갑니다.

Asynchronous+ Blocking 조합은 결국 다른 작업이 끝날 때를 기다려야 하기 때문에 Synchronous+ Non-Blocking과 비슷한 작업 효율이 나옵니다. 즉 그리 좋은 효율이 나오지 않습니다.





## CPU의 관점에서의 Synchronous / Asynchronous

데이터를 송,수신해야 하는 프로그램을 개발하다보면 Asynchronous I/O를 이용해야 하는 경우가 있다고 합니다. 어떤 경우에 Asynchronous를 사용해야 할지 왜 Synchronous를 사용하지 않는지 CPU의 관점에서 알아보겠습니다.

>  유튜브나 넷플릭스, 스트리밍 앱 처럼 평소에도 Asynchronous I/O를 이용하는 프로그램을 사용합니다.



### 동영상 스트리밍 서비스

동영상 스트리밍 서비스 프로그램을 개발한다고 가정해보겠습니다. 대부분의 스트리밍 서비스는 재생할 동영상이 있는 서버로부터 데이터를 수신 받아 영상을 플레이하는 방식입니다. 이런 서비스를 Synchronous 방식으로 구현하게 되면 그 과정은 아래와 같습니다.

<img src="https://user-images.githubusercontent.com/79291114/122693924-bafc4180-d276-11eb-9d3e-cd68daabd4e7.jpg" alt="sync-video"  />



데이터 수신과 플레이라는 동작을 CPU관점에서 보게 되면, **실질적으로 CPU를 사용하고 있는 부분은 동영상 플레이** 동작입니다.

반대로 **데이터를 수신할 때는 I/O 연산이 대부분을 차지**하고 CPU의 관여가 거의 없기 때문에 Synchronous 방식은 데이터 수신이 발생하는 시간 동안 CPU가 일을 하지 않는 시간이 늘어나게 되고, 인터넷 속도가 느리기라도 한다면 계속해서 버퍼링 현상이 발생합니다. 이는 아래와 같이 CPU 사용률의 활용 관점에서 굉장히 좋지 않을 뿐만 아니라 시간 지연 및 성능 저하로까지 이어지게 될 수 있습니다.

<img src="https://user-images.githubusercontent.com/79291114/122693922-ba63ab00-d276-11eb-8790-79ce1271d0d2.png" alt="cpu-utilization"  />



이러한 문제점은 아래와 같은 Asynchronous I/O 방식으로 구현하여 해결할 수 있으며, 이 때 얻게 되는 성능 향상은 하드웨어나 시스템 구조를 더 좋게 변경한다고 해도 비교할 수 없을 정도입니다.

![Async-video](https://user-images.githubusercontent.com/79291114/122693921-b9327e00-d276-11eb-9449-448dab54c434.jpg)



데이터 수신은 CPU 사용을 크게 요구하지 않는 작업이기 때문에 영상이 재생되는 동안 I/O 작업이 병행되어도 문제가 되지 않습니다. **서버로부터 지속적인 데이터 수신을 하면서 버퍼링된 데이터를 기반으로 영상을 재생**하기 때문에, 시간 지연 문제는 줄어들게 되고 아래와 같이 CPU 사용률도 개선됩니다.

![cpu-utilization2](https://user-images.githubusercontent.com/79291114/122693923-ba63ab00-d276-11eb-8a69-ee313e6e59ca.jpg)

### 질문 답변

Q1. Blocking / Non-Blocking 과 Synchronous / Asynchronous의 조합에서 polling과 callback에 대한 비교.

→ (JAVA를 기준으로 한 설명)
<img src="https://t1.daumcdn.net/cfile/tistory/99AD623D5CD18E521D">



Q2. 동기-논 블로킹 조합에 대하여

동기는 제어권을 반환하는 시간과 결과값을 전달하는 시간이 같아야 한다. 

그런데, 다음 사진의 동기 - 논블로킹 형태를 살펴보자.

![image](https://user-images.githubusercontent.com/40491724/123636641-11114c00-d858-11eb-9158-b440a245feab.png)

Application이 커널로 컨텍스트 스위칭되고, 작업이 수행된다. 하지만 논 블로킹이라 제어권을 반환해주게 되는데 동기이기에 결과값도 같이 전달해야한다. 그런데, 아직 작업이 끝나지 않은 시점인데 결과를 반환하게 되면 어떻게 되는지가 고민대상이었다. 이에 대한 해답은 결과값에는 함수의 완료된 상태 뿐만 아니라, 완료되지 않음이라는 상태도 결과값으로 보기에 제어권을 반환하면서 "완료되지 않음이라는 상태"라는 결과값을 같이 보내게 된다. (EAGAIN/EWOULDBLOCK : 에러가 아니라 들어온게 없음을 의미함.) 그래서, 이것을 동기-논블로킹 조합으로 볼 수 있다.

그리고 지속적으로 컨텍스트 스위칭이 발생하긴 하지만, 제어권이 돌아오는 시점에서 잠깐 제 할일을  한다.

**정리**

- **동기 - 논 블로킹**은 지속적으로 컨텍스트 스위칭이 발생하긴 하지만, 잠시 제어권이 돌아오는 시점에서 제 할일을 하다가 다시 제어권을 넘겨준다.
- 동기는 제어권을 반환하는 시간과 결과값을 전달하는 시간이 같은데, 작업이 끝나지 않은 것의 결과는 "완료되지 않음"이라는 결과를 보내게 된다.



---



참고 : [https://deveric.tistory.com/99](https://deveric.tistory.com/99)

[https://velog.io/@wonhee010/%EB%8F%99%EA%B8%B0vs%EB%B9%84%EB%8F%99%EA%B8%B0-feat.-blocking-vs-non-blocking](https://velog.io/@wonhee010/%EB%8F%99%EA%B8%B0vs%EB%B9%84%EB%8F%99%EA%B8%B0-feat.-blocking-vs-non-blocking)

[https://m.blog.naver.com/sky00141/221926559559](https://m.blog.naver.com/sky00141/221926559559)

[https://chogyujin.github.io/2019/03/19/2.2-CPU%EC%99%80-IO-%EC%97%B0%EC%82%B0/](https://chogyujin.github.io/2019/03/19/2.2-CPU%EC%99%80-IO-%EC%97%B0%EC%82%B0/)

[https://asfirstalways.tistory.com/348](https://asfirstalways.tistory.com/348)

[https://velog.io/@codemcd/Sync-VS-Async-Blocking-VS-Non-Blocking-sak6d01fhx](https://velog.io/@codemcd/Sync-VS-Async-Blocking-VS-Non-Blocking-sak6d01fhx)

