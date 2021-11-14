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

## 우지현

### 코루틴 (Coroutine)

- 하나의 스레드 내에서 여러 개의 코루틴이 실행되는 개념

- 비동기 프로그래밍에 권장되는 동시 실행 설계 패턴

- 단일 스레드 내에서 여러 개의 코루틴을 실행할 수 있기 때문에 많은 양의 동시 작업을 처리할 수 있고 메모리가 절약된다.

  - 기존 스레드는 Context-switching이 발생하기 때문에 많은 양의 스레드를 갖기 어렵다.

  - 하지만 코루틴은 스레드가 아닌 루틴을 일시 중단(suspend)하는 방식이기 때문에 context-switching 비용이 들지 않는다.

    > context-switching : CPU가 스레드를 점유하면서 실행, 종료를 반복하며 메모리 소모

- 지정된 작업 범위 내에서 실행되기 때문에 메모리 누수를 방지할 수 있다.

### 코루틴 스코프 (Coroutine Scope)

코루틴이 실행되는 범위로, 코루틴을 실행하고 싶은 LifeCycle에 따라 원하는 Scope를 생성하여 코루틴이 실행될 작업 범위를 지정할 수 있다.

- CoroutineScope
  - 사용자 정의 Scope
- GlobalScope
  - 앱이 실행될 때부터 종료될 때까지 실행된다.
- viewModelScope
  - ViewModel 대상
  - ViewModel이 제거되면 코루틴 작업이 자동으로 취소된다.
- lifecycleScope 
  - Lifecycle 객체 대상 (Activity, Fragment, Service, ... )
  - Lifecycle이 끝날 때 코루틴 작업이 자동으로 취소된다.
- liveData
  - LiveData 대상
  - LiveData가 활성화되면 실행을 시작하고 비활성화되면 자동으로 취소된다.

### 코루틴 컨텍스트 (Coroutine Context)

코루틴 작업을 어떤 스레드에서 실행할 것인지에 대한 동작을 정의하고 제어하는 요소.

- Job

  - 코루틴을 고유하게 식별하고 코루틴을 제어한다.

- Dispatchers

  - 코루틴을 어떤 스레드에서 실행할 것인지에 대한 동작을 지정한다.

  |       Thread        | 설명                                                         |
  | :-----------------: | ------------------------------------------------------------ |
  |  Dispatchers.Main   | 안드로이드의 메인 스레드로, UI 작업을 위해 사용해야 한다.<br />예를 들어, UI를 구성하거나 LiveData를 업데이트할 때 사용된다. |
  |   Dispatchers.IO    | 네트워크, 디스크 I/O 실행에 최적화되어 있다.<br />예를 들어, Retrofit으로 네트워크 통신을 하거나, File이나 Room 데이터베이스에서 데이터를 읽고 쓸 때 사용된다. |
  | Dispatchers.Default | CPU 사용량이 많은 무거운 작업 처리에 최적화되어 있다.<br />예를 들어, 데이터를 가공하거나 복잡한 연산, JSON 파싱을 할 때 주로 사용된다. |

### 코루틴 빌더 (Coroutine Builder)

위에서 설정한 CoroutineScope와 CoroutineContext를 통해 코루틴을 실행시켜주는 함수.

- launch
  - Job 객체이며, 결과값을 반환하지 않는다.
  - 실행 후 결과값이 필요없는 모든 작업은 launch를 사용하여 실행할 수 있다.
- async
  - Deferred 객체이며, 결과값을 반환한다.
  - await() 함수를 사용하여 코루틴 작업의 최종 결과값을 반환한다.
- withContext
  - async와 동일하게 결과값을 반환하며, async와의 차이점은 await()를 호출할 필요가 없다는 것이다.
  - async{ }.await()와 동일하다고 보면 된다.
  - 코루틴 내부나 suspend 함수 안에서 구현이 가능하며, 콜백 없이 코드의 스레드 풀을 제어할 수 있기 때문에 네트워크 요청이나 DB 조회 같은 작업에 주로 사용한다.

### suspend function

- 코루틴 안에서만 실행할 수 있는 코루틴 전용 메소드.
- suspend 메소드는 일반적인 곳에서 호출할 수 없으며, 반드시 코루틴 안에서만 호출이 가능하다.
  - 코루틴의 실행이 일시중단(suspend)되거나 다시 재개(resume)될 수 있기 떄문에, 컴파일러에게 이 메소드는 코루틴 안에서 실행할 메소드임을 정의하기 위해 메소드명 앞에 `suspend`를 붙여줘야 한다.

### 코루틴을 사용할 때 순서!!

Context로 Scope를 만들고, Builder를 이용하여 코루틴을 실행한다!

1. 어떤 스레드에서 실행할 것인지 Dispatchers를 정하고 
   (Dispatchers.Main, Dispatchers.IO, Dispatchers.Default)
2. 코루틴이 실행될 Scope를 정하고
   (CoroutineScope, viewModelScope, livecycleScope, ...)
3. launch 또는 async로 코루틴을 실행시킨다.


## 이유진
### Coroutine
- 비동기적으로 실행되는 코드를 간소화하기 위해 Android에서 사용할 수 있는 동시 실행 설계 패턴
- 경량화된 쓰레드같지만 실제 사용은 쓰레드와 매우 다르다. (특징 참고)
- 안드로이드의 비동기 프로그래밍에 권장되는 솔루션
  - 여러 언어에서 지원하고 있는 개념
- 안드로이드에서 기본 스레드를 차단하여 앱이 응답하지 않게 만들 수 있는 장기 실행 작업을 관리하는 데 도움이 됨

#### 특징
1. 협력형 멀티 태스킹
  - `Co` + `Routine` => 협력하는 함수
  - 루틴 : 컴퓨터 프로그램에서 하나의 정리된 일
  - 서브루틴(SubRoutine)은  루틴에 진입하는 지점과 빠져나오는 지점(return)이 명확하다.<br>반면 코루틴은 언제든 진입, 탈출할 수 있다.
  - 코루틴 함수가 실행되는 과정에서 `suspend`키워드를 가진 함수를 만나면, 더 이상 아래 코드를 실행하지 않고 멈추고(suspend) 코루틴 block을 (잠시) 탈출한다. 그리고 할 일을 다 하면 다시 코루틴 block으로 돌아와서 다음 코드를 실행한다.
2. 동시성 프로그래밍 지원
  - 쓰레드는 병렬성 프로그래밍이기 때문에 context switching이 발생한다. 
  - 코루티간의 작업 교환은 비용이 적다.
3. 비동기 처리를 쉽게 도와줌

	
## 이수형

### Coroutine
- 진입접을 여러개 허용하는 subroutine
- 코루틴은 비동기적으로 실행되는 코드를 간소화하기 위해 Android에서 사용할 수 있는 동시 실행 설계 패턴
- Android에서 코루틴은 기본 스레드를 차단하여 앱이 응답하지 않게 만들 수도 있는 장기 실행 작업을 관리하는 데 도움
- CoroutineScope와 GlobalScope가 있음
- Context를 사용해 Scope를 구성하고 Builder로 실행

### Context
- Dispacher
   - Main : 메인쓰레드, UI작업을 위해 사용
   - IO : 네트워크, 디스크I/O작업을위해 사용
   - Default : CPU를 많이 사용하는 작업에 사용
- Job
   - 고유한 코루틴을 제어
- context는 + 연산자를 이용해 조합가능
```Kotlin
val someContext = Dispatcher.IO + aJob + bCoroutine + cExceptionHandelr
```
	
