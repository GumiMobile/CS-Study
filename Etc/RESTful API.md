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
