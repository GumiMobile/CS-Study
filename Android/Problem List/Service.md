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
 
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcsvOQo%2FbtqEmwgjKcm%2FGZLLDKs46ed0aVF09wGFv1%2Fimg.png", width="300px"/>

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
