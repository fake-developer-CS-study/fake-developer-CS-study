CORS는 Cross-Origin Resource Sharing 줄인말입니다. 쉽게 생각하면 서로 다른 도메인에서 자원을 공유하는것을 이야기 합니다. 하나도 쉽지 않다구요?

간단한 예를 들어서 설명해보겠습니다. 

특정 서비스 웹사이트 (service.co.kr)에서 API(api.****.me)를 사용하여 동작합니다.  서비스 웹사이트와 API서버는 도메인이 서로 다르기에 이런 경우 Ajax통신을 시도시 cross origin 오류가 발생합니다.

![](https://user-images.githubusercontent.com/54073761/132977683-724b6ba2-9e45-4138-9d4b-ccd5dc5a1709.png)

이럴때 서버측에서 어떤 클라이언트를 허용할지에 대한 설정을 header를 넣어줘야합니다. header정보가 추가되면 통신이 허용되며 정상적으로 동작합니다. 이런 통신 방식을 CORS라고 합니다.

### CORS가 등장한 배경

초기장기 웹에서는 CORS개념이 없습니다. 보안에 대한 개념이 크게 대두되지 않았었습니다. 하지만 다른 사이트에서 API를 가져다 쓰는 경우 악의적인 코드를 응답값에 심어서 사이트를 해킹을 할 수 있었습니다. 그래서 다른 도메인에서 통신을 하는경우에 대한 보안 정책이 생겼습니다. 그정책이 CORS 입니다.

### 허용을 위한 작업

앞서 설명한것처럼 서버에서 작업을 해야하는것이 핵심입니다. API를 불특정 도메인에서 접근을 허용하는 경우는 아래와 같은 헤더가 필요합니다.

```
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: POST, GET, OPTIONS
```

`Access-Control-Allow-Origin` 은 허용을 할 도메인 정보 입니다. 

`*` 사용시 모든 도메인이 허용 됩니다. 만약 지정된 도메인만을 허용하려면 정확한 도메인을 지정해야합니다.

```
Access-Control-Allow-Origin: https://service.co.kr
Access-Control-Allow-Methods: POST, GET, OPTIONS
```

`Access-Control-Allow-Methods` 은 허용할 http 메소드입니다. 크로스 도메인을 허용한 메소드를 기술하면 됩니다.

### Options

options 메소드는 생소한 사람들도 많을것입니다. 직접적으로 사용하거나 구현한 적이 없기 때문이죠. 하지만 CORS를 구현할때 options 메소드를 필수로 구현해야합니다. 실제 데이터 통신하기전에 options 메소드를 이용해 실제 통신이 가능한지 확인하기 때문입니다. 브라우저가 원래 요청을 해야한 내용에서 메소드만 options로 변경된 형태로 요청을 합니다.

해더 설정이 잘되었는데도 cross origin 에러가 난다면 options를 먼저 확인하기를 권장 합니다.

![](https://user-images.githubusercontent.com/54073761/132977696-4199decf-c0b8-4320-b73b-f544605e0202.png)

### allow-headers

allow-headers도 챙겨야할 중요한 부분입니다. 해당 기능은 CORS가 허용할 해더를 정의하는것입니다. header 허용 역시 서버가 내려주는 설정 값입니다. 만약 `Authorization` 이라는 header정보가 인증을 위해서 필수 값이라면 `allow-headers` 에 꼭 추가해주어야 서버에 해당값이 전달 됩니다. 만약 여러개의 해더를 허용해야한다면 콤마로 이어 붙이면 됩니다.

```bash
access-control-allow-headers: Authorization, Content-Type, G-Auth-Key, Sid
```

### withCredentials, Access-Control-Allow-Credentials

credentials 보다 타이트한 보안 설정이라고 보시면 편할거 같습니다. 좀더 신뢰성있는 통신을 구축하는 방법이며 그렇기에 허용되는 범위도 최소한으로 해야하는 제약이 생깁니다.

[official site](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Access-Control-Allow-Credentials)

withCredentials설정이 없는경우 CORS요청은 쿠키값 전달을 하지 않습니다. 쿠키값을 전달하기위해선 withCredentials = true를 요청하는 쪽에서 옵션으로 사용해야합니다.

```jsx
const invocation = new XMLHttpRequest();
const url = 'http://bar.other/resources/credentialed-content/';
    
function callOtherDomain() {
  if (invocation) {
    invocation.open('GET', url, true);
    **invocation.withCredentials = true;**
    invocation.onreadystatechange = handler;
    invocation.send(); 
  }
}
```

withCredentials 값을 true로 변경시 몇가지 제약사항 추가됩니다.

- TLS 인증을 사용해야한다.
- 서버가 Access-Control-Allow-Credentials: true 해더를 반환해야한다.
- Access-Control-Allow-Origin에서 *를 사용 할 수 없다.

withCredentials= true 설정하고 서버 응답 해더에 Access-Control-Allow-Credentials=true가 없다면 아래와 같은 에러가 납니다.

![](https://user-images.githubusercontent.com/54073761/132977720-21809525-de88-4ffa-af42-54a849f45870.png)

서버에서 정상적으로 해더정보를 내려주면 아래와 같이 CORS가 성공하게 됩니다. Response Headers 부분을 유념해서 보시면 됩니다.

![](https://user-images.githubusercontent.com/54073761/132977714-19606a9f-c582-4b70-b2f4-c38fc17206a8.png)
