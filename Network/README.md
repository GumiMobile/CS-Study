# Network
- [GET, POST 방식의 차이점](#get-post-방식의-차이점)
- [TCP와 UDP의 차이점](#tcp와-udp의-차이점)
- [HTTP와 HTTPS의 차이점](#http와-https의-차이점)
- [Round Robin DNS](#round-robin-dns)
- [웹 통신의 큰 흐름](#웹-통신의-큰-흐름)
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

[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#network)
<br><br>

## TCP와 UDP의 차이점

### TCP와 UDP가 나오게 된 배경

1. IP의 역할은 Host to Host (장치 to 장치)만을 지원한다. 장치에서 장치로의 이동은 IP로 해결되지만, 하나의 장비 안에서 수많은 프로그램들이 통신을 할 경우에는 IP만으로는 한계가 있다.

2. 또한, IP에서 오류가 발생한다면 ICMP에서 알려준다. 하지만 ICMP는 알려주기만 할 뿐 대처를 못하기 때문에 IP보다 위에서 처리를 해줘야 한다.

   > ICMP : Internet Control Message Protocol (인터넷 제어 메시지 프로토콜)
   > 네트워크 컴퓨터 위에서 돌아가는 운영체제에서 오류 메시지를 전송받는 데 주로 쓰임

1번을 해결하기 위해 포트 번호가 생겼고, 2번을 해결하기 위해 상위 프로토콜인 TCP와 UDP가 나오게 되었다.

<br />

### TCP와 UDP

- TCP와 UDP는 OSI 표준모델과 TCP/IP 모델의 전송계층에서 사용되는 프로토콜이다.

![image](https://user-images.githubusercontent.com/76988389/133215254-e89d9880-78e3-4810-bcfa-3d649e5015d6.png)

- 전송계층은 데이터의 전달(송-수신자간 통신 서비스 제공, 패킷 검사, 재전송 요구 제어 등)을 담당한다.
- TCP와 UCP의 차이점은 연결(Connection)과 신뢰도(Unrliable)에 있다.
	- TCP는 송-수신자간 서로 데이터의 송수신이 완료되었다는 확인 메시지를 통해 통신의 신뢰성을 높인다.
	- UDP는 데이터 송수신 확인 절차 없이 데이터를 보낸다.
> **프로토콜** : 컴퓨터 네트워킹을 할 때 서로 약속하는 통신 규약

<br />

### TCP (Transmission Control Protocol)
**인터넷상에서 데이터를 메세지의 형태로 보내기 위해 IP와 함께 사용하는 프로토콜**

#### 특징
- 연결 지향적 프로토콜 : 클라이언트와 서버가 연결된 상태에서 데이터를 주고받는다.
- 연결형 서비스로 가상 회선 방식을 제공 : 발신지와 수신지를 연결하여 패킷을 전송하기 위한 논리적 경로를 배정한다.
- 수신여부를 확인한다.
- 높은 신뢰성을 보장한다.
	- 데이터의 분실, 중복, 순서가 뒤바뀜 등을 자동으로 보정해줘서 송수신 데이터의 정확한 전달을 할 수 있도록 해준다. 
	- 세그먼트가 유실될 경우 재전송을 통해 복구해준다.
- 흐름 제어 및 혼잡 제어 -> UDP보다 속도가 느리다.
- 일반적으로 TCP와 IP를 함께 사용하는데, IP가 데이터의 배달을 처리한다면 TCP는 패킷을 추적 및 관리한다. 
- 3-way handshaking과정을 통해 Connection을 형성한 뒤에 정보의 송수신이 이루어지고, 4-way handshaking을 통해 해제한다.
- 모든 TCP 연결은 전이중(full-duplex), 점대점(point to point) 방식이고 멀티캐스팅이나 브로드캐스팅을 지원하지 않는다.
	- 전이중 (Full-Duplex) : 전송이 양방향으로 동시에 일어날 수 있다.
	- 점대점 (Point to Point) : 각 연결이 정확히 2개의 종단점을 가지고 있다.

#### 장점
- 데이터의 전송 순서를 보장한다.
- 데이터를 정확하고 안정적으로 보낼 수 있다.
- 데이터 흐름 제어와 혼잡 제어에 용이하다.

#### 단점
- 1:1 통신만 가능하다.
- 반드시 연결이 형성되어야 한다.
- 통신 선로의 길이에 UDP보다 영향을 많이 받는다.

<br />

### 차세대 HTTP/3는 UDP를 사용한다
#### TCP의 문제점 
현재 HTTPS는 보안을 위해 TCP+TLS를 쓰는데, 핸드쉐이크 과정이 매우 복잡하고 최소 100ms시간이 소요된다. 반면 UDP는 핸드쉐이크 과정이 없으므로 빠르다.

<br />

### UDP (User Datagram Protocol)
**데이터를 독립적인 관계를 지니는 패킷 단위로 처리하는 프로토콜**

#### 특징
- 비연결형 프로토콜
	- 연결을 위해 할당되는 논리적인 경로가 없으며, 연결을 설정하고 해제하는 과정이 존재하지 않는다.
	- 각각의 패킷은 다른 경로로 전송되고 독립적인 관계를 지니게 된다.
- UDP헤더의 CheckSum 필드를 통해 최소한의 오류만 검출한다.
- 전송한 데이터그램이 유실 될 수 있고, 순서가 바뀌어 도착할 수 있다.
- 패킷에 순서를 부여하여 재조립을 하거나 흐름 제어 또는 혼잡 제어와 같은 기능도 처리하지 않는다.
	- 신뢰성이 낮다, TCP보다 속도가 빠르다, 네트워크 부하가 적다.
	- 신뢰성보다는 연속성이 중요한 서비스 (ex. 실시간 서비스(streaming))에 자주 사용된다.
- UDP를 사용하는 것들에는 DNS(Domain Name Service)가 있다.

#### 장점
  - TCP보다 데이터의 전송이 뻐르다.
  - 패킷 오버헤드가 적어 네트워크 부하가 감소된다.
#### 단점
  - 데이터의 신뢰성이 낮다.
  - 패킷의 송신 순서와 수신 순서가 다를 수 있다.
  - 패킷이 잘못된 선로로 전송되어 유실될 수 있다.
  - 의미있는 서버를 구축하기 위해 일일이 패킷을 관리해야 한다.

> 왜 DNS는 UDP를 사용할까?
>
> - Request의 양이 작아서 UDP Request에 담을 수 있다.
>
> - TCP는 데이터를 송신할 때까지 세션 확립을 위한 처리를 하고, 송신한 데이터가 수신되었는지 점검하는 과정이 필요하지만 UDP는 3-way handshaking으로 연결을 유지할 필요가 없어 오버헤드가 작아진다.
>
> - Request에 대한 손실은 Application Layer에서 제어가 가능하다.
>
> - 하지만 Zone Transfer를 사용하거나 데이터가 512 bytes를 넘는 경우, 응답을 못 받은 경우는 TCP를 사용한다.
>
>   Zone Transfer : DNS 서버 간의 요청을 주고 받을 때 사용하는 transfer

<br />

#### QUIC(Quck UDP Internet Connections)
TCP의 성능을 개선하고자 UDP를 채택한 기술이다. 또한, UDP는 데이터 전송에만 초점을 맞추고 설계되어 헤더에 아무것도 없기 때문에 커스터마이징이 용이하다. 따라서 개발자의 구현에 따라 신뢰성을 높일 수 있고, 기능 확장이 쉽다.
- 전달 속도의 개선
- 클라이언트와 서버의 연결수 최소화
- 대역폭을 예상하여 패킷 혼잡(congestion)을 피함

> 참고 : [HTTP/3는 왜 UDP를 선택한 것일까?](https://evan-moon.github.io/2019/10/08/what-is-http3/) 

<br />

### 3-way handshake : 연결

  TCP는 정확한 전송을 보장해야 한다. 따라서 통신하기에 앞서, 논리적인 접속을 성립하기 위해 3-way handshake 과정을 진행한다. 신뢰성 있는 연결을 맺어주는 TCP의 3-way handshake 방식은 다음과 같다.

  <br />

  ![3-way handshake](https://t1.daumcdn.net/cfile/tistory/225A964D52F1BB6917)

  <br />

  [STEP 1] 

  A 클라이언트는 B 서버에 접속을 요청하는 SYN 패킷을 보낸다. 이때 A 클라이언트는 SYN을 보내고 SYN/ACK 응답을 기다리는 SYN_SENT 상태가 된다.

  [STEP 2]

  B 서버는 SYN 요청을 받고 A 클라이언트에게 요청을 수락한다는 ACK와 SYN flag가 설정된 패킷을 발송하고 A가 다시 ACK으로 응답하기를 기다린다. 이때 B 서버는 SYN_RECEIVED 상태가 된다.

  [STEP 3] 

  A 클라이언트는 B 서버에게 ACK를 보내고 이후로부터는 연결이 이루어지고 데이터가 오가게 된다. 이때의 B 서버 상태가 ESTABLISHED다. 

<br />

### 4-way handshake : 연결 해제

  3-way handshake는 TCP의 연결을 초기화할 때 사용한다면, 4-way handshake는 세션을 종료하기 위해 수행되는 절차다. 

  <br />

  ![4-way handshake](https://t1.daumcdn.net/cfile/tistory/2152353F52F1C02835)

  <br />

  [STEP 1]

  클라이언트가 연결을 종료하겠다는 FIN 플래그를 전송한다.

  [STEP 2]

  서버는 일단 확인 메시지를 보내고 자신의 통신이 끝날 때까지 기다리는데 이 상태가 TIME_WAIT 상태다.

  [STEP 3]

  서버가 통신이 끝났으면 연결이 종료되었다고 클라이언트에게 FIN 플래그를 전송한다.

  [STEP 4]

  클라이언트는 확인했다는 메시지를 보낸다.

  

  그런데 만약 "서버에서 FIN을 전송하기 전에 전송한 패킷이 Routing 지연이나 패킷 유실로 인한 재전송 등으로 인해 FIN 패킷보다 늦게 도착하는 상황"이 발생한다면?

  

  클라이언트에서 세션을 종료시킨 뒤 뒤늦게 도착하는 패킷이 있다면 이 패킷은 drop되고 데이터는 유실될 것이다. 이러한 현상에 대비하여 클라이언트는 서버로부터 FIN을 수신하더라도 일정시간(디폴트 240초) 동안 세션을 남겨놓고 잉여 패킷을 기다리는 과정을 거치게 되는데 이 과정이 "TIME_WAIT"다.

[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#network)
<br><br>

## HTTP와 HTTPS의 차이점

### HTTP (HyperText Transfer Protocol)

- 서로 다른 시스템들 사이에서 통신을 주고 받게 해주는 가장 기초적인 프로토콜

  > 프로토콜 : 컴퓨터 내부에서, 또는 컴퓨터 사이에서 데이터의 교환 방식을 정의하는 규칙 체계

- 인터넷 상에서 클라이언트와 서버가 자원을 주고 받을 때 쓰는 통신 규약

- 애플리케이션 레벨의 프로토콜로 TCP/IP 위에서 작동한다.

- HTTP는 80번 포트를 이용한다. 즉, 클라이언트는 80번 포트로 요청을 보내고 HTTP 서버도 80번 포트에서 요청을 받는다.

- 상태를 가지고 있지 않는 Stateless 프로토콜이며 Method, Path, Version, Headers, Body 등으로 구성된다.

- 단순한 정보 조회 등만을 처리할 때 사용한다.

### HTTP의 문제점

- HTTP는 평문 통신이기 때문에 도청이 가능하다.

- 통신 상대를 확인하지 않기 때문에 위장이 가능하다.

- 완전성을 증명할 수 없기 때문에 변조가 가능하다.

- 즉, 보안이 취약하다. 위의 문제들은 다른 암호화하지 않은 프로토콜에도 공통되는 문제점들이다. 이런 문제를 해결해주는 프로토콜이 HTTPS이다.

  [Reference](https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/Network#http%EC%99%80-https)

### HTTPS (HyperText Transfer Protocol Secure)

- HTTPS는 HTTP에 암호화와 인증, 그리고 완전성 보호를 더한 것이다.
  - HTTP는 원래 TCP와 직접 통신했지만, HTTPS에서 HTTP는 SSL(Secure Socket Layer)과 통신하고 SSL이 TCP와 통신하게 된다.
  - TLS 보안프로토콜도 사용하여 보안을 유지한다.
  - SSL을 사용한 HTTPS는 암호화와 증명서, 안정성 보호를 이용할 수 있게 된다.
- HTTPS의 SSL에서는 공통키 암호화 방식과 공개키 암호화 방식을 혼합한 하이브리드 암호 시스템을 사용한다.
  - 공개키 방식으로 대칭키를 전달하고, 공유된 대칭키를 가지고 통신하게 된다.
  - SSL 방식을 적용하려면 인증서를 발급받아 서버에 적용해야 한다. 인증서는 의도한 서버로 접속했는지 보장하는 역할을 한다.
  - 인증서를 바급하는 기관을 CA(Certificate authority)라고 한다.
- 기본 TCP/IP 포트는 443번이다.
- 암호화/복호화 과정이 필요하기 때문에 HTTP보다 속도가 느리다.
- 인증서를 발급하고 유지하기 위한 추가 비용이 발생한다.
- 검색 엔진 최적화와 가속화된 모바일 페이지를 만들 수 있다. (구글)

> **TLS** (Transport Layer Security)
>
> - 데이터 무결성 제공
> - 데이터가 전송 중 수정되거나 손상되는 것 방지
> - 사용자가 의도하는 웹사이트와 통신하고 있음을 입증하는 인증 기능

#### 👀 총평

HTTP는 보안에 취약한 반면 속도가 빠르다 : 단순 정보 조회 시 사용

HTTPS는 보안이 뛰어난 반면 인증서 발급 및 유지를 위한 추가 비용이 발생하고, 속도가 느리다(큰 차이는 없다) : 민감 정보 전달/제공 시 사용

### 모든 웹 페이지에서 HTTPS를 사용해도 될까?

암호화 통신은 CPU, 메모리 등의 리소스를 더 많이 요구하기 때문에 서버 한 대당 처리할 수 있는 리퀘스트의 수가 상대적으로 줄어들게 된다. 하지만 최근에는 하드웨어의 발달로 HTTPS를 사용하더라도 속도 저하가 거의 일어나지 않으며, HTTPS가 HTTP보다 더 빠르게 동작한다. 따라서 과거에는 민감한 정보를 다룰 때만 HTTPS에 의한 암호화 통신을 사용했지만 현재는 모든 웹 페이지에 HTTPS를 적용하는 방향으로 바뀌어가고 있다.

[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#network)
<br>

## Round Robin DNS
### Round Robin

시분할 시스템을 위해 설계된 선점형 스케줄링 방법

컨슈머들 사이에 우선순위를 두지 않고, 순서대로 시간단위로 자원을 분배한다.

> 선점형 스케줄링?
>
> 어떤 프로세스가 cpu를 점유하고 있어도 다른 프로세스가 cpu를 강제로 점유할 수 있다. 모든 프로세스에게 동일한 사용시간을 부여할 수 있다.

### DNS (Domain name system)

사용자는 숫자로 된 ip주소보다 문자로 된 도메인 주소를 기억하는게 쉽다. 따라서 도메인 주소를 ip 주소로 변환하는 과정이 필요하다. DNS는 클라이언트로부터 도메인을 요청받아 ip주소를 반환해 주는 시스템이다.

### Round Robin DNS의 원리

![라운드 로빈 DNS를 이용한 로드 밸런싱](https://camo.githubusercontent.com/cde72c56ccd59c728054846a71770e0b378f3319ef2337ae97e88c9c94669282/68747470733a2f2f696d67312e6461756d63646e2e6e65742f7468756d622f523132383078302f3f73636f64653d6d746973746f72793226666e616d653d6874747073253341253246253246626c6f672e6b616b616f63646e2e6e6574253246646e2532467959327363253246627471345642706a6d414d2532466b53764252507872555577474b564f62546430386b31253246696d672e706e67)

웹서비스를 제공하는 서버들은 자신의 공인 ip를 각각 가지고 있다. 

DNS는 요청이 들어오면, 라운드 로빈 방식으로 도메인 정보를 조회하여 하나 이상의 공인 ip를 제공한다.

결국, 사용자들은 여러 서버에 나눠져 접속하므로 서버의 부하가 분산된다.

### 장점

- 적은 비용으로 트래픽 분산이 가능하다

- 별도의 로드 밸런서가 필요하지 않다.

  > 로드 밸런싱?
  >
  > 하나의 인터넷 서비스가 발생하는 트래픽이 많을 때 여러 대의 서버가 분산처리하여 서버의 로드율 증가, 부하량, 속도저하 등을 고려하여 적절히 분산처리하여 해결하는 시스템.

### 단점

- DNS 서버는 각각의 공인 ip를 반환하기 때문에 서버의 수에 맞게 공인 ip (비용 발생)가 필요하다.

- 균등하게 분산되지 않는다. 특히 모바일 환경에서는 캐리어 게이트웨이라는 프록시 서버를 경유하는데, 프록시 서버에서는 이름 변환 결과가 일정 기간 캐싱되므로 그 기간동안 같은 서버로 접속된다.

- DNS 서버는 웹 서버의 부하, 사용자 접속 수, 상태를 알지 못하므로 문제가 생긴 서버를 필터링하지 않고 제공한다.

  > 프록시 서버?
  >
  > 클라이언트가 자신을 통해 다른 네트워크 서비스에 간접적으로 접속할 수 있게 하는 시스템. 프록시 서버 중 일부는 프록시 서버에 요청된 내용들을 캐싱한다. 같은 요청이 들어오면 캐싱된 데이터를 제공하므로 시간절약과 외부 트래픽 감소를 통한 네트워크 병목 현상을 방지 할 수 있다.

### 해결 방법

- 다중화 구성 방식 (Synchronous Time Division Multiplexing)
  - AP 서버에 VIP(Virtual IP)를 부여하여 다중화를 구성한다.
  - 각 AP 서버 Health Check 후, 이상이 감지되면 VIP를 정상 AP 서버로 인계한다.
  - 즉, DNS Server Table에 실시간으로 AP 서버의 상태를 확인할 수 있는 컬럼 및 함수를 추가하고, 요청을 받으면 서버 상태를 확인 후 우회루트를 제공하거나 에러를 전송한다.
- 가중 라운드 로빈 (Weighted Round Robin)
  - 각각의 웹 서버에 가중치를 추가하여 분산 비율을 변경한다. 성능이 좋은 서버의 가중치를 높게 설정해야 한다.
- Least Connection
  - Round Robin 방식을 기반으로 한다.
  - 접속 클라이언트 수가 가장 적은 서버를 선택한다.
  - 별도의 로드 밸런서에서 실시간으로 connection 수를 관리하거나 각 서버에 알려야 한다.

[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#network)

## 웹 통신의 큰 흐름

### 웹 통신의 과정
	
![웹 통신의 과정](https://media.vlpt.us/images/woo0_hooo/post/e119383c-61cc-46d5-a85d-b27b65ddee1e/Untitled.png)

1. 사용자가 웹 브라우저를 통해 URL을 입력한다
2. 입력된 URL 중 도메인 네임을 DNS 서버에서 검색한다
3. DNS 서버에서 해당 도메인 네임에 해당하는 IP 주소를 찾아 사용자가 입력한 URL 정보와 함께 전달한다
4. 웹 페이지 URL 정보와 전달받은 IP 주소를 이용해 HTTP 요청 메시지를 생성한다
5. 요청은 TCP를 통해 서버로 전송된다
6. 서버는 클라이언트의 요청을 받고 응답을 전송한다

### in 브라우저

1. url에 입력된 값을 브라우저 내부에서 결정된 규칙에 따라 그 의미를 조사한다.
2. 조사된 의미에 따라 HTTP Request 메시지를 만든다.
3. 만들어진 메시지를 웹 서버로 전송한다.

이 때 만들어진 메시지 전송은 브라우저가 직접 하는 것이 아니다. 브라우저는 메시지를 네트워크에 송출하는 기능이 없으므로 OS에 의뢰하여 메시지를 전달한다. 단, OS에 송신을 의뢰할 때는 도메인명이 아니라 IP주소로 메시지를 받을 상대를 지정해야 하는데, 이 과정에서 DNS 서버를 조회해야 한다.

#### URL의 구조

``` scheme:[//[user:password@]host[:port]][/]path[?query][#fragment]``` 

- URL은 제일 앞에 자원에 접근할 방법을 정의해 둔 프로토콜을 적는다. http, ftp, usenet, telnet 등
- 프로토콜 이름 다음에는 프로토콜 이름을 구분하는 `:`을 적는다.
- `:` 또는 `//`뒤에는 프로토콜마다 특화된 정보를 넣는다.

만약 URL이 문법에 맞지 않는다면 웹 브라우저의 검색엔진으로 검색을 요청하고, <br>
URL이 문법에 맞으면 Punycode encoding을 URL의 host부분에 적용한다.

> Punycode
>
> 퓨니코드는 유니코드 문자열을 호스트 이름에서 허용된 문자만으로 인코딩하는 방법이다. 퓨니코드는 유니코드가 지원하는 모든 언어로 국제화 도메인을 쓸 수 있게한 IDNA (Internationalised Domain Names in Applications)의 일부로, 변환은 전적으로 웹 브라우저와 같은 클라이언트에서 이루어진다.

### in 프로토콜 스택, LAN 어댑터

1. 프로토콜 스택이 브라우저로부터 메시지를 받는다.

   > 프로토콜 스택 : 운영체제에 내장된 네트워크 제어용 소프트웨어

2. 브라우저로부터 받은 메시지를 패킷 속에 저장한다.

3. 그리고 수신처 주소 등의 제어정보를 덧붙인다.

4. 그런 다음, 패킷을 LAN 어댑터에 넘긴다.

5. LAN 어댑터는 다음 Hop의 MAC주소를 붙인 프레임을 전기신호로 변환시킨다.

6. 신호를 LAN 케이블에 송출시킨다.

프로토콜 스택은 통신 중 오류가 발생했을 때, 이 제어 정보를 사용하여 고쳐 보내거나, 각종 상황을 조절하는 등 다양한 역할을 하게 된다. 네트워크 세계에서는 비서가 있어서 우리가 비서에게 물건만 건네주면, 받는 사람의 주소와 각종 유의사항을 써준다. 여기서는 프로토콜 스택이 비서의 역할을 한다고 볼 수 있다.

### in 허브, 스위치, 라우터

1. LAN 어댑터가 송신한 프레임은 스위칭 허브를 경유하여 인터넷 접속용 라우터에 도착한다.
2. 라우터는 패킷을 프로바이더(통신사)에게 전달한다.
3. 인터넷으로 들어가게 된다.

### in 액세스 회선, 프로바이더

1. 패킷은 인터넷의 입구에 있는 액세스 회선(통신 회선)에 의해 POP(Point Of Presence, 통신사용 라우터)까지 운반된다.
2. POP를 거쳐 인터넷의 핵심부로 들어가게 된다.
3. 수많은 고속 라우터들 사이로 패킷이 목적지를 향해 흘러가게 된다.

### in 방화벽, 캐시 서버

1. 패킷은 인터넷 핵심부를 통과하여 웹 서버측의 LAN에 도착한다.
2. 기다리고 있던 방화벽이 도착한 패킷을 검사한다.
3. 패킷이 웹 서버까지 가야하는지 가지 않아도 되는지를 판단하는 캐시서버가 존재한다.

굳이 서버까지 가지 않아도 되는 경우를 골라낸다. 액세스한 페이지의 데이터가 캐시 서버에 있으면 웹 서버에 의뢰하지 않고 바로 그 값을 읽을 수 있다. 페이지의 데이터 중에 다시 이용할 수 있는 것이 있으면 캐시 서버에 저장된다.

### in 웹 서버

1. 패킷이 물리적인 웹 서버에 도착하면 웹 서버의 프로토콜 스택은 패킷을 추출하여 메시지를 복원하고 웹 서버 애플리케이션에 넘긴다.
2. 메시지를 받은 웹 서버 애플리케이션은 요청 메시지에 따른 데이터를 응답 메시지에 넣어 클라이언트로 회송한다.
3. 왔던 방식대로 응답 메시지가 클라이언트에게 전달된다.

[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#network)
