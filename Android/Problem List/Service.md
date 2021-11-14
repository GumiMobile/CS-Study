# Service

## 김현수

### 서비스란?

**Service는 백그라운드 작업을 위한 애플리케이션 구성 요소이다.**

Activity가 사용자에게 직접 보이는 화면이라면 Service는 뒤에서 수행된다.

ex) 음악 재생, 파일 입출력, 네트워크 트랜잭션 처리 등

### 서비스의 3가지 유형
 
1) 백그라운드

    백그라운드는 이름 그대로 사용자에게 직접 보이지 않는 작업을 수행한다.
    애플리케이션을 꺼도 백그라운드 서비스는 계속 수행할 수 있다.

2) 바인더

    바인딩을 위한 서비스이다.
    여러 구성 요소(예를 들면 Acticity)를 서비스에 바인딩하여 서비스와 상호작용할 수 있다.
    액티비티와 서비스는 따로 운영될 수 있지만 서로 바인딩하여 통신할 수도 있다.
    bindService()를 호출하여 사용한다.
    바인딩이 해제되면 해당 서비스는 소멸된다.

3) 포그라운드

    서비스는 포그라운드로도 실행할 수 있다.
    포그라운드는 사용자에게 직접 보이는 작업을 수행한다.
    사용자가 다른 앱을 사용중이어도 계속해서 실행된다.

### 서비스의 선언

서비스는 activity처럼 Manifest에서 application의 하위 요소로 선언한다.

```xml
<manifest>
   <application>
      <service android:name=".MyService"/>
   </application>
</manifest>
```

### 서비스 생명 주기 (Service LifeCycle)
 
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcsvOQo%2FbtqEmwgjKcm%2FGZLLDKs46ed0aVF09wGFv1%2Fimg.png" width="300px" />

#### onStartCommand
- 액티비티 같은 앱 컴포넌트가 startService()를 호출하면, 서비스는 실행되며(started), 그 서비스를 실행한 컴포넌트가 종료되어도 할 일을 모두 마칠 때까지 서비스는 종료되지 않는다. 
- 따라서 서비스가 할일을 다하여 종료시키고 싶다면, 서비스 내에서 stopSelf()를 호출하여 스스로 종료되도록 하거나, 다른 컴포넌트에서 stopService()를 호출하여 종료해야 한다.
- 액티비티 등의 앱 컴포넌트는 startService()를 호출함으로써 서비스를 실행할 수 있고, 이때 어떤 서비스를 실행할지에 대한 정보와 그 외에 서비스에 전달해야할 데이터를 담고 있는 인텐트를 인자로 넘길 수 있다. 그리고 서비스의 onStartCommand() 콜백 메소드에서 매개변수로 그 인텐트를 받는다.

#### onBind
- startService() 메소드 대신 bindService() 메소드를 통해 시작되는 서비스를 서비스 바인딩 (Service Bind 혹은 Bound Service) 라 한다.
- 마치 클라이언트-서버 와 같이 동작을 하는데 서비스가 서버 역할을 한다. 
	(액티비티는 서비스에게 어떠한 요청을 할 수 있고, 서비스로부터 요청에 대한 결과를 받을수 있음)
- 하나의 서비스에 다수의 액티비티 연결 가능하며, 서비스 바인딩은 연결된 액티비티가 사라지면 서비스도 소멸된다. (즉 백그라운드에서 무한히 실행되지는 않음)
- 클라이언트가 서비스와의 접속을 마치려면 unbindService() 를 호출한다.

# Service

## 김민수

### 서비스?

안드로이드 앱 구성요소 중 하나로 <u>백그라운드에서 작업</u>을 수행한다. 액티비티와는 달리 사용자 인터페이스를 제공하지 않는다. 백그라운드에서 실행하기 때문에, 액티비티와는 달리 다른 앱으로 전환하여도 그 작업을 유지하며 수행할 수 있다. 하지만 API 26+부터 <u>백그라운드 서비스의 사용이 제한</u>된다.

### 유형

- 포그라운드

  포그라운드 서비스는 상단 `notification`을 통해 사용자에게 서비스가 실행되고 있다는 것을 알린다. 예를 들어 음악 재생 앱을 실행하면 상단 상태표시줄에 음악 재생 아이콘과 알림 창에 재생 UI를 띄운다.

- 백그라운드

  백그라운드 서비스는 사용자에게 보이지 않는 작업을 수행한다.

- 바인드

  앱 구성요소가 `bindService()`를 호출하여 해당 서비스에 바인딩 할 수 있다. 이때 바인딩 된 서비스와 앱 구성요소는 서버-클라이언트 인터페이스를 통해 상호작용을 한다. 서비스는 자신의 앱 뿐만 아니라 다른 앱의 구성요소와도 바인딩 될 수 있다. 다만, 모든 바인딩이 해제되면 서비스는 소멸한다.

### 수명주기

서비스는 `started service`와 `bound service`로 유형을 나눌 수 있는데, 이에 따라 서비스의 수명 주기 콜백이 달라진다.

<img src="https://developer.android.com/images/service_lifecycle.png" width=600/>

서비스의 전체 수명은 `onCreate()`부터 `onDestroy()`까지이며 `onDestroy()`에서 사용한 리소스를 릴리즈하면 된다.

### 서비스 선언 및 실행

서비스는 액티비티와 마찬가지로 매니페스트에 선언해야 한다. 또한 보안 상, 명시적 인텐트를 사용하여 실행해야 하며 인텐트 필터는 선언하지 말아야한다. `android:exported`속성을 `false`로 설정하면 다른 앱에서 서비스를 사용하지 못한다.

- Started Service

  실행: 명시적 인텐트를 인자로 넣어 `Context#startService(Intent)`를 호출하면, 서비스는 `onStartCommand()` 메서드가 호출된다. 포그라운드 서비스 실행은 `Context#startForegroundService(Intent)`를 호출한다.

  종료: `Service#stopSelf()` 또는 `Context#stopService(Intent)`

- Bound Service

  실행: `Context#bindService(Intent, ServiceConnection, int)`를 호출하여 서비스를 바인드 한다. 이때 서비스에서는 `onBind()`메서드가 호출된다.

  종료: `bindService()`에서 사용한 ServiceConnection 구현체를 변수에 넣어 `Context#unbindServie(ServiceConnection)`를 호출한다.
  서비스에 연결된 모든 커넥션이 사라지면 `onUnbind()`가 호출되며, 시스템은 해당 서비스를 종료시킨다.

### 서비스와 스레드

서비스는 호스트의 메인 스레드에서 동작한다. 따라서 서비스 또한 ANR을 일으킬 수 있으며, 긴 작업이 필요하면 별도의 워커 스레드를 생성해 처리해야 한다. 서비스는 단지 시스템의 백그라운드에서 작업을 처리할 수 있는 컴포넌트일 뿐이다.
