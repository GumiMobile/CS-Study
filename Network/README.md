# Network
- [GET, POST 방식의 차이점](#GET,-POST-방식의-차이점)
<br><br>

## GET, POST 방식의 차이점
### HTTP 요청 메서드
HTTP가 제공하는 7가지 메서드(GET, POST, HEAD, PUT, OPTION, DELETE, TRACE)가 있다.
요즘은 GET, POST를 제외한 나머지 메서드를 비활성화는 편인데, 보안 취약점이 생길 수 있기 때문이다.

HTTP통신 시 주고받는 데이터를 HTTP 패킷이라 한다. 패킷의 구조는 크게 Header 영역과 Body 영역으로 나눌 수 있다. Header에는 크게 여러가지 정보와 어떠한 방식의 메서드를 사용하였는지 적게 된다. Body의 경우, 메서드 방식에 따라 사용 유무와 사용 방법이 다르다.

### GET
- 요청하는 데이터가 HTTP Request Message의 Header 부분에 url로 담겨서 전송된다.
  - url 끝에 ?와 함께 쿼리 스트링(query string)으로 전달된다.
  - 쿼리 스트링은 Key와 Value로 작성되어있으며 요청하는 파라미터가 여러개인경우 &를 이용하여 연결한다.
  - 예시) http://www.example-url.com/resources?name1=value1&name2=value2

- url의 크기가 한정되어있기 때문에 전송할 수 있는 데이터의 크기가 제한적이다.

- 또 데이터가 url에 그대로 노출되므로 보안이 필요한 데이터에 대해서는 GET 방식은 적절하지 않다.

- post와 달리 get은 캐싱/브라우저 히스토리/북마크 가 가능하며, 기존에 caching 되었던 데이터가 응답될 가능성이 있고 POST방식보다 전송 속도가 빠르다.

- CRUD의 Read에 해당하는 만큼, 서버에서 값을 가져오는 Select 용도의 개발에 주로 이용한다.

### POST
- 데이터를 HTTP Request Message의 Body에 담아서 전송하며, Body 영역 데이터 타입을 Header Content-Type에 명시해줘야 한다.

- HTTP Message의 Body는 길이의 제한이 없기 때문에 대용량 데이터 전송이 가능하다.

- URL에 데이터가 노출되지않고 브라우저 기록에 남지 않아 GET보다 보안적인 면에서 안전하다고 생각할 수 있지만 크롬 개발자 도구, Fiddler와 같은 툴로 요청 내용을 확인할 수 있기 때문에 민감한 데이터의 경우 반드시 암호화가 필요하다.

- GET방식보다 전송 속도가 느리다.

- 서버에 값 추가, 복잡한 형태의 데이터 전송, 데이터를 변경시킬때 많이 사용되는 방법이다.

### 멱등성
💡 멱등성 (Idempotent) : 수학이나 전산학에서 연산의 한 성질을 나타내는 것으로, 연산을 여러 번 적용하더라도 결과가 달라지지 않는 성질

GET은 멱등(Idempotent), POST는 멱등이 아님(Non-idempotent)

- GET은 서버에게 동일한 요청을 여러 번 전송하더라도 동일한 응답이 돌아오기 때문에 주로 조회를 할 때 사용한다.

- POST는 서버에게 동일한 요청을 여러 번 전송해도 응답은 항상 다를 수 있기 때문에 서버의 상태나 데이터를 변경할 때 사용한다.
