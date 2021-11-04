# Looper와 Handler

## 우지현

### Looper와 Handler의 필요성

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FxDe3n%2FbtqEO0ARSld%2Fw9rGCevC0JaX7zFpZ6xMa1%2Fimg.png" alt="메인 스레드와 서브 스레드 동기화 문제" width="400" height="200"/>

- Handler와 Looper는 안드로이드 내에서 Thread 백그라운드 처리에 사용된다.
- 위의 그림과 같이 메인 쓰레드와 서브 쓰레드에서 textView의 setText 메서드 사용시 동기화 문제가 발생하게 된다.
- 이런 동기화 문제를 처리하기 위해 안드로이드는 메인 쓰레드에서만 UI 작업이 가능하도록 제한해두었다.
- Handler와 Looper를 이용해 서브 쓰레드에서 메인 쓰레드로 UI 처리작업을 전달하거나 메인 쓰레드 자체적으로 처리할 수 있다.

### Looper와 Handler 동작과정

<img src="https://media.vlpt.us/images/yeji/post/6bea1286-602e-464b-a4a8-3dfb90ea3bff/Thread-Looper-Handler.png" alt="Looper와 Handler 동작 과정" width="450" height="500"/>

- Main Thread는 내부적으로 Looper를 가지며, 그 안에 Message Queue가 포함된다.
- Message Queue는 Thread가 다른 Thread나 혹은 자기 자신으로부터 전달받은 메세지를 기본적으로 FIFO 형식으로 보관하는 Queue이다.
- Looper는 Message Queue에서 Message나 Runnable 객체를 차례로 꺼내 Handler가 처리하도록 전달한다.
- Handler는 Looper로부터 받은 Message/Runnable을 실행, 처리하거나 다른 Thread로부터 Message를 받아서 Message Queue에 넣는 역할을 하는 Thread 간의 통신 장치이다.

## 김현수

### Looper와 핸들러
안드로이드 시스템은 기본적으로 하나의 Main Thread(=UI Thread)만을 가지고 있고 보통 이 스레드 안에서 작업을 하는데, UI 관련 작업은 반드시 Main Thread에서 처리를 해야하고 네트워크 작업이나 데이터베이스 작업 등 시간이 오래 걸릴 수 있는 작업은 Main Thread가 아닌 별도의 스레드에서 해야한다.

따라서 다운로드나 업로드와 같이 네트워크 작업은 별도의 스레드에서 하고 해당 스레드에서 작업한 결과를 UI에 보여줄때는 Main Thread에 표시를 해야하는 경우가 발생한다. 이러한 멀티 스레드 환경에서 스레드간 통신을 도와주는 도구가 핸들러와 루퍼(=Handler & Looper)이다.

### 핸들러(Handler)

- 안드로이드에서 사용할 수 있는 대표적인 스레드 통신 방법 중 하나가 Hadler를 통해 Message를 전달하는 것이다.
- Handler를 생성하면 호출한 스레드의 Message Queue와 Looper에 자동으로 연결된다.

### 루퍼(Looper)
- 루퍼는 스레드당 하나씩만 가질 수 있다.
- Message Queue가 비어 있는 동안 아무 행동도 하지 않고, 메시지가 들어오면 해당 메시지를 꺼내 적절한 Handler로 전달한다.
- 새로 생성한 스레드는 루퍼를 가지지 않고 Looper.prepare() 메서드를 호출해야 Looper가 생성된다.
- 루퍼는 무한히 실행되는 메시지 루프를 통해 큐에 메시지가 들어오는지 감시하며 들어온 메시지를 처리할 핸들러를 찾아서 handleMessage()를 호출한다.

### 핸들러와 루퍼를 통한 스레드간 통신
<img src="https://miro.medium.com/max/1000/1*cPvR6xzW8oSMhcUCaJNZ4w.png" width="600px">

1. 핸들러에 있는 sendMessage()를 통해서 Message(=작업)를 전달한다.
2. 핸들러는 받은 Message를 Message Queue에 차례대로 넣는다.
3. Looper가 Message Queue로부터 하나씩 Message를 뽑아서 핸들러로 전달한다.
4. Looper로부터 전달받은 메시지는 handleMessage()를 통해서 작업한다.

<br>

## 이유진
안드로이드에서 스레드간 통신을 도와주는 도구

### 핸들러 (Handler)
Handler를 생성하면 호출한 스레드의 Message Queue와 Looper에 자동으로 연결된다.
#### 핸들러를 통한 스레드간 통신
![](https://miro.medium.com/max/1000/1*cPvR6xzW8oSMhcUCaJNZ4w.png)
1. 핸들러에 있는 sendMessage()를 통해 Message(작업)를 전달한다.
2. 핸들러는 Message Queue에 Message를 차례로 넣는다.
3. Looper가 Message Queue로부터 하나씩 Message를 꺼내서 Handler로 전달한다.
4. Looper로부터 전달받은 메시지는 handleMessage()를 통해서 작업하게 된다.

> `sendMessage(msg: Message): boolean` : 메시지 큐에 message를 만들어서 전달한다.  
> `handleMessage(msg: Message)` : 루퍼를 통해 메시지 큐에서 꺼낸 message나 runnable 처리한다.

#### Message
스레드 통신에서 핸들러에 데이터를 보내기 위한 클래스

### 루퍼 (Looper)
무한히 실행되는 메시지 루프를 통해 큐에 메시지가 들어오는지 감시한다. 메시지가 들어오면 처리할 핸들러를 찾아서 `handleMessage()`를 호출한다. 루퍼는 스레드당 하나씩만 가질 수 있다. 메시지큐가 비어있으면 아무 행동도 하지 않는다.

