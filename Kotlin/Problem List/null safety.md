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

## 김현수

### Nullable과 Non-Nullable 프로퍼티

자바의 경우 primitive type을 제외한 객체들은 항상 null이 될 수 있다. 코틀린은 자바와 다르게 Nullable과 Non-nullable 타입으로 프로퍼티를 선언할 수 있다. Non-nullable 타입으로 선언하면 객체가 null이 아닌 것을 보장하기 때문에 null check 등의 코드를 작성할 필요가 없다.

타입을 선언할 때 ?를 붙이면 null을 할당할 수 있는 프로퍼티이고, ?가 붙지 않으면 null이 허용되지 않는 프로퍼티를 의미한다.

```kotlin
var nullable: String? = "nullable"
var nonNullable: String = "non-Nullable"
```

nullable 프로퍼티는 null을 할당할 수 있지만, nonNullable에 null을 할당하려고 하면 컴파일 에러가 발생한다.

```kotlin
nullable = null      // 컴파일 성공
nonNullable = null   // 컴파일 에러
```

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

### 안전하게 nullable 프로퍼티 접근하기

1. 조건문으로 nullable 접근

가장 쉬운 방법은 `if-else`를 이용하는 것. 단점은 if-else 루프가 반복되는 경우 가독성을 해칠 수 있다.
	
2. Safe call `?.` 연산자로 nullable 접근

Safe call은 객체를 접근할 때 ?.로 접근하는 방법을 말한다. 아래 코드에서 a?.length를 수행할 때 a가 null이라면 length를 호출하지 않고 null을 리턴하므로 NPE가 발생하지 않는다.

```kotlin
val a: String? = null
println(a?.length)
```

### 안전하게 nullable 프로퍼티 할당하기

어떤 프로퍼티를 다른 프로퍼티에 할당할 때, 객체가 null인 경우 default 값을 할당하고 싶을 수 있다. 이를 위해 자바에서는 삼항연산자를 사용하지만 코틀린은 삼항연산자를 지원하지 않는다. 따라서 삼항연산자를 대체하기 위해 다음의 방법을 사용한다.

1. if-else
자바와는 다르게 코틀린은 한줄로 if-else를 쓸 수 있다. 

```kotlin
val l = if (b != null) b.length else -1
```

2. 엘비스 연산자(Elvis Operation)
엘비스 연산자는 `?:`를 말한다. 삼항연산자와 비슷한데 ?: 왼쪽의 객체가 null이 아니면 이 객체를 리턴하고 null이라면 ?:의 오른쪽 객체를 리턴한다. 다음은 위의 if-else 예제를 엘비스 연산자를 사용하여 구현한 코드이다.

```kotlin
val l = b?.length ?: -1
```

3. Safe Cast
코틀린에서 형변환 할 때 Safe Cast를 이용하면 안전히다. 아래 코드에서 문자열이지만 Any타입인 string을 as?를 이용하여 String과 Int로 형변환을 시도하면, String은 성공하지만 Int는 타입이 맞지 않기 때문에 null을 리턴한다.

```kotlin
val string: Any = "AnyString"
val safeString: String? = string as? String
val safeInt: Int? = string as? Int
println(safeString) // AnyString
println(safeInt)	// null
```

4. Collection의 Null 객체를 모두 제거
Collection에 있는 Null 객체를 미리 제거할 수 있는 함수도 제공한다. 다음은 List에 있는 null 객체를 filterNotNull 메소드를 이용하여 삭제하는 코드이다.

```kotlin
val nullableList: List<Int?> = listOf(1, 2, null, 4)
val intList: List<Int> = nullableList.filterNotNull()
println(intList)	// [1, 2, 4]
```

## 김민수

### Null safety, Kotlin

자바의 경우 모든 레퍼런스 타입에 null 대입이 가능하다. 따라서 런타임 시에 `NulllPointerException` 이 발생할 가능성이 높다. 하지만 코틀린의 경우, nullable 타입인지 ?로 명시해주어야 하며, 이를 통해 null이 할당되는 것을 컴파일 타임에서 미리 방지할 수 있다. 자바의 경우 `@Nonnul` 어노테이션 또는 `java.util.Optional`을 사용하면 null체크를 할 수 있다.

```kotlin
val str: String? = null // 에러 없음
val str: String = null // 에러 발생
```



### Safe call

#### ?. Safe call operator

nullable 타입의 경우 safe call operator ?. 을 이용하여 null을 컨트롤 할 수 있다.

``` kotlin
val a = "kotlin"
val b: String? = null
println(a?.length) // 6, ?.필요없음
println(b?.length) // null

bob?.department?.head?.name // bob, department, head 중 하나라도 null이면 null 반환
```

위 코드에서 b?.length에 접근할 때, ?. 왼쪽의 값이 null이면 null을 반환한다. b?.length의 타입은 Int?가 된다.

#### .let() 표준 확장함수를 이용한 null 무시

``` kotlin
val list: List<String?> = listOf("kotlin", null, "java")
for(item in list) {
    item?.let(println(it)) // null을 무시하고 kotlin java 출력
}
```



### ?: Elvis operator

``` kotlin
val l: Int = if(b != null) b.length else -1
val l = b?.length ?: -1 // 위 코드를 Elvis 연산자를 이용하여 간단하게 표현 가능
```

?: 왼쪽의 값이 null 이라면 ?: 오른쪽 값을 할당한다. 위 코드에서 b가 null이라면 l = -1이 된다. 또한 ?: 이후에 return 문 혹은 throw를 통해 Exception을 던질 수 도 있다.

