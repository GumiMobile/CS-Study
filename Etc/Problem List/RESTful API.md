# RESTful API

### 우지현

REST란 REpresentational State Transfer의 약자로 월드 와이드 웹(www)과 같은 분산 하이퍼미디어 시스템을 위한 소프트웨어 개발 아키텍처의 한 형식이다. REST는 자원 기반의 구조(Resource Oriented Architecture)로 설계의 중심에 자원(Resource)이 있고 HTTP Method를 통해 Resource를 처리하도록 설계된 아키텍쳐이다.

RESTful API는 REST API 설계 가이드에 따라 만든 API로 그러한 API를 제공하면 해당 웹 서비스는 RESTful하다고 한다. RESTful API는 그 자체만으로도 API의 목적이 무엇인지 쉽게 알 수 있기 때문에 (self-descriptiveness) RESTful함을 지향한다.

REST의 구성 요소

- 자원(Resource) : URI
- 행위(Verb) : HTTP Method (GET, POST, PUT, DELETE)
- 표현(Representations) : 요청에 대한 적절한 응답. JSON 혹은 XML 등으로 나타냄

REST 6가지 원칙

- Uniform Interface : URI로 지정한 리소스에 대한 조작을 통일되고 한정적인 인터페이스로 수행하는 아키텍처 스타일
- Stateless : 작업을 위한 상태정보를 따로 저장하고 관리하지 않음
- Cacheable : HTTP라는 기존 웹표준을 그대로 사용하기 때문에 HTTP가 가진 캐싱 기능을 적용할 수 있음
- Client-Server : REST 서버는 API 제공, 클라이언트는 사용자 인증이나 context(세션, 로그인 정보) 등을 직접 관리하는 구조 
- Layered System :  REST 서버는 다중 계층으로 구성될 수 있으며 보안, 로드 밸런싱, 암호화 계층을 추가해 구조상의 유연성을 둘 수 있고 PROXY, 게이트웨이 같은 네트워크 기반의 중간매체를 사용할 수 있게 함
- Code on demand : 서버로부터 스크립트를 받아서 클라이언트에서 실행

### 김현수

RESTFul은 REST 아키텍처의 제약 조건을 준수하는 애플리케이션 프로그래밍 인터페이스를 뜻하며, 일관적인 컨벤션을 통한 API의 이해도 및 호환성을 높이는 것이 RESTFul의 주된 목적이다.

REST(REpresentational State Transfer)는 웹에 존재하는 모든 자원(이미지, 동영상, DB 자원)에 고유한 URI를 부여해 활용"하는 것으로, 자원을 정의하고 자원에 대한 주소를 지정하는 방법론을 의미한다.

API가 RESTful로 간주되려면 다음 기준을 따라야 한다.

1. Server-Client(서버-클라이언트 구조)
- REST Server: API를 제공하고 비즈니스 로직 처리 및 저장을 책임진다.
- Client: 사용자 인증이나 context(세션, 로그인 정보) 등을 직접 관리하고 책임진다.

2. Stateless(무상태)
- 요청 간에 클라이언트 정보가 저장되지 않는다.
- Server는 각 요청을 별개의 것으로 인식하고 처리한다.

3. Cacheable(캐시 처리 가능)
- 캐시 사용을 통해 응답시간이 빨라지고 REST Server 트랜잭션이 발생하지 않기 때문에 전체 응답시간, 성능, 서버의 자원 이용률을 향상시킬 수 있다.

4. Layered System(계층화)
- Client는 REST API Server만 호출한다.
- REST Server는 다중 계층으로 구성될 수 있다.
  - 보안, 로드밸런싱, 암호화, 사용자 인증 등을 추가하여 구조상의 유연성을 줄 수 있다.

5. Code-On-Demand(optional)
- Server로부터 스크립트를 받아서 Client에서 실행한다.
- 반드시 충족할 필요는 없다.

6. Uniform Interface(인터페이스 일관성)
- 아키텍처를 단순화시키고 작은 단위로 분리함으로써 클라이언트-서버의 각 파트가 독립적으로 개선될 수 있도록 해준다.

### 이수형

REST는 자원의 이름으로 구분하여 해당 자원의 상태를 주고받는 모든것을 의미
JSON혹은 XML을 통해 데이터를 주고받는것이 일반적임
RESTFul API는 CRUD(GET, POST, PUT, DELETE)의 HTTP 메소드로 표현함에 있어REST의 설계 가이드에 따라 만든 API이고 이해하기 쉽고 사용하기쉬운 REST API를 만드는것이 목적임
그 자체 만으로 API의 목적이 무엇인지 쉽게 알수있고 그것을 지향함

### 이유진
REST API == Representational State Transfer API

