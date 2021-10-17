# Singleton Pattern

## 우지현

### Singleton Pattern (싱글톤 패턴)

- 애플리케이션에서 인스턴스를 하나만 만들어 사용하기 위한 패턴
- 싱글톤 패턴의 목적
  - 커넥션 풀, 스레드 풀, 디바이스 설정 객체 등의 경우, 인스턴스를 여러 개 만들게 되면 자원을 낭비하게 되거나 버그를 발생시킬 수 있다.
  - 따라서 인스턴스를 오직 하나만 생성하고 그 인스턴스를 사용하도록 하는 것이 싱글톤 패턴의 목적이다.
- 장점
  - 싱글톤으로 구현한 인스턴스는 '전역'이므로 다른 클래스의 인스턴스들이 데이터를 공유하는 것이 가능하다.
- 단점
  - 싱글톤 인스턴스가 혼자 너무 많은 일을 하거나 많은 데이터를 공유시키면 다른 클래스들 간의 결합도가 높아지게 되는데, 이때 `개방-폐쇠 원칙`이 위배된다.
  - 결합도가 높아지게 되면, 유지보수가 힘들고 테스트도 원활하게 진행할 수 없는 문제점이 발생한다.
  - 멀티 스레드 환경에서 동기화 처리를 하지 않았을 때, 인스턴스가 2개 생성되는 문제가 발생할 수도 있다.
  - 따라서 반드시 싱글톤이 필요한 상황이 아니면 지양하는 것이 좋다.

#### 구현

1. Lazy Initialization

   하나의 인스턴스만을 유지하기 위해 인스턴스 생성에 특별한 제약을 걸어두어야 한다. new를 생성할 수 없도록 생성자에 private 접근 제어자를 지정하고, 유일한 단일 객체를 반환할 수 있도록 정적 메소드를 지원해야 한다. 또한 유일한 단일 객체를 참조할 정적 참조변수가 필요하다.

   ```java
   public class Singleton {
       private static Singleton singletonObject;
   
       private Singleton() {}
   
       public static Singleton getInstance() {
           if (singletonObject == null) {
               singletonObject = new Singleton();
           }
           return singletonObject;
       }
   }
   ```

   위 코드는 정말 위험하다. 멀티 스레딩 환경에서 싱글턴 패턴을 적용하다보면 문제가 발생할 수 있다. 동시에 접근하다가 하나만 생성되어야 하는 인스턴스가 두 개 생성될 수 있는 것이다. 그렇기 때문에 `getInstance()` 메소드를 동기화시켜야 멀티스레드 환경에서의 문제가 해결된다.

   ```java
   public class Singleton {
       private static Singleton singletonObject;
   
       private Singleton() {}
   
       public static synchronized Singleton getInstance() {
           if (singletonObject == null) {
               singletonObject = new Singleton();
           }
           return singletonObject;
       }
   }
   ```

   `synchronized` 키워드를 사용하게 되면 큰 성능저하를 발생시킨다는 문제점이 존재한다.



2. Lazy Initialization + Double-Checked Locking (DCL)

   DCL(Double Checking Locking)을 써서 `getInstance()`에서 동기화되는 영역을 줄일 수 있다. 초기에 객체를 생성하지 않으면서도 동기화하는 부분을 작게 만들었다. 스레드를 안전하게 만들면서, 처음 생성한 이후에는 synchronized를 실행하지 않기 때문에 성능저하 완화가 가능하다.

   ```java
   public class Singleton {
       private static volatile Singleton singletonObject;
   
       private Singleton() {}
   
       public static Singleton getInstance() {
           if (singletonObject == null) {
               synchronized (Singleton.class) {
                   if(singletonObject == null) {
                       singletonObject = new Singleton();
                   }
               }
           }
           return singletonObject;
       }
   }
   ```

   > Volatile
   >
   > 멀티스레딩 환경에서 동기화를 해주는 키워드
   >
   > 컴파일러가 특정 변수에 대해 옵티마이저가 캐싱을 적용하지 못하도록 하는 키워드
   >
   > synchronized 키워드는 작업 자체를 원자화해버리지만 volatile은 특정 변수에 대해서 최신 값을 제공한다.

   그러나 이 코드는 멀티코어 환경에서 동작할 때, 하나의 CPU를 제외하고는 다른 CPU가 lock이 걸리게 된다. 그렇기 때문에 다른 방법이 필요하다.



3. Initialization on demand holder idiom

   클래스 안에 클래스(holder)를 두어 JVM의 클래스 로더 매커니즘과 클래스가 로드되는 시점을 이용한다. 

   2번처럼 동기화를 사용하지 않는 방법을 쓰지 않는 이유는 개발자가 직접 동기화 문제에 대한 코드를 작성하면서 회피하려고 하면 프로그램 구조가 그만큼 복잡해지고 비용 문제가 발생할 수 있기 때문이다. 또한 코드 자체가 정확하지 못할 때도 많다.

   그렇기 때문에 JVM의 클래스 초기화 과정에서 보장되는 `원자적 특성`을 이용해서 싱글톤의 초기화 문제에 대한 책임을 JVM에게 떠넘기는 방식을 활용한다.

   클래스 안에 선언한 클래스인 holder에서 선언된 인스턴스는 static이기 때문에 클래스 로딩시점에서 한 번만 호출된다. 또란 final을 사용해서 다시 값이 할당되지 않도록 한다. 실제로 가장 많이 사용되는 일반적인 싱글톤 클래스 사용 방법이다.

   ```java
   public class Singleton {
       private Singleton() {}
       
       private static class SingletonHolder {
           public static final Singleton INSTANCE = new Singleton();
       }
   
       public static Singleton getSingletonObject() {
           return SingletonHolder.INSTANCE;
       }
   }
   ```

   

