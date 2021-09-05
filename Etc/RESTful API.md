# RESTful API

### 우지현

REST란 REpresentational State Transfer의 약자로 월드 와이드 웹(www)과 같은 분산 하이퍼미디어 시스템을 위한 소프트웨어 개발 아키텍처의 한 형식이다. REST는 자원 기반의 구조(Resource Oriented Architecture)로 설계의 중심에 자원(Resource)이 있고 HTTP Method를 통해 Resource를 처리하도록 설계된 아키텍쳐이다.

RESTful API는 REST API 설계 가이드에 따라 API를 만드는 것으로 그러한 API를 제공하면 해당 웹 서비스는 RESTful하다고 한다. RESTful API는 그 자체만으로도 API의 목적이 무엇인지 쉽게 알 수 있기 때문에 (self-descriptiveness) RESTful함을 지향한다.

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

