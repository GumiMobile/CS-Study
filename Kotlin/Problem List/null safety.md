# Null safety
## 이유진
> NPE : NullPointerException (null 참조의 멤버에 액세스할 때 발생하는 null 참조 예외)

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
### null check
#### 1. 조건문
```kotlin
val l = if (b != null) b.length else -1
```
### 2. Safe Calls `?.`
안전한 호출 연산자인 `?.` 사용  
```kotlin
val a = "Kotlin"
val b: String? = null
println(b?.length)
println(a?.length) // Unnecessary safe call
```
b가 null이 아니면 b.length를 리턴하고, 그렇지 않으면 null을 반환한다.  
이때, `b?.length`의 표현식은 `Int?`가 된다.

체인에서 유용하다.
```kotlin
bob?.department?.head?.name
```

null이 아닐 때만 특정 연산을 수행하려면 `let`을 사용하면 된다.
```kotlin
item?.let { println(it) } //  prints Kotlin and ignores null
```

#### Elvis 연산자 `?:`
```kotlin
// if문 사용
val l: Int = if (b != null) b.length else -1

// elvis 연산자 사용
val l = b?.length ?: -1
```
`?:`의 왼쪽에 있는 식이 null이 아니면 연산자 왼쪽의 값(`b.length`)을 반환하고, null이면 오른쪽 값(`-1`)을 반환한다.

코틀린에선 `throw`와 `return`이 표현식이므로 엘비스 연산자의 오른쪽에서도 사용할 수 있다.
```kotlin
val parent = node.getParent() ?: return null
```

#### `!!` 연산자
NPE를 발생시킬 수 있는 방법!  
`!!`는 **not-null**을 선언하는 연산자다. 즉, 모든 값을 null이 아닌 타입으로 변환하고, null인 경우 예외를 throw한다.

#### Safe Casts
cast시 대상 유형이 아닌 경우 `ClassCastException`이 발생한다.   
`as?`를 이용해 형변환을 시도하면, 실패했을 때 `null`을 리턴한다.
```kotlin
val string: Any = "AnyString"
val safeString: String? = string as? String // String으로 형변환 시도
val safeInt: Int? = string as? Int // Int로 형변환 시도
println(safeString) // AnyString
println(safeInt) // null
```

#### Null이 가능한 Collection
Nullable 컬렉션에서, Null이 아닌 요소만 필터링 하려면 `filterNotNull()`을 사용한다.
```kotlin
val nullableList: List<Int?> = listOf(1, 2, null, 4)
val intList: List<Int> = nullableList.filterNotNull() // {1, 2, 4}
```

