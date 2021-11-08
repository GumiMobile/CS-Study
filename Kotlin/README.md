# Kotlin

* [Coroutine](#coroutine)

[뒤로](https://github.com/GumiMobile/CS-Study)

</br>

## Coroutine

### Coroutine (코루틴)

비동기적으로 실행되는 코드를 간소화하기 위한 동시 설계 패턴. 코루틴의 코는 'co(함께, 동시에)'라는 의미를 가지고 있다. 즉, 동시성 프로그래밍 개념을 코틀린에 도입한 것이 코루틴이다. 안드로이드의 비동기 프로그래밍에 권장되는 솔루션이지만 여러 언어에서 지원하고 있는 개념이다. 

> 루틴 (routine) : 컴퓨터 프로그램에서 하나의 정리된 일

<br />

#### 특징

- light-weighted thread 같지만 실제 사용은 쓰레드와 매우 다르다.

- 기존의 복잡한 AsyncTask 또는 다수 쓰레드 관리를 직접 해주지 않아도 되며, 기존 다중 쓰레드보다 훨씬 효율적으로 동작한다.

- 단일 스레드 내에서 여러 개의 코루틴을 실행할 수 있기 때문에 많은 양의 동시 작업을 처리할 수 있고 메모리가 절약된다.

  - 기존 스레드는 Context-switching이 발생하기 때문에 많은 양의 스레드를 갖기 어렵다.

  - 하지만 코루틴은 스레드가 아닌 루틴을 일시 중단(suspend)하는 방식이기 때문에 context-switching 비용이 들지 않는다.

    > context-switching : CPU가 스레드를 점유하면서 실행, 종료를 반복하며 메모리 소모

- 안드로이드에서 기본 스레드를 차단하여 앱이 응답하지 않게 만들 수 있는 장기 실행 작업을 관리하는 데 도움이 된다.

- 메인, 서브 루틴에 관계없이 동등하게 루틴을 실행한다.

  - 서브 루틴은 루틴에 진입하는 지점과 빠져나오는 지점이 명확하지만 코루틴은 언제든 진입, 탈출할 수 있다.

- 부모 코루틴이 취소되면 자식 코루틴도 취소된다.

- 지정된 작업 범위 내에서 실행되기 때문에 메모리 누수를 방지할 수 있다.

```kotlin
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

<br />

### Coroutine Scope

코루틴이 실행되는 범위로, 코루틴을 실행하고 싶은 LifeCycle에 따라 원하는 Scope를 생성하여 코루틴이 실행될 작업 범위를 지정할 수 있다.

- CoroutineScope

  - 기본 스코프
  - 코루틴 블록을 묶음으로 제어할 수 있는 단위

  ```kotlin
  // 메인 쓰레드에서 실행될 사용자 정의 Scope
  val scope = CoroutineScope(Dispatchers.Main)
  scope.launch {
      // 메인 쓰레드 작업
  }
  
  // 백그라운드에서 실행될 사용자 정의 Scope
  CoroutineScope(Dispatchers.IO).launch {
      // 백그라운드 작업
  }
  ```

- GlobalScope

  - 싱글톤으로 만들어진다.
  - 앱이 실행될 때부터 종료될 때까지 실행된다.
  - 시스템 전체에 악영향을 끼칠 수 있다.
  - cancel, Exception 처리가 어렵다.

  ```kotlin
  // 앱의 라이프사이클동안 실행될 Scope
  GlobalScope.launch {
      // 백그라운드로 전환하여 작업
      launch(Dispatchers.IO) {
  
      }
  
      // 메인쓰레드로 전환하여 작업
      launch(Dispatchers.Main) {
  
      }
  }
  ```

- viewModelScope

  - ViewModel 대상
  - ViewModel이 제거되면 코루틴 작업이 자동으로 취소된다.

  ```kotlin
  class MyViewModel: ViewModel() {
      init {
          viewModelScope.launch {
              // ViewModel이 제거되면 코루틴도 자동으로 취소됩니다.
          }
      }
  }
  ```

- lifecycleScope

  - Lifecycle 객체 대상 (Activity, Fragment, Service, ... )
  - Lifecycle이 끝날 때 코루틴 작업이 자동으로 취소된다.

  ```kotlin
  class MyActivity : AppCompatActivity() {
      init {
          lifecycleScope.launch {
              // Lifecycle이 끝날 때 코루틴 작업이 자동으로 취소됩니다.
          }
     }
  }
  ```

- liveData

  - LiveData 대상
  - LiveData가 활성화되면 실행을 시작하고 비활성화되면 자동으로 취소된다.

  ```kotlin
  val user: LiveData<User> = liveData {
      val data = repository.loadUser() // suspend function
      emit(data)
  }
  ```

<br />

### Coroutine Context

코루틴 작업을 어떤 스레드에서 실행할 것인지에 대한 동작을 정의하고 제어하는 요소.

- Job

  - 코루틴을 고유하게 식별하고 코루틴을 제어한다.

  ```kotlin
  val job = CoroutineScope(Dispatchers.IO).launch {
      // 비동기 작업
  }
  
  job.join()      // 작업이 완료되기까지 대기
  job.cancel()    // 작업 취소
  ```

- Dispatcher

  - 코루틴을 어떤 스레드에서 실행할 것인지에 대한 동작을 지정한다.

  |         Thread         | 설명                                                         |
  | :--------------------: | ------------------------------------------------------------ |
  |    Dispatchers.Main    | 안드로이드의 메인 스레드로, UI, suspend, LiveData 수정사항 수신 등 작고 가벼운 작업을 실행한다. |
  |     Dispatchers.IO     | 백그라운드 스레드에서 작동하며, 네트워크, 디스크 I/O 실행에 최적화되어 있다. 예를 들어, Retrofit으로 네트워크 통신을 하거나, File이나 Room 데이터베이스에서 데이터를 읽고 쓸 때 사용된다. |
  |  Dispatchers.Default   | CPU 사용량이 많은 무거운 작업 처리에 최적화되어 있다. 예를 들어, 데이터를 가공하거나 복잡한 연산, JSON 파싱을 할 때 주로 사용된다. |
  | Dispatchers.Unconfined | GlobalScope에서 사용하며, 현재 스레드에서 작동한다. 안드로이드 개발에는 사용 지양. |

  

- '+' 연산자를 이용한 컨텍스트 결합

  - Dispatchers.IO + CoroutineName("name...")와 같이 디버깅을 위해 name을 컨텍스트에 결합 가능

<br />

### Coroutine Builder

위에서 설정한 Coroutine Scope와 Coroutine Context를 통해 코루틴을 실행시켜주는 함수.

- launch : `Job` 반환
  - Job 객체이며, 결과값을 반환하지 않는다.
  - cancel(), cancelAndJoin(),  join() 등의 함수로 제어할 수 있다.
  - 실행 후 결과값이 필요없는 모든 작업은 launch를 사용하여 실행할 수 있다.
- async : `Deferred` 반환
  - Deferred 객체이며, 결과값을 반환한다.
  - 미래의 계산 결과가 예상되는 비동기 작업에 사용한다.
  - await() 함수를 사용하여 코루틴 작업의 최종 결과값을 반환한다.
- withContext : `T` 반환
  - async와 동일하게 결과값을 반환하며, async와의 차이점은 await()를 호출할 필요가 없다는 것이다.
  - async{ }.await()와 동일하다고 보면 된다.
  - 코루틴 내부나 suspend 함수 안에서 구현이 가능하며, 콜백 없이 코드의 스레드 풀을 제어할 수 있기 때문에 네트워크 요청이나 DB 조회 같은 작업에 주로 사용한다.
- runBlocking : `T` 반환
  - 새로운 코루틴을 시작하고 내부 코루틴이 완료될 때까지 현재 스레드 block
  - 안드로이드 메인 스레드에서 runBlocking 사용시, 5초 이상 작업하면 ANR 발생

<br />

### suspend function

- 코루틴 안에서만 실행할 수 있는 코루틴 전용 메소드.
- `suspend` 메소드는 일반적인 곳에서 호출할 수 없으며, 반드시 코루틴 안에서만 호출이 가능하다.
  - 코루틴의 실행이 일시중단(suspend)되거나 다시 재개(resume)될 수 있기 떄문에, 컴파일러에게 이 메소드는 코루틴 안에서 실행할 메소드임을 정의하기 위해 메소드명 앞에 `suspend`를 붙여줘야 한다.
- 코루틴 함수가 실행되는 과정에서 `suspend`키워드를 가진 함수를 만나면, 더 이상 아래 코드를 실행하지 않고 멈추고(suspend) 코루틴 block을 (잠시) 탈출한다. 그리고 할 일을 다 하면 다시 코루틴 block으로 돌아와서 다음 코드를 실행한다.

<br />

### 코루틴을 사용할 때 순서!!

Context로 Scope를 만들고, Builder를 이용하여 코루틴을 실행한다!

1. 어떤 스레드에서 실행할 것인지 Dispatchers를 정하고 (Dispatchers.Main, Dispatchers.IO, Dispatchers.Default)
2. 코루틴이 실행될 Scope를 정하고 (CoroutineScope, viewModelScope, livecycleScope, ...)
3. launch 또는 async로 코루틴을 실행시킨다.

[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#폴더명)

</br>
