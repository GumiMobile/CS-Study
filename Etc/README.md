# 기타

- [RESTful API](#RESTful-API)
    - [REST](#1-REST)
    - [RESTful API](#2-RESTful-API)
    - [REST 6가지 원칙 (또는 특징)](#3-REST-6가지-원칙-또는-특징)
- [함수형 프로그래밍](#함수형-프로그래밍)
    - [1급 객체](#1급-객체)
    - [순수 함수](#순수-함수pure-function)
    - [고차 함수](#고차-함수High-Order-Function)
    - [익명 함수](#익명-함수Anonymous-function)
    - [합성 함수](#합성-함수Function-composition)
- [Git과 GitHub](#Git과-GitHub)
    - [버전관리란?](#버전관리란-Version-Control)
    - [Git](#Git)
    - [GitHub](#GitHub)
    - [Git의 장점](#Git의-장점)
    - [Git(GitHub)의 주요 개념들](#GitGitHub의-주요-개념들)
<br>

[뒤로](https://github.com/GumiMobile/CS-Study)

<br>

## RESTful API

### 1. REST

> **RE**presentational **S**tate **T**ransfer  
> 자원(Resource)의 표현(Representation) `=> 자원에 이름(ID) 부여` 에 의한 상태(State) 전달(Transfer)

**대표적인 웹 기반 아키텍쳐**  
월드 와이드 웹(www)과 같은 분산 하이퍼미디어 시스템을 위한 소프트웨어 개발 아키텍처(설계 지침, 원리 등)의 한 형식이다.  
2000년 로이 필딩(Roy Fielding)의 논문에서 정의되었다.

#### 자원 기반의 구조(Resource Oriented Architecture)
- 설계의 중심 : 자원(Resource)
- 웹에 존재하는 모든 자원 (이미지, 동영상, DB 자원)에 고유한 ID를 부여 (ID는 URI PATH를 사용)
- 자원을 정의하고 자원에 대한 주소를 지정하는 방법론을 의미
- HTTP Method를 통해 Resource를 처리하도록 설계된 아키텍쳐

#### REST의 구성 요소

- 자원(Resource) : URI  
    - URI는 정보의 행위가 아닌 자원을 표현하는데 중점을 둔다. 

- 행위(Verb) : HTTP Method (GET, POST, PUT, DELETE)  
    | CRUD            | Create | Read | Update | Delete |
    | --------------- | ------ | ---- | ------ | ------ |
    | **HTTP 메서드** | POST   | GET  | PUT    | DELETE |

- 표현(Representations) : HTTP Message Pay Load  
    - [상태코드](https://developer.mozilla.org/ko/docs/Web/HTTP/Status)(예. `200 OK`, `404 NOT FOUND`) : HTTP 요청의 성공여부를 서버에서 알려주는 코드
    - 요청에 대한 적절한 응답. JSON 혹은 XML 등으로 나타냄

<br>

### 2. RESTful API
> **RE**presentational **S**tate **T**ransfer-**ful API**

**RESTFul**은 **REST 아키텍처의 설계 가이드를 따라 만든 API**를 뜻한다.  
REST API 설계 가이드를 잘 지킨 API를 제공하면 해당 웹 서비스는 RESTful하다고 한다.  
REST 원리를 따르는 시스템은 RESTful이라는 용어로 지칭된다.

#### RESTful API의 목적
- 일관적인 컨벤션을 통한 API의 이해도 및 호환성을 높이는 것
- 이해하기 쉽고 사용하기 쉬운 REST API를 만드는것

#### 장점
- RESTful API는 그 자체만으로도 API의 목적이 무엇인지 쉽게 알 수 있다. (self-descriptiveness)
- HTTP  프로토콜의 인프라를 그대로 사용하므로, REST API사용을 위한 별도의 인프라를 구축할 필요가 없다.
- HTTP 표준 프로토콜을 따르는 모든 플랫폼에서 사용 가능하다.
- 여러가지 서비스 디자인에서 생길 수 있는 문제를 최소화한다.
- 서버와 클라이언트의 역할을 명확하게 분리한다.

#### 단점
- 명확한 표준이 없다.
- 사용할 수 있는 메소드가 4자지이다. (HTTP Method 형태가 제한적)
- 브라우저를 통해 테스트하는 서비스라면, 고칠 수 있는 URL보다 비교적 Header값이 어렵다.
- 구형 브라우저가 제대로 지원해주지 못하는 부분이 존재한다.

#### RESTful 하지 못한 경우 (예시)
- CRUD기능을 모두 POST로만 처리하는 API
- route에 resource,id외의 정보가 들어가는 경우(/students/updateName)

<br>

- [x]  API와 RESTful API  
    API는 기본적으로 프로그래밍 인터페이스로, 프로그램 내부에서 사용된다. 하지만 이식성을 증가시키기 위해 '웹'에 맞춰진 통신 인터페이스가 등장했는데, 그것이 바로 RESTful API이다. 소스코드를 통해 입출력이 이루어지는 일반 API와 달리 RESTful API는 웹을 이용하므로 거의 대부분 웹 프로토콜을 통해 주고 받는다. GET/POST 등의 형태로 인수를 전달받으면 결과값을 JSON이나 XML 형태로 전송해준다.
    > 용례 : 롤 전적 검색 사이트 (OP.GG) - 게임사가 제공하는 외부 RESTful API를 기반으로 동작

<br>

### 3. REST 6가지 원칙 (또는 특징)
> API가 RESTful하다고 간주되기 위한 필요조건

1. **Uniform Interface** (일관성있는 인터페이스)
    - URI로 지정한 리소스에 대한 조작을 통일되고 한정적인 인터페이스로 수행하는 아키텍처 스타일
    - 아키텍처를 단순화시키고 작은 단위로 분리함으로써 클라이언트-서버의 각 파트가 독립적으로 개선될 수 있도록 해준다.
2. **Stateless** (무상태)
    - 작업을 위한 상태정보를 따로 저장하고 관리하지 않음
    - HTTP프로토콜이 Staleless Protocol이므로 REST 역시 무상태성을 갖는다.
    - 요청 간에 클라이언트 정보(세션, 쿠기와 같은 context정보)가 저장되지 않는다.
    ⇒ 구현이 단순해짐
    - Server는 각 요청을 별개의 것으로 인식하고 처리한다.
    ⇒ Server의 처리 방식에 일관성을 부여하고 부담이 줄어들어, 서비스의 자유도가 높아짐
3. **Cacheable** (캐시 처리 가능)
    - HTTP라는 기존 웹표준을 그대로 사용하기 때문에 HTTP가 가진 캐싱 기능을 적용할 수 있다.
    - 캐시 사용을 통해 응답시간이 빨라지고 REST Server 트랜잭션이 발생하지 않기 때문에 전체 응답시간, 성능, 서버의 자원 이용률을 향상시킬 수 있다.
4. **Client-Server** (클라이언트 - 서버 분리)
    - REST Server: API를 제공하고 비즈니스 로직 처리 및 저장을 책임진다.
    - Client: 사용자 인증이나 context(세션, 로그인 정보) 등을 직접 관리하고 책임진다.
5. **Layered System** (시스템이 계층화 되어있어야 함)
    - Client는 REST API Server만 호출한다.
    - REST Server는 다중 계층으로 구성될 수 있다.
        - 보안, 로드밸런싱, 암호화, 사용자 인증 등을 추가하여 구조상의 유연성을 줄 수 있다.
    - PROXY, 게이트웨이 같은 네트워크 기반의 중간매체를 사용할 수 있게 한다.
6.  **Code on demand** (optional)
    - Server로부터 스크립트를 받아서 Client에서 실행한다.
    - 반드시 충족할 필요는 없다.

<br>

- [x]  **Stateless?**  
    서버처리방식의 한 분류. 애플리케이션의 상태에 대한 정보가 얼마나 오래 기록되는지, 어떤 식으로 저장되는지에 따라 Stateful / Stateless 를 분류한다. 이전 트랜잭션에 대한 정보가 저장되어 현재 트랜잭션에 영향을 주면 Stateful, 이전 트랜잭션에 대한 정보가 저장되지 않고 독립적인 기능을 수행하면 Stateless이다. 더 깊은 이야기는 따로 다루어야 할 영역이니 중략.
    > 참조 : https://www.redhat.com/ko/topics/cloud-native-apps/stateful-vs-stateless

<br>

- [x]  **캐시란?**  

    |               | 쿠키                                             | 세션                                     | 캐시                                                         |
    |-------------|-----------------------------------------------|----------------------------------------|------------------------------------------------------------|
    | 저장위치      | 클라이언트                                       | 서버                                     | 클라이언트                                                   |
    | 보안          | 변질 우려로 인해 보안 취약                       | 비교적 보안성이 좋음                     | 고려되지 않음 (이미지, 오디오, css 등 리소스의 재사용을 통해 빠른 페이지 로딩이 목적) |
    | 라이프 사이클 | 만료 기간 지정, 브라우저 종료 시에도 유지        | 브라우저 종료 시 삭제 (기간 지정도 가능) | 별도 삭제 전까지 영구 저장                                   |
    | 속도          | 빠름                                             | 느림 (서버에 정보가 있으므로)            | 빠름                                                         |
    | 용례          | 자동 로그인, 위시 리스트 저장, 팝업 보지 않기 등 | 로그인 정보 유지 등                      | 홈페이지 재접속 시 css/js를 클라이언트 캐리메모리에서 로드. 서버를 거치지 않아도 된다. |
    즉, 보안성이 필요한 데이터를 다루는 쿠키, 세션과는 사뭇 다른 영역의 데이터 처리 방식이다.
    > 참조 : https://ryusae.tistory.com/7

<br>

- [x]  **계층화된 시스템이란?** : ~~물데네전세표응~~  
    > 참조 : https://copycode.tistory.com/30


<br>

[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#기타)

<br>

## 함수형 프로그래밍

함수형 프로그래밍(FP)은 순수 함수(pure function)를 조합하고 공유 상태(shared state), 변경 가능한 데이터(mutable data) 및 부작용(side-effects)을 피하는 기본 원칙에 따라 소프트웨어를 구성하는 프로그래밍 패러다임이다.<br/>
함수 : f(x) = y 즉, x를 입력하면 항상 y가 나와야 함



### 1급 객체

- 변수나 데이터구조안에 담을수 있는 객체
- 파라미터로 전달가능
- 리턴값으로 사용할 수 있음
- 할당시 사용하는 이름과 무관하게 고유한 구별이 가능

### 순수 함수(pure function)

순수 함수란 같은 입력에 대해 항상 같은 출력을 반환하는 함수로 다음과 같은 조건을 만족한다. 멀티쓰레드에서도 안전하고 병렬처리 및 계산도 가능하다.

- 동일한 입력에 대해 항상 같은 값을 반환.
- 부작용(다른 요인에 따른 결과변경)이 없는 결과를 생성.
- 함수에서 인자를 변경하거나 프로그램의 상태를 변경하지 않음.
```JavaScript
//언제 어디서든 항상 같은 값을 리턴함(순수함수)
function add(a,b){
    return a + b;
}

//만약 외부의 c가 변경되면 리턴값이 달라짐(순수하지않음)
var c = 10;
function add2(a,b){
    return a + b + c;
}
```
### 고차 함수(High Order Function)

다음 조건을 만족하는 함수를 말한다.

- 함수의 인자로 함수를 전달할 수 있다.
- 함수의 리턴값으로 함수를 사용할 수 있다.

```JavaScript
const arr1 = [1, 2, 3];
const arr2 = arr1.map(item => item * 2);
console.log(arr2);
```
### 익명 함수(Anonymous function)

이름이 없는 함수, 즉 람다식으로 표현되는 함수 구현을 말한다.
```Kotlin
//안드로이드에 흔히 보이는 
button.setOnClickListener(object : OnClickListener{
  override fun onClick(view: View){
    doSomething()
  }
})
```
### 합성 함수(Function composition)

새로운 함수를 만들거나 계산하기 위해 둘 이상의 함수를 조합하는 과정.

함수형 프로그램은 여러 작은 순수 함수들로 이루어져 있기 때문에 이 함수들을 연쇄적으로 또는 병렬적으로 호출해서 더 큰 함수를 만드는 과정으로 전체 프로그램을 구축해야 한다.

<br><br>

[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#기타)

<br>

# Git과 GitHub
> `Git`은 로컬에서 VCS을 운영하는 방식이고 `Github`는 Github에서 제공해주는 클라우드 서버를 이용한다.

<br>

### 버전관리란? (Version Control)
소스 하나 또는 묶음을 하나의 버전으로 간주한다. 파일이나 폴더를 추가, 수정, 삭제하며 사람이 직접 관리한다. 원할 때 예전 버전 내용 전체를 되돌려 볼 수 있으며 복잡한 코드를 개발할 때 이전 버전과 비교하기 수월하다는 이점이 있다.
- 대표적인 VCS(Version Control System) : Git

### 버전관리가 필요한 이유
개발자간의 협업에 필요하다. 전체 개발 소스를 공유하면서 개발 파트를 나눌 수 있고 같은 모듈을 개발하더라도 소스를 서로 공유하며 개발할 수 있다.
- 전체 개발 소스를 공유하면서 개발 파트를 나누기 수월해진다.
- 같은 모듈을 개발하더라도 소스를 서로 공유하며 개발할 수 있다.
- 원할 때 예전 버전 내용 전체를 되돌려 볼 수 있으며 복잡한 코드를 개발할 때 이전 버전과 비교하기 수월해진다.

<br>

### Git
> 분산 소스 버전 관리 시스템
- 파일 버전들을 분산해서 관리할 수 있는 도구  
⇒ 소스 코드를 효율적으로 관리할 수 있게 해주는 형상 관리 도구
- 서버를 분산 시켜 구축할 수 있게 하는 소프트웨어로, 지역 저장소(Local)를 만들어 파일, 코드 등의 버전을 관리하고 작업할 수 있다.
- 컴퓨터 파일(주로 소스 코드)의 변경 사항을 지속적으로 추적하고 사용자들 간에 해당 파일들의 작업을 조율할 때 사용한다.

### GitHub
> 깃(Git)을 사용하는 프로젝트를 지원하는 웹호스팅 서비스
- Git을 사용할 수 있는 리모트(원격) 공간/저장소를 제공해주는 곳 중 하나이다.
    - Git을 업로드 할 수 있는 웹사이트
    - 자체 구축이 아닌 클라우드 방식으로 관리되는 버전 관리 시스템
    - 개발자들의 버전 제어 및 공동 작업을 위한 플랫폼
- 깃(Git)은 기본적으로 텍스트 명령어 입력 방식이지만, 깃허브는 그래픽 유저 인터페이스(GUI)를 제공한다.

<br>

### Git의 장점
- 빠른 협업환경 조성
    - 여러 파일 버전을 일관되게 관리 가능
    - 분산개발을 한뒤 merge할수 있음
    - 효율적인 배포관리가 가능함
    - 대부분의 IDE에서 git 연동을 제공한다.
- Branch를 이용해 프로젝트를 가지치기 하여 버전별 관리 가능
    - 파일의 복사가 아닌 스냅샷(다른 점)만 가지고 branch를 생성하기 때문에 자원의 부담이 없다.
- 모든 변경사항 저장
    - 누가/무엇을/어떻게 변경했는지 기록
    - 어떤 데이터도 잃어버리지 않음
    - 이전 상태로 복구 가능
- (GitHub) 이슈 트래커 지원
- (GitHub) Git을 쉽게 공유할 수 있고, 코드 리뷰에 편리하다.

<br>

### Git(GitHub)의 주요 개념들
|개념|의미|
|--|--|
|merge | 한 branch에서 완성한 작업을 다른 branch에 병합|
|tag | 특정 이력을 가지는 commit에 대한 참조|
|push | 로컬에 저장되어 있던 버전 정보를 서버(git 저장소)에 업로드|
|pull | 연결된 원격 저장소(서버)의 최신 내용을 로컬 저장소로 가져오며 병합한다. 자동으로 merge까지 해준다.|
|fetch | 원격 저장소의 변경사항을 로컬 저장소로 가져온다. 자동으로 merge해주진 않는다.|
|rebase | branch의 base를 다시 설정한다. commit log가 깔끔해진다.|

GitHub only
|개념|의미|
|--|--|
|pull request | 완료한 작업을 다른 사람이 리뷰하고 병합하도록 요청|
|issue | 기능에 대한 논의, 버그 추적|
|wiki | 링크들을 연결해 만드는 웹페이지|

<br>

[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#기타)

<br>
