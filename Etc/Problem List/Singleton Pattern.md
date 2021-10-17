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

   

# 제목 없음

## 이수형

### Singleton Pattern

- 싱글톤 패턴은 인스턴스가 오직 한개만 생성되야하는 경우 사용
    - 레지스트리 같은 설정 파일의 경우 객체가 여러개 생성되면 설정 값이 바뀔 위험이 생길수 있기 때문
- 여러 스레드가 동시에 하나의 인스턴스를 공유하여 사용하게 하여 요청이 많은 곳에서 사용하여 효율을 높힐수 있음
    - 두번째 사용부터 객체 로딩시간이 줄어듦

단점

- 너무많은 일을하거나 너무 많은 데이터를 공유시키면 "개방-폐쇠 원칙" 위배
    - 수정이 어려워지고, 유지보수 비용이 높아짐
- 멀티 쓰레드 환경에서 동기화 문제가 생길수 있음 ( 인스턴스가 2개 생성 될 수 있음)
- private 생성자를 가지고 있어서 상속이 불가능
    - 객체지향적이지 못한 static 필드와 메소드를 사용해야함

### 구현 방법

싱글톤 패턴을 구현하기 위해서는 3가지를 지키면 된다

> private 생성자
> 
> 
> static 변수로 객체 생성
> 
> 객체의 getter 구현
> 

1. 기본 Lazy Initializtion
    
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
    
2. Thread safe Lazy initialization
    
    1은 멀티쓰레드 환경에서 동기화 문제가 생길수 있는 코드임으로 다음과같은 방법으로 thread-safe하게 만들 수 있다.
    
    ```java
    public class ThreadSafeLazyInitialization {
    	private static ThreadSafeLazyInitialization instance;
        
        private ThreadSafeLazyInitialization() {}
        
        public static synchronized ThreadSafeLazyInitialization getInstance() {
        	if(instance == null)
            	instance = new ThreadSafeLazyInitialization();
            return instance;
        }
    }
    ```
    
3. Thread safe lazy initialization + Double-checked locking
    
    2와 같은 방법은 synchronized 특성상 비교적 큰 성능저하가 발생하므로 다음과 같은 방법을 사용할 수 있음
    
    ```java
    public class ThreadSafeLazyInitialization {
    	private volatile static ThreadSafeLazyInitialization instance;
        
        private ThreadSafeLazyInitialization() {}
        
        public static ThreadSafeLazyInitialization getInstance() {
        	if(instance == null) {
            	synchronized (ThreadSafeLazyInitialization.class) {
                	if(instance == null)
                    	instance = new ThreadSafeLazyInitialization();
                }
            }
            return instance;
        }
    }
    ```
    
4. Initialization on demand holder idiom
    
    3과 같은 방법은 성능저하를 완화 했으나, Thread A가 인스턴스의 생성을 완료하기 전에 메모리 공간에 할당이 가능하기 때문에 Thread B가 할당된 것을 보고 instance를 사용하려고 하나 생성과정이 모두 끝난 상태가 아니기 때문에 오작동을 할 수 있다.
    
    그래서 JVM의 클래스 초기화 과정에서 보장되는 특성을 이용해 싱글턴 초기화 문제의 책임을 JVM에게 떠넘기는 방법을 사용할 수 있다.
    
    ```java
    public class Something {
    	private SomeThing() {}
        
        private static class LazyHolder {
        	public static final Something INSTANCE = new Something();
        }
        
        public static Something getInstance() {
        	return LazyHolder.INSTANCE;
        }
    }
    ```
    
## 김민수

### Singleton Pattern?

객체의 인스턴스가 오직 1개만 생성되는 패턴이다. 프로그램 전역에서 인스턴스가 하나만 존재한다.

### 사용 이유

- 고정된 메모리 영역을 할당받고 인스턴스를 하나만 사용하기 때문에 메모리 낭비를 막을 수 있다.
- static 인스턴스이기 때문에 다른 클래스의 인스턴스들이 접근하여 사용할 수 있다.