``` kotlin
fun foo(node: Node): String? {
	val l = b?.length ?: return null
    val name = node.getName() ?: throw IllegalArgumentException("")
}
```



### !! operator

nullable 타입을 non-null로 강제 변환시키는 연산자이다. 이떄 null이 할당되면 NPE가 발생할 수 있어 신중하게 사용해야 한다.

``` kotlin
val b: String? = null
val length = b!!.length // NPE 발생
```



### Safe cast

자바에서 잘못된 class casting을 할 경우, 런타임에서 `ClassCastException`이 발생한다. 코틀린은 캐스팅 시 타입이 맞지 않으면 null을 반환하는 as? 를 지원한다.

``` kotlin
val b = "kotlin"
val aInt: Int? = b as? Int // aInt = null
```

b는 "kotlin"이므로 Int로 캐스팅 할 수 없다. 하지만 as?로 인해 `ClassCastException` 발생을 회피하고 aInt에 null을 할당한다.



### Collections 에서 null filter

nullable 요소를 갖는 Collection에서 `filterNotNull()`을 이용해 null 요소를 필터링 할 수 있다.

``` kotlin
val nullableList: List<Int?> = listOf(1, 2, null, 4)
println(nullableList.filterNotNull()) // [1, 2, 4]
```



## 우지현

### Nullable types and Non-Null Types

코틀린의 타입 시스템은 `Bilion Dollar Mistake`라고도 알려진 null 참조 코드의 위험성을 없애기 위한 것이다. 

Java를 포함한 많은 프로그래밍 언어에서 가장 일반적인 함정 중 하나는 null 참조 멤버에 접근하면 null 참조 예외(null reference exception)가 발생한다는 것이다. Java에서는 이것을 `NullPointerException` 또는 `NPE`라고 한다.

코틀린 타입 시스템은 코드에서 `NullPointerException`을 제거하기 위한 것이다. 코틀린 타입은 기본적으로 Non-Null 타입이고 Nullable type은 타입 뒤에 ?가 붙는다.

#### 코틀린에서 `NPE`가 발생하는 원인

- `throw NullPointerException`을 명시적으로 호출
- `!!` 연산자 사용
- 초기화와 관련하여 아래와 같은 특성으로 인한 데이터 불일치가 발생할 때
  - 생성자에서 `this`를 초기화하지 않고도 사용할 수 있으며, 다른 곳으로 전달되어 사용할 수 있다. (`leaking this`)
  - 수퍼 클래스 생성자는 파생 클래스의 구현에서 초기화되지 않은 상태를 사용하는 open member 호출.
- 자바 상호작용
  - 플랫폼 유형의 null 참조에서 멤버에 접근하려고 시도
  - 올바르지 않은 nullability로 자바 상호작용에 사용되는 제네릭 타입
  - 외부의 자바 코드로 인한 기타 문제들

### 조건문으로 null 확인

```kotlin
val l = if (b != null) b.length else -1
```

명시적으로 b가 null인지를 체크하여 null일 때와 null이 아닐 때를 구분해서 다룬다.

### Safe Calls `?.`

```kotlin
val a = "Kotlin"
val b: String? = null
println(b?.length)
println(a?.length) // Unnecessary safe call
```

b가 null이 아니면 b.length를 리턴하고, 그렇지 않으면 null을 반환한다.

safe call은 chain에서 유용하다. 아래와 같은 경우 프로퍼티가 null인 것이 있따면 이 체인은 null을 리턴한다.

```kotlin
bob?.department?.head?.name
```

null이 아닌 값에 대해서만 특정 연산을 수행하려면 아래와 같이 안전한 호출 연산자인 `let`을 사용하면 된다.

```kotlin
val listWithNulls: List<String?> = listOf("Kotlin", null)
for (item in listWithNulls) {
    item?.let { println(it) } // prints Kotlin and ignores null
}
```

### 엘비스 연산자 (Elvis Operator) `?:`

`?:`는 결과값이 null이 아니면 연산자 왼쪽의 값을 반환하고, 값이 null이면 오른쪽의 값을 반환한다. 오른쪽 표현식은 왼쪽의 값이 null인 경우에만 계산된다.

```kotlin
val l = b?.length ?: -1
```

코틀린에서는 `throw`와 `return`이 표현식이므로 엘비스 연산자의 오른쪽에 사용할 수 있다.

```kotlin
fun foo(node: Node): String? {
    val parent = node.getParent() ?: return null
    val name = node.getName() ?: throw IllegalArgumentException("name expected")
    // ...
}
```

### `!!` 연산자

non-null을 선언하는 연산자인 `!!`는 모든 값을 null이 아닌 타입으로 변환하고, 값이 null인 경우에는 예외를 throw한다. NPE를 원하는 경우 NPE를 발생시킬 수 있지만 명시적으로 요청해야 한다.

```kotlin
val l = b!!.length
```

### 안전한 캐스팅 (Safe Casts)

객체가 target 타입이 아닌 경우 규칙적인 타입 캐스팅으로 인해 `ClassCastException`이 발생할 수 있다. 이때는 null을 리턴하는 safe cast를 사용하면 된다.

```kotlin
val aInt : Int? = a as? Int
```

### 널이 가능한 타입 컬렉션

null을 입력할 수 있는 타입의 컬렉션이 있을 때, null이 아닌 요소를 필터링하고 싶으면 `filterNotNull`을 사용하면 된다.

```kotlin
val nullableList: List<Int?> = listOf(1, 2, null, 4)
val intList: List<Int> = nullableList.filterNotNull()
```

