# 코루틴
## 김민수

### 코루틴

비동기적으로 실행되는 코드를 간소화하기 위한 동시 설계 패턴

- Light-weight thread
- 메인, 서브 루틴에 관계없이 동등하게 루틴 실행
- 부모 코루틴이 취소되면 자식 코루틴도 취소됨

``` kotlin
1. 이렇게 코딩하고 싶다..
val user = getUserFromServer() // 그러나 NetworkOnMainThreadException
textview.text = user.name
// 네트워크 작업은 메인 스레드에서 할 수 없음

2. Thread 처리
Thread {
	val user = getUserFromServer()
	textview.text = user.name // CalledFromWrongThreadException
}
// view는 메인 스레드에서만 접근 가능

3. Callback
getUserFromServer { user -> // welcome to callback hell...
    getDataWithUser(user) { data ->
	textView.text = data	                          
      	textview.text = user.name
    }
}

4. coroutine!!
suspend fun loadUser() {
	val user = getUserFromServer() // withContext로 인해 user를 받아온 뒤 show() 실행
	show(user)
}

suspend fun getUserFromServer() = withContext(Dispatchers.IO) {
	// getting data on network..
}
```

### 코루틴 스코프

기본 스코프: CoroutinScope

- 코루틴을 실행할 범위를 나타냄

글로벌 스코프: GlobalScope

- 싱글톤으로 만들어 짐
- 앱이 종료될 때 까지 존재하는 스코프
- 시스템 전체에 악영향을 끼칠 수 있음
- cancel, Exception 처리가 어려움

### 코루틴 컨텍스트

- Dispatcher를 통한 스레드풀 선택

  - Dispatchers.IO

    백그라운드 스레드에서 작동하며, 로컬 DB, 네트워크, 파일 작업에 사용

  - Dispatchers.Main

    메인스레드에서 작동하며, ui, suspend, livedata 수정사항 수신 등 작고, 가벼운 작업 실행

  - Dispatchers.Default

    CPU부하가 많은 작업 수행

  - Dispatchers.Unconfined

    GlobalScope에서 사용하며, 현재 스레드에서 작동한다. 안드로이드 개발에는 사용 지양

- '+' 연산자를 이용한 컨텍스트 결합

  - Dispatchers.IO + CoroutineName("name...")와 같이 디버깅을 위해 name을 컨텍스트에 결합 가능


### 코루틴 빌더

- .launch() : Job 반환
  - 결과가 없는 코루틴 생성, 반환된 Job으로 해당 코루틴 제어
  - job.cancel(), job.cancelAndJoin(), job.join()
  - 비동기 작업을 수행하지만 결과 값을 return하지 않음

- .async() : Deferred<T> 반환
  - 미래의 계산 결과가 예상되는 비동기 작업에 사용
  - 결과를 얻어 다른 작업에 사용할 때 결과는 Deferred<T>에 받음
  - await()로 대기, 결과값 수신
- withContext() : T 반환
  - 결과 리턴까지 대기함
  - 결과는 T로 반환 받음
  - 코루틴이 취소될 때 finally{} 구문 내에서 withContext 사용하면 자원반환 등 여러 작업을 끝까지 시행할 수 있음
- runBlocking() : T 반환
  - 새로운 코루틴을 시작하고 내부 코루틴이 완료될 때까지 현재 스레드 block
  - 안드로이드 메인 스레드에서 runBlocking 사용시, 5초 이상 작업하면 ANR 발생

## 김현수
	
### 코루틴이란?
- 코루틴의 코는 'co(함께, 동시에)'라는 의미를 가지고 있다. 즉, 동시성 프로그래밍 개념을 코틀린에 도입한 것이 코루틴이다.
- 코루틴은 코루틴이 시작된 스레드를 중단하지 않으면서 비동기적으로 실행되는 코드이다.
- 기존의 복잡한 AsyncTask 또는 다수 스레드 관리를 직접 해주지 않아도 되며, 기존 다중 스레드보다 훨씬 효율적으로 동작한다.

### 주요 키워드
- CoroutineScope
	- CoroutineScope 는 말 그대로 코루틴의 범위, 코루틴 블록을 묶음으로 제어할수 있는 단위이다.
	- GlobalScope 는 CoroutineScope 의 한 종류이다. 미리 정의된 방식으로 프로그램 전반에 걸쳐 백그라운드에서 동작한다.

- CoroutineContext
	- 코루틴을 어떻게 처리할것인지에 대한 여러가지 정보의 집합.
	- 주요 요소로는 Job과 dispatcher가 있다.

- Dispatcher
	- CoroutineContext 을 상속받아 어떤 스레드를 이용해서 어떻게 동작할것인지를 미리 정의한다다.
	- Dispatcher 의 용도
		- Dispatchers.Default : CPU 사용량이 많은 작업에 사용한다. 주 스레드에서 작업하기에는 너무 긴 작업에 알맞다.
		- Dispatchers.IO : 네트워크, 디스크 사용 할때 사용한다. 파일을 읽고 쓰고, 소켓을 읽고 쓰고, 작업을 멈추는것에 최적화되어 있다.
		- Dispatchers.Main : 안드로이드의 경우 UI 스레드를 사용한다.

- Launch, Async
	- launch와 async는 CoroutineScope의 확장함수이며, 넘겨 받은 코드 블록으로 코루틴을 만들고 실행해주는 코루틴 빌더이다.
	- launch는 Job객체를, async는 Deferred 객체를 반환하며, 이 객체를 사용해서 수행 결과를 받거나, 작업이 끝나기를 대기하거나, 취소하는 등의 제어가 가능하다.
