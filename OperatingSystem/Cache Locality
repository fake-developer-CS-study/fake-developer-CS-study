## 1)  캐시메모리

캐시 지역성을 알아보기전에 메모리 계층구조와 캐시 메모리에 대하여 간단히 알아봅시다.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/97cad085-8b1d-41f3-b753-ff895578e06d/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/97cad085-8b1d-41f3-b753-ff895578e06d/Untitled.png)

메모리 계층구조

많은 종류의 메모리가 존재하지만 대부분의 저장장치는 처리 속도가 빨라 질 수록 저장단위당 단가가 높아져 총 용량은 적어진다는 특징을 가지고 있다.

그 중 cache 메모리는 프로세서 내부에 위치하고있고 총 용량은 아주 작지만 매우 빠른 속도를 가지고있고 따라서 굉장히 비싸다.  (실제로 L1캐시 용량이 1MB 이상 급인 CPU들의 가격은 수백만원을 호가한다.)

위와 같은 이유로 (프로세서 관점에서 보았을 때) 왠만하면 캐시에서 데이터를 가져와서 사용하는 것이 가장 효율적이라고도 볼 수 있다.

프로세서가 매번 MM이나 Disk까지 접근하여  데이터를 불러온다면 시간이 너무 오래걸리기 때문이다. ( 메모리 접근으로 인한 딜레이 때문에 CPU의 성능을 온전히 이끌어내지 못함. = 메모리 병목 현상 )

캐시메모리의 내부를 물리적 형태로 구분하자면 L1 , L2 , L3 Cache가 존재하는데 내용이 다소 지엽적이라 판단되어 간단히 차이점만 짚어보고 넘어가도록 하겠습니다.

- L1 : 프로세서와 가장 가까운 캐시. I$와 D$로 나뉜다. 서버급을 제외하면 대부분 1MB 미만의 크기를 가지고있음.

    — Instruction Cache (I$): 메모리의 TEXT 영역 데이터를 다루는 캐시.

    — Data Cache (D$): TEXT 영역을 제외한 모든 데이터를 다루는 캐시.

- L2 : L1보다 큰 용량과 느린 속도를 가짐.
- L3 : 가장 낮은 수준의 캐시. 10MB에서 64MB까지 다양.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/46d2c16b-8e55-4409-b4be-74133b73b460/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/46d2c16b-8e55-4409-b4be-74133b73b460/Untitled.png)

프로세서 Layout

**캐시 성능지표**

캐시의 성능을 측정할 때는 히트 레이턴시(Hit latency)와 미스 레이턴시(Miss latency)를 중요한 요인으로 꼽을 수 있다.

CPU에서 요청한 데이터가 캐시에 존재하는 경우를 캐시 히트(Hit)라고 한다. **히트 레이턴시는 히트가 발생해 캐싱된 데이터를 가져올 때 소요되는 시간을 의미한다**. 반면 요청한 데이터가 캐시에 존재하지 않는 경우를 캐시 미스(Miss)라고 하며, 미스 레이턴시는 미스가 발생해 상위 캐시에서 데이터를 가져오거나(L1 캐시에 데이터가 없어서 L2 캐시에서 데이터를 찾는 경우) 최종적으로 메모리에서 데이터를 가져올 때 소요되는 시간을 말한다.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/20d61d46-aad8-4ce1-a171-158da0f2be3f/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/20d61d46-aad8-4ce1-a171-158da0f2be3f/Untitled.png)

Cache Hit

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/942a6acd-dc65-4f9c-a990-499ef4e3e41e/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/942a6acd-dc65-4f9c-a990-499ef4e3e41e/Untitled.png)

Cache Miss

평균 접근 시간(Average access time)은 다음과 같이 구한다.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/17c42695-ae50-4ac6-82de-df739b6a7ab0/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/17c42695-ae50-4ac6-82de-df739b6a7ab0/Untitled.png)

평균 접근 시간

캐시의 성능을 높이기 위해서는 캐시의 크기를 줄여 히트 레이턴시를 줄이거나, 캐시의 크기를 늘려 미스 비율을 줄이거나, 더 빠른 캐시를 이용해 레이턴시를 줄이는 방법이 있다.

**메모리 참조 과정 : 캐시→MM→Disk**

"캐시 미스 횟수 ≒ RAM 에서 데이터를 가져 오는 횟수" 이므로 캐시 적중률은 컴퓨터의 전체적인 성능과 매우 밀접하다. RAM의 경우 수백주기, HDD의 경우 수천만주기 인데 비해 (가장 높은 수준의) 캐시에서 데이터를 읽는 데 일반적으로 몇 번의 주기만 소요된다.

## **2) 캐시지역성 (공간 , 시간)**

빠른 처리를 위해 CPU가 어떤 데이터를 원할 것인가를 예측하는 과정에서 적용된 특징

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/55d00e32-7501-4c48-af1a-2ccccca59a31/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/55d00e32-7501-4c48-af1a-2ccccca59a31/Untitled.png)

x : 실행시간 / y : 메모리 주소 영역

