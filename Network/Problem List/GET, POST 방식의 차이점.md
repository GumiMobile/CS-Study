## GET, POST 방식의 차이점

### 우지현

GET과 POST는 HTTP 메서드로 클라이언트에서 서버로 무언가를 요청할 때 사용한다.

그렇다면 각 방식의 특징과 GET과 POST의 차이점은 무엇일까?

- GET

  클라이언트에서 서버로부터 정보를 조회하기 위해 설계된 메서드

  요청하는 데이터가 HTTP Request Message의 Header 부분에 url로 담겨서 전송됨

  url 끝에 ?와 함께 이름과 값으로 쌍을 이루는 요청 파라미터로 포함되어 전송되는데 이 부분을 쿼리 스트링(query string)이라고 한다

  예시) http://www.example-url.com/resources?name1=value1&name2=value2

  url에 데이터가 담겨가기 때문에 전송할 수 있는 데이터의 크기가 제한적이다

  또 데이터가 url에 그대로 노출되므로 보안이 필요한 데이터에 대해서는 GET 방식은 적절하지 않다

  GET 방식의 요청은 브라우저에서 Caching 할 수 있어서 기존에 caching 되었던 데이터가 응답될 가능성이 있다

- POST 

  클라이언트에서 서버로 리소스를 생성/변경하기 위해 설계된 메서드

  따라서 전송해야될 데이터를 HTTP Request Message의 Body에 담아서 전송

  HTTP Message의 Body는 길이의 제한이 없기 때문에 대용량 데이터 전송 가능

  데이터가 Body로 전송되고 내용이 보이지 않기 때문에 GET보다 보안적인 면에서 안전하다고 생각할 수 있지만 크롬 개발자 도구, Fiddler와 같은 툴로 요청 내용을 확인할 수 있기 때문에 민감한 데이터의 경우 반드시 암호화가 필요함

- GET과 POST의 차이

  GET은 멱등(Idempotent), POST는 멱등이 아님(Non-idempotent)

  💡 멱등성 (Idempotent) : 수학이나 전산학에서 연산의 한 성질을 나타내는 것으로, 연산을 여러 번 적용하더라도 결과가 달라지지 않는 성질

  GET은 서버에게 동일한 요청을 여러 번 전송하더라도 동일한 응답이 돌아오기 때문에 주로 조회를 할 때 사용

  POST는 서버에게 동일한 요청을 여러 번 전송해도 응답은 항상 다를 수 있기 때문에 서버의 상태나 데이터를 변경할 때 사용

### 이수형

GET과 POST방식은 클라이언트에서 서버로 요청을 보내는 HTTP 메소드의 종류이다

- GET 방식 특징

  데이터를 Header부분에 포함하여 전송하는 방법 

  URL 끝에 데이터를 ? 마크를 포함하여 요청하는 방식으로 데이터가 노출되어 보안에 취약함 

  이 데이터는 Key와 Value로 작성되어있으며 이를 쿼리스트링이라고 하며 요청하는 파라미터가 여러개인경우 &를 이용하여 연결함

  GET방식은 Caching이 가능하며 브라우저가 기존에 Caching되었던 데이터를 불러올수있고 이는 브라우저 기록에 남는다는것과 북마크 추가가 될수 있다

  그러므로 주로 조회하는데에 많이 사용된다

- POST방식 특징

  데이터를 Body부분에 담아서 전송하는 방법

  URL에 데이터가 노출되지않고 브라우저 기록에 남지않아 보안에 강하지만 서버에게 동일한 요청을 보내도 다른응답을 받을 수 있음

  주로 복잡한 형태의 데이터 전송이나 데이터를 변경시킬때 많이 사용되는 방법

### 김현수

GET
- 서버에서 값을 가져온다.
- 요청하는 데이터를 HTTP Request Message의 Header 부분의 url에 담겨서 전송한다.(주소 뒤에 이름과 값이 결합된 스트링 형태로 전달)
- 전송할 수 있는 데이터의 크기가 제한적이다
- POST방식보다 전송 속도가 빠르다.
- 보안성이 낮다.
- 캐싱할 수 있다.