REST API는 URI만 보고 어떤 요청을 하는지 추측이 가능하도록 하는 형식이다. REST API의 규칙을 잘 지킨 요청을 RESTful 하다고 한다.
URI는 정보의 행위가 아닌 자원을 표현하는데 중점을 둔다. REST API에서 URI는 정보의 행위가 아닌 자원을 표현하는데 중점을 둬야 한다. 자원의 행위는 HTTP Method(GET, POST, PUT, DELETE emd)로 표현한다.
|메소드|역할 (CRUD)|
|--|--|
|POST|리소스 생성|
|GET|리소스 조회 (+ 도큐먼트에 대한 자세한 정보 얻기)|
|PUT|리소스 수정|
|DELETE|리소스 삭제|

### 윤상일

#### 1. REST

> Representational State Transfer : 추상적 표현 상태 전달...? 직역이 어렵다.

**대표적인 웹 기반 아키텍쳐**. 2000년 로이 필딩(Roy Fielding)의 논문에서 정의되었다.

1) **REST 아키텍쳐의 필요조건**에는 다음 다섯 가지가 있다.

- **클라이언트와 서버의 분리**

- **무상태(Stateless)**

- **캐시 처리가 가능해야 함**

- **시스템이 계층화(Layered) 되어있어야 함**

- **일관성 있는 인터페이스**

  - [x] **Stateless?** 서버처리방식의 한 분류. 애플리케이션의 상태에 대한 정보가 얼마나 오래 기록되는지, 어떤 식으로 저장되는지에 따라 Stateful / Stateless 를 분류한다. 이전 트랜잭션에 대한 정보가 저장되어 현재 트랜잭션에 영향을 주면 Stateful, 이전 트랜잭션에 대한 정보가 저장되지 않고 독립적인 기능을 수행하면 Stateless이다.
    더 깊은 이야기는 따로 다루어야 할 영역이니 중략.

    > 참조 : https://www.redhat.com/ko/topics/cloud-native-apps/stateful-vs-stateless 

    

  - [x] **계층화된 시스템이란?** : ~~물데네전세표응~~

    > 참조 : https://copycode.tistory.com/30

    

  - [x] **캐시란?**

|               | 쿠키                                             | 세션                                     | 캐시                                                         |
| ------------- | ------------------------------------------------ | ---------------------------------------- | ------------------------------------------------------------ |
| 저장위치      | 클라이언트                                       | 서버                                     | 클라이언트                                                   |
| 보안          | 변질 우려로 인해 보안 취약                       | 비교적 보안성이 좋음                     | 고려되지 않음 (이미지, 오디오, css 등 리소스의 재사용을 통해 빠른 페이지 로딩이 목적) |
| 라이프 사이클 | 만료 기간 지정, 브라우저 종료 시에도 유지        | 브라우저 종료 시 삭제 (기간 지정도 가능) | 별도 삭제 전까지 영구 저장                                   |
| 속도          | 빠름                                             | 느림 (서버에 정보가 있으므로)            | 빠름                                                         |
| 용례          | 자동 로그인, 위시 리스트 저장, 팝업 보지 않기 등 | 로그인 정보 유지 등                      | 홈페이지 재접속 시 css/js를 클라이언트 캐리메모리에서 로드. 서버를 거치지 않아도 된다. |

즉, 보안성이 필요한 데이터를 다루는 쿠키, 세션과는 사뭇 다른 영역의 데이터 처리 방식이다.

> 참조 : https://ryusae.tistory.com/7

2. **REST 아키텍쳐의 구성 요소**

- 자원
- 메서드 : HTTP 메서드를 사용

| CRUD            | Create | Read | Update | Delete |
| --------------- | ------ | ---- | ------ | ------ |
| **HTTP 메서드** | POST   | GET  | PUT    | DELETE |

- 메시지 : 대표적으로 404NOT FOUND

> 참조 : 나무위키 (https://namu.wiki/w/REST) (https://namu.wiki/w/HTTP/%EC%9D%91%EB%8B%B5%20%EC%BD%94%EB%93%9C)

#### 2. RESTful API

> Representational State Transfer-ful API

API는 기본적으로 프로그래밍 인터페이스로, 프로그램 내부에서 사용된다. 하지만 이식성을 증가시키기 위해 '웹'에 맞춰진 통신 인터페이스가 등장했는데, 그것이 바로 RESTful API이다.

소스코드를 통해 입출력이 이루어지는 일반 API와 달리 RESTful API는 웹을 이용하므로 거의 대부분 웹 프로토콜을 통해 주고 받는다. GET/POST 등의 형태로 인수를 전달받으면 결과값을 JSON이나 XML 형태로 전송해준다.

용례 : 롤 전적 검색 사이트 (OP.GG) - 게임사가 제공하는 외부 RESTful API를 기반으로 동작