1. **시간적 지역성(Temporal locality)** : 주어진 메모리 위치에 액세스 할 때 가까운 미래에 동일한 위치에 다시 액세스 할 가능성이 있습니다. 따라서 한번 사용된 메모리 위치는 바로 캐시메모리에 적재시키는(캐싱) 특성을 가지고 있습니다.
2. **공간적 지역성(Spatial Locality)** : 관련 데이터를 서로 가깝게 배치하는 것을 의미합니다. 캐싱은 CPU뿐만 아니라 여러 수준에서 발생한다. 예를 들어, RAM에서 읽을 때 일반적으로 프로그램이 곧 해당 데이터를 요구하기 때문에 특별히 요청한 것보다 더 큰 메모리 청크를 가져옵니다. 

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/16e18540-8e32-45a5-b07e-eda098c35f93/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/16e18540-8e32-45a5-b07e-eda098c35f93/Untitled.png)

    DB에서 많이 사용되는 B트리 자료구조

    ex) B트리는 디스크기반 스토리지에 적합하다. 한노드에 연속된 메모리(배열)를 할당해 많은 데이터를 저장한다. 이렇게 하면 한번의 디스크읽기를 통해 (단일 디스크 블록 읽는데 10ms 걸릴 수 있음) 최대로 주변노드들의 데이터를 함께 가져오게 된다. 이를 통해 데이터 검색시간을 줄일 수 있게 되고 디스크에 재차 접촉해야하는 횟수를 줄이게 되므로, 디스크 기반 스토리지에 적합한 데이터 구조이다.

    ### **캐싱 라인**

    캐시(cache)는 프로세서 가까이에 위치하면서 빈번하게 사용되는 데이터를 놔두는 장소이다. 하지만 캐시가 아무리 가까이 있더라도 찾고자 하는 데이터가 어느 곳에 저장되어 있는지 몰라 모든 데이터를 순회해야 한다면 시간이 오래 걸리게 된다. 즉, 캐시에 목적 데이터가 저장되어 있다면 바로 접근하여 출력할 수 있어야 캐시가 의미 있어진다는 것이다.

    **그렇기 때문에 캐시에 데이터를 저장할 때 특정 자료구조를 사용하여 묶음으로 저장하게 되는데 이를 캐싱 라인 이라고 한다.** 프로세스는 다양한 주소에 있는 데이터를 사용하므로 빈번하게 사용하는 데이터의 주소 또한 흩어져 있다. 따라서 캐시에 저장하는 데이터에는 데이터의 메모리 주소 등을 기록해 둔 태그를 달아놓을 필요가 있다. 이러한 태그들의 묶음을 캐싱 라인이라고 하고 메모리로부터 가져올 때도 캐싱 라인을 기준으로 가져온다. 종류로는 대표적으로 세 가지 방식이 존재한다.

    1. Direct Mapped Cache (직접 방식)

    - 메모리의 특정 블록 주소가 상위 계층의 단 한 곳에만 배치, 주 기억장치의 블록들이 지정된 한 개의 캐시 라인으로만 매핑한다.
    - 메모리 주소와 캐시의 순서를 일치시킨다.ex) 메모리가 1~100, 캐시가 1~10으로 가정하면 1~10까지의 메모리는 캐시의 1에 할당, 11~20까지의 메모리는 캐시의 2에 할당한다.
    - **구현 정말 간단하지만 캐싱 효율이 떨어져 잦은 교체 발생한다.**
    - **적중률이 낮고 성능이 낮은 단순한 방식이다.**

    2. Fully Associative Cache (완전 연관 방식)

    - 호출되는 블록이 캐시 내 어느곳에나 위치할 수 있는 방식이다. 즉, 블록을 찾기위해서는 모든 캐쉬내 블록을 찾아야한다.
    - 검색을 효율적으로 수행하기 위해서 캐시 엔트리와 연결된 비교기를 이용하여 병렬로 검색한다. 이러한 **비교기는 하드웨어 비용을 크게 증가시키므로 완전 연관 방식은 작은 수의 블록을 갖는 캐시에서만 유용하다.**
    - 메모리 주소와 캐시의 순서를 일치시키지 않는다.
    - **찾는 과정은 느리지만 필요한 캐시들 위주로 저장할 수 있기 때문에 적중률이 높다.**

    3. Set Associative Cache (집합 연관 방식)

    - 연관매핑에 직접매핑을 합쳐 놓은 방식으로 순서를 일치시키고 저장하되, 일정 그룹을 두어 그 그룹 내에서 저장토록 한다.ex) 메모리가 1~100, 캐시가 1~10으로 가정하면 캐시 1~5에는 1~50의 데이터 저장 가능하다.
    - **블록화가 되어 있으므로 검색 효율 증가, 적중률도 일정 수준 보장한다.**
    - index를 통해 한번에 접근할 수 있지만, N번의 비교가 필요하며 여전히 공간을 완전히 효율적으로 사용하진 못한다.

## 번외)  "캐시 친화적 코드" 란 ?

 
**메모리 이해 → 캐시 친화적인 코드**

지금까지의 내용을  종합해보면 "아무튼.. 왠만하면 캐시에서 데이터를 가져올 수 있으면, 가장 좋다."라는 결론에는 이견이 없을 것으로 생각된다. 

어떤 방식으로 캐싱이 되는지도 이해하셨을 것 같다. 그런데 코드레벨에서는 ? 어떠한 코드가 캐시를 효율적으로 사용할 수 있게 하는지 궁금해집니다. 

예로  JAVA Collection에서는 ArrayList는 공간지역성에 부합하나(데이터들이 일정 주소 범위내에 존재하기 때문에 메모리 청크를 가져오기 용이하다.) LinkedList의 경우 비어있는 위치를 찾아 저장하다보면 메모리 주소가 산발적인 경우가 많기 때문에 캐싱에 적합하지 않을 것이라고 유추 해볼 수 있다.

ref : 

[https://parksb.github.io/article/29.html](https://parksb.github.io/article/29.html)

[https://fogsbane.dev/ko/questions/16699247](https://fogsbane.dev/ko/questions/16699247)

[https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/OS#캐시의-지역성](https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/OS#%EC%BA%90%EC%8B%9C%EC%9D%98-%EC%A7%80%EC%97%AD%EC%84%B1)

[https://velog.io/@ha0kim/2021-02-22](https://velog.io/@ha0kim/2021-02-22)