POST
- 서버의 값이나 상태를 변경 또는 추가한다.
- 데이터가 HTTP Message의 Body 부분에 담겨서 전송된다.
- 전송할 수 있는 데이터의 크기에 제한이 없다.
- GET방식보다 전송 속도가 느리다.
- GET방식보다는 보안성이 높다.
- 캐싱할 수 없다.


### 이유진
둘 다 HTTP프로토콜을 이용해서 서버에 요청할 때 사용하는 방식이다.

**GET**은 요청하는 데이터가 HTTP Request Message에 URL로 담겨서 전송된다. (-> Query String이라고 부른다. `?변수명=값&변수명=값…`) url의 크기가 한정되어있기 때문에 전송할 수 있는 데이터의 크기가 제한적이다. 또한, 데이터가 url에 노출되기 때문에 보안성이 떨어진다. 따라서 서버에서 값을 가져올 때 주로 사용한다.

**POST**의 요청데이터는 HTTP Request Message의 body에 담겨서 전송된다. 따라서 get보다 많은 양의 데이터를 전송할 수있고, 상대적으로 보안성이 낫다. post는 서버에 값을 추가하거나 변경할 때 사용된다.

post와 달리 get은 캐싱/브라우저히스토리/북마크 가 가능하다.


### 윤상일

HTTP가 제공하는 7가지 메서드(GET, POST, HEAD, PUT, OPTION, DELETE, TRACE)가 있다. 요즘은 GET, POST를 제외한 나머지 메서드를 비활성화는 편인데, 보안 취약점이 생길 수 있기 때문이다.

HTTP통신 시 주고받는 데이터를 HTTP 패킷이라 한다. 패킷의 구조는 크게 Header 영역과 Body 영역으로 나눌 수 있다. Header에는 크게 여러가지 정보와 어떠한 방식의 메서드를 사용하였는지 적게 된다. Body의 경우, 메서드 방식에 따라 사용 유무와 사용 방법이 다르다.

#### GET 방식

- [특징]
  URL에 Parameter를 붙여서 전송 => 따라서 Body 영역 사용하지 않음 => 대용량 데이터 전송이 제한된다.  
  Idempotent(역동적인, 가변적인)하다 => CRUD의 Read에 해당하는 만큼, Select 용도의 개발에 이용한다.  
  캐싱을 이용하기 때문에 POST방식보다 빠르다.  
  
- [예시]  

  https://www.google.com/search?q=get+post+%EB%B0%A9%EC%8B%9D&newwindow=1&sxsrf=AOaemvLzvkfAJBeAZWpujHIZeqXrKOkp-A%3A1630972821590&ei=las2Yae3I4GxmAXo0p34Bg&oq=GET&gs_lcp=Cgdnd3Mtd2l6EAMYADIECCMQJzIECCMQJzIICAAQgAQQsQMyCAgAEIAEELEDMgsIABCABBCxAxCDATILCAAQgAQQsQMQgwEyCwgAEIAEELEDEIMBMgUIABCABDIFCAAQgAQyCwgAEIAEELEDEIMBOgcIIxDqAhAnSgQIQRgAUNKRpwJYupynAmDaqKcCaANwAHgAgAHWAogB5AqSAQcwLjEuNC4xmAEAoAEBsAEKwAEB&sclient=gws-wiz



#### POST 방식
  
- [특징]
  URL이 아닌 Body 영역에 데이터를 실어 보내기 때문에 데이터 전송량에 제한이 없으며, 대용량 데이터 전송에 유리하다.  
  Body 영역 데이터 타입을 Header Content-Type에 명시해줘야 한다.  
  non-idempotent => CRUD의 Create에 해당하는 만큼, 결과값이 바뀌는 유형의 개발에 이용한다.  
  캐싱을 사용할 수 없다.  
- [예시] BOJ  



#### 비교

- 보안 취약성  
  GET방식이 URL에 정보를 담아 보내기 때문에 정보의 접근은 더 쉽지만, POST방식도 Body 영역의 데이터를 들여다볼 수 있기 때문에 보안 성능이 특별히 더 뛰어난 것은 아니다. 보안을 위해서는 두 방식 모두 데이터를 암호화해야 한다.
- 속도  
  GET방식은 캐싱을 사용하므로 POST방식보다 빠르다.