### 문제점

- 멀티 스레딩 환경에서 발생할 수 있는 동시성 문제를 해결해야 한다.
- 싱글톤 인스턴스는 자원을 공유하고 있기 때문에 테스트가 어렵다. 매번 초기화 시켜야 한다.
- 자식 클래스를 만들수 없다.
- 싱글톤 인스턴스가 너무 많은 일을 하거나 많은 데이터를 공유할 경우 **개방 폐쇄 원칙**을 위배하게 된다.

### 구현방법

#### 이른 초기화

``` java
public class Singleton {
    // Eager Initialization
    private static Singleton uniqueInstance = new Singleton();

    private Singleton() {}

    public static Singleton getInstance() {
      return uniqueInstance; 
    } 
}
```

이른 초기화 방식은, static 키워드의 특징을 이용해 클래스 로더가 초기화하는 시점에서 정적바인딩을 통해 해당 인스턴스를 메모리에 등록한다.

#### Lazy Initialization with synchronized

``` java
public class Singleton {
    private static Singleton uniqueInstance;

    private Singleton() {}

    // Lazy Initailization
    public static synchronzied Singleton getInstance() {
      if(uniqueInstance == null) {
         uniqueInstance = new Singleton();
      }
      return uniqueInstance;
    }
}
```

synchronized 키워드를 이용해 초기화 메서드를 Thread-safe를 보장한다. 그러나 해당 초기화 메서드는 인스턴스가 생성되었든, 안되었든 synchronized 블럭이 실행된다. 따라서 synchronized 키워드에 의한 성능저하를 피할 수 없다.

#### Lazy Initialization. Double Checking Locking

``` java
public class Singleton {
    private volatile static Singleton uniqueInstance;

    private Sigleton() {}

    // Lazy Initialization. DCL
    public Singleton getInstance() {
      if(uniqueInstance == null) {
         synchronized(Singleton.class) {
            if(uniqueInstance == null) {
               uniqueInstance = new Singleton(); 
            }
         }
      }
      return uniqueInstance;
    }
}
```



위 동기화 블럭 방식을 개선한 DCL(Doubl Checking Locking) 방식으로, 인스턴스가 생성되지 않은 경우에만 동기화 블럭을 실행하게끔 구현한다.

> volatile
>
> volatile 변수를 사용하지 않는 멀티 스레드 어플리케이션에서는 작업(Task)를 수행하는 동안 변수 값을 cpu cache에 저장한다. 만약 멀티 스레드 환경에서 스레드가 변수 값을 읽어올 때 각각의 cpu cache에 저장된 값이 다르기 때문에 변수 값 **불일치** 문제가 발생한다. 이때, volatile 키워드가 문제를 해결할 수 있다.

#### Lazy Initialization. Lazy Holder

``` java
public class Singleton {

    private Singleton() {}

    /**
     * static member class
     * 내부클래스에서 static변수를 선언해야하는 경우 static 내부 클래스를 선언해야만 한다.
     * static 멤버, 특히 static 메서드에서 사용될 목적으로 선언
     */
    private static class InnerInstanceClazz() {
        // 클래스 로딩 시점에서 생성
        private static final Singleton uniqueInstance = new Singleton();
    }

    public static Singleton getInstance() {
        return InnerInstanceClazz.instance;
    }
    
}
```

Lazy Holder 방식은 가장 많이 사용되는 싱글턴 구현방식이다. volatile, synchronized 키워드 없이 동시성 문제를 해결하기 때문에 성능이 뛰어나다. 싱글턴 클래스에는 InnerInstanceClazz의 변수가 없기 때문에 getInstance의 메서드가 호출될 때, 초기화가 일어난다. 즉 동적 바인딩의 특징을 이용하여 Thread safe하면서 성능이 뛰어나다.
