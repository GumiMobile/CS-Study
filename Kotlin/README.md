# Kotlin

* [Coroutine](#coroutine)
* [고차함수](#고차함수)
* [Null safety, Kotlin](#Null-safety-Kotlin)
  * [Nullable과 Non-Nullable 프로퍼티](#Nullable과-Non-Nullable-프로퍼티)
  * [nullable 타입을 non-nullable 타입으로 변경하기](#nullable-타입을-non-nullable-타입으로-변경하기)
  * [코틀린에서 NPE가 발생하는 경우](#코틀린에서-NPE가-발생하는-경우)
  * [안전하게 nullable 프로퍼티 접근하는 방법](#안전하게-nullable-프로퍼티-접근하는-방법)
  * [안전하게 nullable 프로퍼티 할당하기](#안전하게-nullable-프로퍼티-할당하기)


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

[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#kotlin)

</br>

## 고차함수

- 함수를 인자로 받거나 반환하는 함수
- 코틀린에서는 람다나 함수 참조를 사용해 함수를 값으로 표현 가능

함수 타입을 매개변수로 받는 고차함수의 실행은 아래와 같다.

### 함수 타입

코틀린은 타입 추론이 가능해서 변수 타입을 지정하지 않아도 람다를 변수에 대입할 수 있다.

```kotlin
val sum = { x: Int, y: Int -> x + y }
val action = { println(42) }
```

각 변수에 구체적인 타입 선언을 추가하면 이렇게 된다.

```kotlin
val sum: (Int, Int) -> Int = { x: Int, y: Int -> x + y }
val action: () -> Unit = { println(42) }
```

> **Unit타입** : 의미 있는 값을 반환하지 않는 함수 반환 타입에 쓰는 특별한 타입 타입을 지정할 때, 함수 타입 전체가 널이 될 수 있다면 함수 타입을 괄호로 감싸고 물음표를 붙여야 한다.

```kotlin
val canReturnNull: (Int, Int) -> Int? = { x, y -> null }
val funOrNull: ((Int, Int) -> Int)? = null
```

#### 파라미터 이름과 함수 타입

함수 타입에서 파라미터 명을 지정할 수 있다.

강제적인 것은 아니며, 호출하는 쪽에서 원하는 파라미터 명으로 변경하여 사용할 수 있다.

```kotlin
fun performRequest(
        url: String,
        callback: (code: Int, content: String) -> Unit
) {
    /* .. */
}

fun main() {
    val url = "http://www.naver.com"
    performRequest(url) { code, content -> /*..*/ } // api 에서 정의한 파라미터 명을 사용
    performRequest(url) { code, page -> /*..*/ } // 원하는 이름으로 변경해서 사용 가능
}
```

#### 인자로 받은 함수 호출

doTwoThee 함수의 인자로 람다식을 받고 doTwoThree에서 인자 람다식을 호출하여 사용한다.

```kotlin
fun doTwoThree(predicate: (Int, Int) -> Int) {
  val result = predicate(2, 3)
  println(result)
}

fun main() {
  doTwoThree {a, b -> a + b}  // 5  fun predicate(a: Int, b: Int): Int {return a + b}
  doTwoThree {a, b -> a * b}  // 6  fun predicate(a: Int, b: Int): Int {return a * b}
}
```

#### 함수에서 함수를 반환

함수 내에서 함수를 반환하는 경우는 적지만 매우 유용하다.

특정 조건에 따라 달라질 수 있는 로직을 함수를 반환하는 함수로 정의하여 해당 함수를 호출하는 구조를 가짐으로써 깔끔한 코드 스타일로 가져갈 수 있다.

```kotlin
enum class Delivery { STANDARD,  EXPEDITED }

class Order(val itemCount: Int)

fun getShippingCostCaculator(
        delivery: Delivery) : (Order) -> Double  // 함수를 반환하는 함수를 선언한다.
{
    if (delivery == Delivery.EXPEDITED) {
        return { order -> 6 + 2.1 * order.itemCount } // 람다 반환
    }
    return { order -> 61.2 * order.itemCount } // 람다 반환
}

fun main() {
    val calculator = getShippingCostCaculator(Delivery.EXPEDITED) // 반환받은 함수를 변수에 저장한다.
    println("Shipping costs ${calculator(Order(3))}") // 반환받은 함수를 호출한다.
}
```
[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#kotlin)

<br>

## Null safety, Kotlin

> NPE : NullPointerException (null 참조의 멤버에 액세스할 때 발생하는 null 참조 예외)

### Nullable과 Non-Nullable 프로퍼티

자바의 경우 primitive type을 제외한 객체들은 항상 null이 될 수 있다. 따라서 런타임시에 NPE가 발생할 가능성이 높다.  
**코틀린은 자바와 다르게 Nullable과 Non-nullable 타입으로 프로퍼티를 선언할 수 있다.** Non-nullable 타입으로 선언하면 객체가 null이 아닌 것을 보장하기 때문에 null check 등의 코드를 작성할 필요가 없고, null이 할당되는 것을 컴파일 타임에서 미리 방지할 수 있다.  
타입을 선언할 때 ?를 붙이면 null을 할당할 수 있는 프로퍼티이고, ?가 붙지 않으면 null이 허용되지 않는 프로퍼티를 의미한다.

> 자바의 경우 `@Nonnull` 어노테이션 또는 `java.util.Optional`을 사용하면 null체크를 할 수 있다.


```kotlin
val nullable: String? = null // 에러 없음 (nullable)
val nonNullable: String = null // 에러 발생 (non-nullable)
```
nullable 프로퍼티는 null을 할당할 수 있지만, nonNullable에 null을 할당하려고 하면 컴파일 에러가 발생한다.

```kotlin
nullable = null      // 컴파일 성공
nonNullable = null   // 컴파일 에러
```


<br>

### nullable 타입을 non-nullable 타입으로 변경하기
코틀린에서는 자바의 라이브러리를 쓰는 경우 NPE가 발생할 수 있다. 자바는 non-nullable 타입이 없기 때문에 자바 라이브러리를 사용하면 nullable 타입으로 리턴된다.

코틀린에서 자바 라이브러리의 함수의 리턴 값을 non-nullable인 String으로 변환하려 할 때 아래 코드처럼 대입하면 컴파일 에러가 발생한다. String?타입을 String타입에 할당하려고 했기 때문이다.

```kotlin
var nonNullString1: String = getString()     // 컴파일 에러
```

반면 아래 코드는 !! 연산자를 사용했기 때문에 컴파일이 된다. !!연산자는 객체가 null이 아닌 것을 보장하고 만약 null이라면 NPE를 발생시킨다. 따라서 !!연산자는 null이 아닌 것을 보장할 수 있는 객체에만 사용해야 한다.

```kotlin
var nonNullString2: String = getString()!!   // 컴파일 성공
```


<br>

### 코틀린에서 NPE가 발생하는 경우
- `throw NullPointerException()`을 명시적 호출
- `!!` 사용법에 따라
- 초기화에 대해 아래와 같은 특성으로 데이터의 불일치가 발생할 때 :
  - 생성자에서 `this`를 초기화하지 않고 사용할 수 있고, 다른 곳으로 전달되어 사용할 수 있다. (leaking this)
  - superclass 생성자가 파생 클래스의 구현에서 초기화되지 않은 상태를 사용하는 open member을 호출함
- 자바 상호 작용
  - 플랫폼 유형의 `null`참조에서 멤버에 접근하려고 시도함
  - generic type과 관련한 null 허용 여부 문제. 예를 들어,  `MutableList<String>`에 `null`을 추가할 때
  - 외부 java 코드로 인한 기타 문제
- String은 null을 포함할 수 없다.
```kotlin
var a: String = "ab"
a = null // Compilation error
```

#### `!!` 연산자
NPE를 발생시킬 수 있는 방법!  
`!!`는 **not-null**을 선언하는 연산자다. 즉, 모든 값을 null이 아닌 타입으로 변환하고, null인 경우 예외를 throw한다. (null이 할당되면 NPE가 발생할 수 있다.)

``` kotlin
val b: String? = null
val length = b!!.length // NPE 발생
```


<br>

### 안전하게 nullable 프로퍼티 접근하는 방법

#### 1. 조건문
가장 쉬운 방법은 `if-else`를 이용하는 것. 단점은 if-else 루프가 반복되는 경우 가독성을 해칠 수 있다.
```kotlin
val l = if (b != null) b.length else -1
```

#### 2. Safe Calls `?.`

Safe call은 객체를 접근할 때 ?.로 접근하는 방법을 말한다. 아래 코드에서 `b?.length`를 수행할 때 b가 null이라면 length를 호출하지 않고 null을 리턴하므로 NPE가 발생하지 않는다. 이때, `b?.length`의 표현식은 `Int?`가 된다.

```kotlin
val a = "Kotlin"
val b: String? = null
println(a?.length) // Unnecessary safe call. 6출력
println(b?.length) // null
```

체인에서 유용하다.
```kotlin
bob?.department?.head?.name
// bob, department, head 중 하나라도 null이면 null 반환
```

null이 아닐 때만 특정 연산을 수행하려면 `let`을 사용하면 된다.
```kotlin
item?.let { println(it) } //  prints Kotlin and ignores null
```

#### 3. .let() 표준 확장함수를 이용한 null 무시

``` kotlin
val list: List<String?> = listOf("kotlin", null, "java")
for(item in list) {
    item?.let(println(it)) // null을 무시하고 kotlin java 출력
}
```

<br>

### 안전하게 nullable 프로퍼티 할당하기

어떤 프로퍼티를 다른 프로퍼티에 할당할 때, 객체가 null인 경우 default 값을 할당하고 싶을 수 있다. 이를 위해 자바에서는 삼항연산자를 사용하지만 코틀린은 삼항연산자를 지원하지 않는다. 따라서 삼항연산자를 대체하기 위해 다음의 방법을 사용한다.

#### 1. if-else
자바와는 다르게 코틀린은 한줄로 if-else를 쓸 수 있다. 

```kotlin
val l = if (b != null) b.length else -1
```

#### 2. Elvis 연산자 `?:`
`?:`의 왼쪽에 있는 식이 null이 아니면 연산자 왼쪽의 값(`b.length`)을 반환하고, null이면 오른쪽 값(`-1`)을 반환한다. 다음은 같은 식을 if문과 elvis연산자를 사용해 구현한 코드이다.

```kotlin
// if문 사용
val l: Int = if (b != null) b.length else -1

// elvis 연산자 사용
val l = b?.length ?: -1
```


코틀린에선 `throw`와 `return`이 표현식이므로 엘비스 연산자의 오른쪽에서도 사용할 수 있다.
```kotlin
// return 사용 예
val parent = node.getParent() ?: return null

// throw 사용 예
fun foo(node: Node): String? {
	val l = b?.length ?: return null
    val name = node.getName() ?: throw IllegalArgumentException("")
}
```


#### 3. Safe Casts
코틀린에서 형변환 할 때 Safe Cast를 이용하면 안전히다. cast시 대상 유형이 아닌 경우 `ClassCastException`이 발생한다.   
`as?`를 이용해 형변환을 시도하면, 실패했을 때 `null`을 리턴한다.

```kotlin
val string: Any = "AnyString"
val safeString: String? = string as? String // String으로 형변환 시도 => 성공
val safeInt: Int? = string as? Int // Int로 형변환 시도 => 실패
println(safeString) // AnyString (형변환 성공)
println(safeInt) // null (형변환 실패)
```

#### 4. Collection의 Null 객체를 모두 제거
Nullable Collection에서는, Null 객체를 필터링 할 수 있는 함수를 제공한다. 다음은 List에 있는 null 객체를 `filterNotNull()` 메소드를 이용하여 삭제하는 코드이다.

```kotlin
val nullableList: List<Int?> = listOf(1, 2, null, 4)
val intList: List<Int> = nullableList.filterNotNull()
println(intList)	// [1, 2, 4]
```


[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#kotlin)

<br>



