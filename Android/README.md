# Android

* [액티비티, 생명주기](#액티비티-생명주기)
* [프래그먼트, 생명주기, FragmentManager](#프래그먼트-생명주기-fragmentmanager)
* [액티비티 vs 프래그먼트](#activity-vs-fragment)
* [Intent](#intent)
* [액티비티, 프래그먼트의 데이터 공유 방법](#액티비티-프래그먼트의-데이터-공유-방법)
* [Looper와 Handler](#looper와-handler)
* [비동기 처리 방법](#비동기-처리-방법)
* [ANR, 발생 시 대처](#ANR)
* [메인 스레드와 워커 스레드](#메인-스레드와-워커-스레드)
* [Bundle, Context](#Bundle)
* [MVC, MVP, MVVM 패턴](#mvc-mvp-mvvm-패턴)

[뒤로](https://github.com/GumiMobile/CS-Study)

</br>

## 액티비티, 생명주기

### 액티비티
- 액티비티는 4개의 앱 구성요소 중 하나로, 사용자와 **상호작용** 하기위한 진입점이다. 
- 사용자에게 UI가 있는 화면을 제공하는 앱 컴포넌트이다.
- App은 반드시 하나 이상의 액티비티를 가져야 하며, 각 액티비티는 독립적이다.
- 모든 Activity는 `Manifest.xml`파일에 application의 하위요소로 선언해야 한다. 그렇지 않으면 시스템에 표시되지 않으며 실행되지 않는다.
- 최초 실행시 보여지는 `Main Activity`를 하나 가지며, `AndroidManifest.xml` 파일에서 해당 액티비티의 `intent-filter`내 action, category 속성을 각각 `android.intent.action.MAIN`, `android.intent.category.LAUNCHER`로 할당하여 설정한다.

### 액티비티 생명주기
#### 필요성
PC에서 모바일로 전환되며 화면의 크기가 작아졌다. 이에 따라, 화면 회전, 진행상태가 저장되지 않는 문제, 다른 앱으로 전환되며 비정상 종료되는 문제, 리소스 낭비 등 여러 이슈가 발생했다. 따라서 화면이 작아지면서 생긴 다양한 화면의 상태(가려지고, 다시 보여지고)를 파악하고, 해당 상황에서 원하는 동작을 수행하기 위해 등장했다. Activity클래스는 Activity의 상태 변화를 알아차릴 수 있는 여러 콜백 메서드를 제공한다.


#### 생명주기

액티비티는 프로세스가 생성되면서부터 사라질 때까지 일련의 상태를 거친다. 상태 전환을 처리하는데 콜백을 사용 할 수 있다.

<img src="https://developer.android.com/guide/components/images/activity_lifecycle.png"/>


- onCreate()
	- 액티비티가 생성될 때 최초로 한 번 실행되는 메소드. (필수적으로 구현해야 한다.)
	- Activity가 생성되면 `Created` 상태가 된다.
	- 인터페이스를 초기화하고, View를 생성한다.
		- setContentView()를 호출하여 레이아웃을 정의한다.
		- View 객체를 생성하여 ViewGroup에 넣어 layout 대신 사용할수도 있다. (ViewBinding)
	- 주로 view를 만들거나 view resource bind , data to list 등을 onCreate()에서 담당한다.
	- `savedInstanceState`매개변수를 수신하는데, 이는 액티비티의 이전 저장 상태가 포함된 Bundle 객체이다. 만약 처음 생성된 경우 null이다.
	- 다음 메소드 : onStart()
  
- onRestart()
   - Activity가 onStop()이 호출된 이후에 재시작 되는 경우 호출되는 메소드
   - OnStop()부터 액티비티 상태를 복원
   - 다음 메소드 : onStart()

- onStart()
	- Activity가 사용자에게 표시되기 직전에 호출된다. (매우 빠르게 완료된다.)
	- 네트워크 API 호출 관련 코드가 작성되는 곳
	- onStarted() 종료 후 상태: Resumed
	- 다음 메소드 : onResume() 또는 onStop()
	
- onResume()
	- 액티비티가 사용자에게 보여지는 단계
	- Activity가 사용자와 상호작용하기 직전에 호출된다.
	- 액티비티가 Stack의 맨위에 있으며 모든 사용자 입력을 캐치함(상호작용)
	- OnPause와 짝을 이루며 포커스를 잃으면 OnPause가 불리고 다시 얻으면 OnResume이 호출됨
	- Activity Running 상태가 된다. 앱에서 포커스가 떠날 때까지 이 상태에 머무른다.
	- Activity Stack 최상단에 위치한다.
	- 다음 메소드 : onPause()

- onPause()
	- 액티비티가 포커스를 잃었을 때, 즉 액티비티의 일부분이 보이거나 투명상태여서 사용자와 상호작용하기 어려운 상태일 때 호출되는 메소드
		- 사용자가 액티비티를 떠나거나, 다른 이벤트로 인해 액티비티가 강제로 전환될 때 (전화)
		- 멀티 윈도우 모드에서 하나의 윈도우만 포커스를 가질 수 있으므로 나머지 액티비티는 pause
		- 반투명한 액티비티가 열렸을 때 - 다이얼로그
	- 사용자가 액티비티를 떠나고 있으며 조만간 OnStop되거나 OnRestart 될것임을 나타낸다.
	- View나 데이터를 SharePreference에 저장하고, 재개될 때 불러올 수 있다.
	- onPause()는 아주 잠깐 실행되기 때문에 사용자 데이터 저장, 네트워크 호출, DB 트랜잭션 등을 실행해서는 안된다. 
	- 다음 메소드 : onResume() 또는 onStop()
		- 액티비티가 포커스를 다시 갖게되면 onResume() 호출
		- 액티비티 화면이 사라지면 상태는 Stopped가 되고 onStop() 호출

- onStop()
	- Activity가 화면에서 100% 가려질 때 호출되는 함수.
		- 홈키를 누르는 경우, 또는 다른 액티비티 페이지 이동이 있는 경우.
	- 아직 Activity가 완전히 소멸된 상태는 아니다.
	- 메모리가 부족할 경우에는 onStop() 메소드가 호출되지 않을 수도 있다.
	- onStop()에서 CPU를 많이 소모하는 종료 작업을 실행해야 한다.
		- Thread, 메모리와 관련된 resource들을 해제
		- db에 데이터 저장 등 
	- 다음 메소드 : onRestart() 또는 onDestroy()
		- OnRestart()가 호출되어 다시 시작할 수 있고 OnDestroy()가 호출되어 프로세스를 종료할 수 있다.

- onDestroy()
	- Activity가 완전히 스택에서 없어질 때, 즉 제거될 때 호출되는 메소드
		- 사용자가 액티비티를 종료하거나, finish() 함수를 호출하여 종료된 경우
		- 기기 회전, 멀티 윈도우 모드 등으로 시스템이 일시적으로 액티비티를 소멸하는 경우
	- Activity가 강제 종료될 때는 호출되지 않는다.
	- onDestroy() 메서드는 이전의 콜백에서 아직 해제되지 않은 모든 액티비티 리소스를 해제해야 한다. (Memory Leak(메모리 누수)의 위험이 있음) 
	- 다음 메소드 : 없음
	

**onStop(), onDestory()는 호출되지 않을 수도 있음**

#### 상태에 따른 프로세스 종료 가능성

| 종료 가능성 | 프로세스 상태                           | 액티비티 상태             |
| ----------- | --------------------------------------- | ------------------------- |
| 최소        | 포그라운드 (포커스 있거나, 가져올 예정) | Created, Started, Resumed |
| 중간        | 백그라운드 (포커스만 상실)              | pause                     |
| 최대        | 백그라운드 (보이지 않음)                | stopped                   |
| 최대        | 소멸                                    | destroyed                 |

#### 액티비티 전환 중 상태변화

한 액티비티(A)가 다른 액티비티(B)를 시작하는 경우 두 개의 액티비티 모두 수명주기가 전환된다. 이때 B 액티비티가 생성될 동안, A 액티비티는 pause 또는 stopped 상태로 전환된다. 두 액티비티가 디스크 혹은 다른 저장소에 저장된 데이터를 공유하는 경우, 주의해야할 점은 두 액티비티의 수명주기 전환이 겹쳐 일어나는 것이다.

1. A 액티비티의 onPause() 실행
2. B 액티비티의 onCreate(), onStart(), onResume() 실행
3. A 액티비티의 onStop() 실행

이와 같은 순서로 각각의 액티비티의 수명주기가 전환되므로 개발 시, 유의하며 컨트롤 할수 있어야한다.

[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#android)

<br />

<br />

## 프래그먼트, 생명주기, FragmentManager
### 프래그먼트

프래그먼트는 `FragmentActivity` 내의 사용자 인터페이스 중 하나이다. 하나의 액티비티에서 여러 개의 프래그먼트를 사용할 수 있으며, 프래그먼트는 여러 액티비티에서 재사용될 수 있다. 프래그먼트는 자신의 수명주기를 갖고 있으며 항상 **액티비티에 종속되어 동작**한다. 따라서 프래그먼트는 부모 액티비티의 수명주기에 영향을 받는다. 프래그먼트는 `FragmentActivity`의 `getSupportFragmentManager()`로 리턴받은 프래그먼트매니저를 통해 관리된다.

- 프래그먼트는 같은 앱에서 서로 다른 크기의 화면을 갖는 기기에서 유연한 UI를 제공하기위해 만들어 졌다.

- 프래그먼트는 다른 액티비티에서 사용될 수 있으므로 프래그먼트가 다른 프래그먼트를 직접 조작하는 것을 지양해야 한다.
- API 28부터는 Jetpack 라이브러리의 프래그먼트를 사용한다.

### 프래그먼트 생명주기

![Fragment Life Cycle](https://developer.android.com/images/guide/fragments/fragment-view-lifecycle.png)

|      콜백 메소드      | 설명                                                         |
| :-------------------: | ------------------------------------------------------------ |
|      onCreate()       | FragmentManager에 add 되었을 때 호출된다.<br />onCreate() 이전에 onAttach()가 먼저 호출된다.<br />아직 FragmentView가 생성되지 않았기 때문에 Fragment의 View와 관련된 작업을 하기에는 적절하지 않다. |
|    onCreateView()     | Fragment의 View를 생성한다.<br />onCreateView()의 반환값으로 정상적인 FragmentView 객체를 제공했을 때만 Fragement View의 LifeCycle이 생성된다.<br /> |
|    onViewCreated()    | View 생성이 완료되었을 때 호출된다.<br />View의 초기값을 설정해주거나 LiveData 옵저빙, RecyclerView 또는 ViewPager2에 사용될 Adapter 세팅 등을 해주는 것이 적절하다. |
| onViewStateRestored() | 저장해둔 모둔 state 값이 Fragment의 View 계층구조에 복원됐을 때 호출된다.<br />체크박스 위젯이 현재 체크되어 있는지 등 각 View의 상태값을 체크할 수 있다. |
|       onStart()       | Fragment가 사용자에게 보여지기 직전에 호출된다.              |
|      onResume()       | Fragment가 사용자와 상호작용할 수 있을 때 호출된다.          |
|       onPause()       | 화면이 일부 가려졌을 때 호출된다.                            |
|       onStop()        | Fragment가 화면에서 사라졌을 때 호출된다.<br />API 28 버전을 기점으로 onSaveInstanceState() 메소드와 onStop() 메소드의 호출 순서가 달라졌다.<br />API 28 버전부터 onStop() 메소드가 onSaveInstanceState() 메소드보다 먼저 호출됨으로써 onStop()이 FragmentTransaction을 안전하게 수행할 수 있는 마지막 지점이 되었다. |
| onSaveInstanceState() | Fragment의 상태를 저장하는 메소드                            |
|    onDestroyView()    | Fragment의 View가 사라질 때 호출된다.<br />가비지 컬렉터에 의해 수거될 수 있도록 Fragment View에 대한 모든 참조가 제거되어야 한다. |
|      onDestroy()      | Fragment가 제거될 때 호출된다.                               |

### 프래그먼트 매니저

액티비티와 프래그먼트, 혹은 프래그먼트와 그 내부 프래그먼트 사이에서 서로 간의 상호작용을 이어주는 역할을 한다. 

- 프래그먼트들을 add, remove, replace 하는 트랜잭션
- 트랜잭션으로 인한 백스택 관리
- 각 하위 프래그먼트의 구성요소에 접근

#### 접근 방법

- FragmentActivity는 getSupportFragmentManager()를 호출하여 프래그먼트 매니저를 받을 수 있다.

- 프래그먼트에서 자신을 호스트한 매니저는 getParentFragmentManager()로 접근 가능하다.
- 프래그먼트에서 자식 프래그먼트를 관리하는 매니저는 getChildFragmentManager()로 접근 가능하다.

[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#android)

<br>

## Activity vs Fragment

### Activity

- 액티비티는 앱을 구성하는 요소로, 앱의 진입점 역할을 하며 사용자와 상호작용할 수 있는 화면을 가진다.
- 액티비티는 가장 바닥에 존재하는 틀 같은 것이다.
- 액티비티 없이는 View도 프래그먼트도 존재할 수 없다.
- 액티비티는 독립적으로 활용할 수 있다.
- 프래그먼트를 사용하려면 FragmentActivity를 상속받아야 한다.
- Manifest에 등록되어 있어야 한다.
- Activity간 통신을 위해 Intent 객체를 활용한다.

### Fragment

- 액티비티와 View의 개념을 합쳐놓은 것으로, 프래그먼트를 사용하면 액티비티를 변경하지 않고도 쉽게 View를 변경할 수 있다.
- 프래그먼트는 화면 안에 들어가는 레이아웃이 중복되지 않도록 한 번만 정의하고 **재사용**이 가능하도록 만든 것이다.
- 프래그먼트는 독립적으로 존재할 수 없고 항상 액티비티 또는 다른 프래그먼트 위에 올라가서 동작한다.
- Manifest에 정의될 필요가 없다.
- 프래그먼트 또한 자신의 생명주기를 가지고 있으나, 호스트 액티비티의 생명주기에 영향을 받는다.
- 프래그먼트는 View가 아니기 때문에 findViewById() 메서드를 즉각 호출하는 것이 불가능하다.
  - getSupportFragmentManager()를 호출해서 프래그먼트 매니저 객체를 불러와서 findViewById()를 사용한다.
- ViewModel 혹은 Fragment Result API를 통해 통신한다.

| 공통점                                                       | 차이점                                                       |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 작업 방식이 비슷하다. (매니저 사용) <br>Activity를 관리하기 위해서 시스템은 액티비티 매니저를 이용한다. Fragment도 프래그먼트 매니저로 관리한다. | Activity는 데이터를 전달하기 위해 Intent객체를 사용한다.<br> Fragment는 Activity와 통신하기 위해 관련 메서드를 이용하지 Intent를 사용하지 않는다. (fragment는 시스템이 관여하지 않기 때문이다.) |
| 생명주기 방식이 비슷하다. <br> Fragment의 생명주기는 Activity의 메서드에 5가지가 추가되었다. | Fragment는 View가 아니기 때문에 findViewById()를 즉각 호출하는 것이 불가능하다. |

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FJYuvN%2FbtqECMbGjnd%2FStnAqpuI0XTmfHonPOttNK%2Fimg.png)

[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#android)

<br />

## Intent

![Intent](https://developer.android.com/images/components/intent-filters_2x.png?hl=ko)

- Android 어플리케이션을 구성하는 네 가지 기본 요소인 Activity, Service, Broadcast Receiver, Content Provider 간에 작업 수행을 위한 정보를 전달한다.
- Intent를 통해 메시지를 전달하고 데이터를 주고받기도 한다. (4대 컴포넌트 간에 작업 수행을 위한 정보전달)
- 명시적 인텐트와 암시적 인텐트로 구분된다.

인텐트는 크게 3가지  경우에서 사용한다. 인텐트에 부가적인 데이터를 key, value로 담아 넘겨 줄 수 있다.

- 액티비티 시작
    - 액티비티의 새로운 인스턴스를 시작하려면 `startActivity()`메서드에 인텐트를 인자로 넘겨줘야 한다.
    - `startActivity(Intent)`, `startActivityForResult(Intent, requestCode)`
- 서비스 시작
    - `startService(Intent)`, `bindService(Intent)`
    
    서비스는 사용자 인터페이스 없이 백그라운드, 포그라운드에서 작업을 수행한다. 일회성 작업을 수행하는 서비스를 시작하는 경우 `startService()`에 인텐트를 전달하면 된다.
    
- 브로드캐스트 전달
    - 브로드케스트 전달: `sendBroadcast(Intent)`, `sendOrderedBroadcast(Intent)`, `sendStickyBroadcast()`
    
    브로드캐스트는 모든 앱이 수신할 수 있는 메시지이다. `sendBroadcase()` `sendOrderdBroadcast()`에 인텐트를 전달하면 다른 앱에 브로드캐스트를 전달할 수 있다.
    

### Intent Types

#### 명시적 인텐트 (Explicit Intent)

- 시작할 컴포넌트 이름을 지정한다. (예: Intent(context, 클래스이름) 또는 Intent.setClass(context, 클래스이름) 등)
- 일반적으로 본인이 만든 컴포넌트를 실행할 때 사용한다.
- 실행하고자하는 컴포넌트와 클래스명이 명시적으로 작성되어 호출 대상을 확실히 알 수 있음

```kotlin
startActivity(Intent(this, NextActivitiy::class.java)
```

- 주로 애플리케이션 내부에서 사용한다.
- 인텐트에 클래스 이름이나 다른 앱의 패키지 이름을 명시한 인텐트를 말한다. 보통 앱 안에서 다른 구성요소를 시작할 때 사용한다.
    
    

#### 암시적 인텐트 (Implicit Intent)

- 인텐트의 액션과 데이터를 지정하긴 했지만, 호출할 대상이 달라질 수 있는 경우에 사용한다.
- 특정 컴포넌트의 클래스명 없이 어떠한 작업을 수행할것인지만 선언한다.
- 해당 인텐트를 처리할 수 있는 컴포넌트를 시스템이 필터링하여 수행하거나 사용자에게 선택하도록 한다.

```kotlin
startActivity(Intent(Intent.ACTION_SEND, uri))
```

- 반드시 특정 컴포넌트를 지정하지 않고, 원하는 작업 의도만 담고 컴포넌트를 실행할 수 있다.
- 암시적 인텐트
    
    특정 구성요소의 이름을 명시하진 않지만, 일반적인 작업을 명시하여 해당 작업을 처리할 수 있는 구성요소를 요청한다. 이때, 시스템은 다른 앱의 Manifest파일에 명시되어있는 intent-filter와 요청한 작업을 비교하여 구성요소를 시작한다. 여러 구성요소가 있을 경우, 사용자에게 선택할 수 있는 UI를 제공한다.
    

### Intent 구성요소

- Action (액션) : 수행할 액션 이름
- Data (데이터) : 수행할 데이터의 URI `e.g) tel: http:`
- Category (카테고리) : 인텐트를 처리해야 하는 구성 요소의 종류에 대한 추가 정보
- Type (타입) : 수행할 인텐트 데이터의 명시적인 타입 (MIME 타입) `(video/mpeg)`
- Component name (컴포넌트 이름) : 대상 컴포넌트의 완전한 클래스 이름
- Extras (추가 정보) : `key-value`로 이뤄진 추가 정보, 인텐트를 통해 데이터를 넘길 수 있다.
- Flag(플래그) : 인텐트에 대한 메타데이터와 같은 기능을 한다. 액티비티를 시작할 방법 혹은 그 후의 처리 방법을 알린다.

### IntentFilter

- 암시적 인텐트를 통해 사용자로 하여금 어느 앱을 사용할지 선택하도록 할 때 IntentFilter가 필요하다.
- 컴포넌트의 특징이 정해진다.
- 인텐트 필터에 `android.intent.action.MAIN`과 `android.intent.category.LAUNCHER`로 선언하면 런처에서 클릭 시 진입점이 되는 Activity가 된다.
- 인텐트 필터는 `AndroidManifest.xml`에 정의한다.
- 인텐트 필터를 구성하는 요소는 인텐트에 작성할 수 있는 요소들과 동일하다.
- 각 인텐트 필터는 어떤 유형의 인텐트를 수락할 지 결정한다. (by `action`, `category`, `data`)

### PendingIntent

- Intent를 가지고 있는 클래스
- 기본 목적은 다른 애플리케이션(다른 프로세스)의 권한을 허가하여 가지고 있는 Intent를 마치 본인 앱의 프로세스에서 실행하는 것처럼 사용하는 것이다.
- Notification은 안드로이드 시스템의 NotificationManager가 Intent를 실행한다.
    - 즉, 다른 프로세스에서 수행하기 때문에 Notification으로 Intent 수행 시 PendingIntent의 사용이 필수이다.
    <br>
    
[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#android)

<br>


## 액티비티, 프래그먼트의 데이터 공유 방법

### Activity - Activity 

1) 이동할 액티비티 클래스를 담고 있는 Intent 객체 생성
2) Intent객체.putXXXExtra(name, value)
3) startActivity(Intent객체)

### Activity - Fragment

** **Bundle을 사용한다.** **

(1) Fragment1에서 Fragment2로 데이터 가지고 이동하기
```kotlin
var fragment2 = Fragment2()
var bundle = Bundle()
bundle.putString("key1", "value1")
bundle.putString("key2", "value2")
fragment2.arguments = bundle //fragment의 arguments에 데이터를 담은 bundle을 넘겨줌

activity?.supportFragmentManager!!.beginTransaction()
                        .replace(R.id.view_main, fragment2)
                        .commit()
```

(2) Fragment2에서 데이터 받기
```kotlin
var bundle:Bundle = getArgument()

if(bundle != null) {
	var str = bundle.getString("key")
}
var str2 = arguments?.getString("key2")

```

### Fragment - Fragment/Activty

프래그먼트는 재사용을 염두해 둔 컴포넌트이다. 따라서 프래그먼트를 독립적으로 유지하려면, 다른 프래그먼트 혹은 액티비티와 직접적으로 통신해선 안된다
하나의 activity아래에 있는 fragment들이 통신한다면, 공유된 ViewModel이나 연결된 Activity를 통해서 통신한다.


ViewModel을 사용해야 하는 경우

- 영속적인 데이터를 공유할 때

Fragment Result Api를 사용하는 경우

- 일회성 데이터를 주고 받을 때

### ViewModel

> 뷰모델은 액티비티, 프래그먼트 등 수명주기를 갖는 컴포넌트의 UI 관련 데이터를 저장하고 관리한다. 기기 회전을 했을 경우, 액티비티, 프래그먼트는 destroy되고 다시 create 된다. 이때 각 컴포넌트가 갖고있는 화면의 데이터는 초기화 된다. Bundle을 이용하여 저장할 수 있지만 번들의 저장용량 권장 사항이 50kb이며 메인 스레드가 번들을 처리하기 때문에 반응성이 떨어진다. 이런 문제점을 해결하기 위해 뷰모델을 사용한다.

뷰모델은 여러 프래그먼트 혹은 액티비티와 데이터를 공유해야 할 때, 가장 적합한 방법이다.
두 프래그먼트는 activity를 통해 ViewModel을 공유할 수 있다. 프래그먼트가 ViewModel내 데이터를 업데이트할 수 있다. 그러면 observe하고 있던 다른 프래그먼트로 새로운 상태가 푸시되고, 푸시를 받은 프래그먼트에서 LiveData로 데이터를 받아와서 작업을 수행할 수 있다.

- 호스트 액티비티와 데이터 공유
- 프래그먼트 간 데이터 공유
- 부모, 자식 프래그먼트 간 데이터 공유
- 뷰모델은 액티비티,프래그먼트,context,View에 대한 참조를 저장하면 안됨

프래그먼트는 기본적으로 액티비티에 의존적인 요소이다. 이때, 뷰모델의 소유자를 액티비티에 지정하고, 호출된 프래그먼트에 `activityViewModels()`라는 대리자를 통해 뷰모델에 접근할 수 있다. 

자식 프래그먼트에서는 `requireParentFragment()`를 이용하여 부모 프래그먼트의 뷰모델에 접근 가능하다.

### Fragment Result API

> 프래그먼트 매니저가 상속받은 인터페이스 `FragmentResultOwner`를 통해 결과 값을 받을 수 있다. 결과를 받고 싶은 액티비티 혹은 프래그먼트의 매니저의 메서드 `setFragmentResultListener`의 인자 리스너를 통해 결과를 받을 수 있다. 자식 프래그먼트의 결과를 받고 싶다면 childFragmentManager를 통해 받을 수 있다.

- 호스트 액티비티에서 결과 수신

- 프래그먼트 간 결과 전달

  <img src="https://developer.android.com/images/guide/fragments/fragment-a-to-b.png" style width="500"/>

  

- 부모, 자식 프래그먼트 간 결과 전달

  <img src="https://developer.android.com/images/guide/fragments/pass-parent-child.png" style width="500"/>
  
  [뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#android)

<br>

## Looper와 Handler

### Looper와 핸들러
안드로이드 시스템은 기본적으로 하나의 Main Thread(=UI Thread)만을 가지고 있고 보통 이 스레드 안에서 작업을 하는데, UI 관련 작업은 반드시 Main Thread에서 처리를 해야하고 네트워크 작업이나 데이터베이스 작업 등 시간이 오래 걸릴 수 있는 작업은 Main Thread가 아닌 별도의 스레드에서 해야한다.

따라서 다운로드나 업로드와 같이 네트워크 작업은 별도의 스레드에서 하고 해당 스레드에서 작업한 결과를 UI에 보여줄때는 Main Thread에 표시를 해야하는 경우가 발생한다. 이러한 멀티 스레드 환경에서 스레드간 통신을 도와주는 도구가 핸들러와 루퍼(=Handler & Looper)이다.

### 핸들러(Handler)

- 안드로이드에서 사용할 수 있는 대표적인 스레드 통신 방법 중 하나가 Hadler를 통해 Message를 전달하는 것이다.
- Handler를 생성하면 호출한 스레드의 Message Queue와 Looper에 자동으로 연결된다.

### 루퍼(Looper)
- 루퍼는 스레드당 하나씩만 가질 수 있다.
- Message Queue가 비어 있는 동안 아무 행동도 하지 않고, 메시지가 들어오면 해당 메시지를 꺼내 적절한 Handler로 전달한다.
- 새로 생성한 스레드는 루퍼를 가지지 않고 Looper.prepare() 메서드를 호출해야 Looper가 생성된다.
- 루퍼는 무한히 실행되는 메시지 루프를 통해 큐에 메시지가 들어오는지 감시하며 들어온 메시지를 처리할 핸들러를 찾아서 handleMessage()를 호출한다.

### 핸들러와 루퍼를 통한 스레드간 통신
<img src="https://miro.medium.com/max/1000/1*cPvR6xzW8oSMhcUCaJNZ4w.png" width="600px">

1. 핸들러에 있는 sendMessage()를 통해서 Message(=작업)를 전달한다.
2. 핸들러는 받은 Message를 Message Queue에 차례대로 넣는다.
3. Looper가 Message Queue로부터 하나씩 Message를 뽑아서 핸들러로 전달한다.
4. Looper로부터 전달받은 메시지는 handleMessage()를 통해서 작업한다.

> `sendMessage(msg: Message): boolean` : 메시지 큐에 message를 만들어서 전달한다.  
> `handleMessage(msg: Message)` : 루퍼를 통해 메시지 큐에서 꺼낸 message나 runnable 처리한다.

#### Message
스레드 통신에서 핸들러에 데이터를 보내기 위한 클래스

[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#android)

<br>

## 비동기 처리 방법

### 안드로이드에서 비동기 처리의 필요성
안드로이드의 어플리케이션은 UI 스레드라고 하는 메인 스레드가 UI를 관리하고 처리한다. 이 메인 스레드는 유저의 앱 사용성에 직결된 요소이기 때문에 항상 빠르게 처리해주어야 한다. 메인 스레드에서 오래 걸리는 작업을 수행할 경우 사용자는 앱이 멈춰있다고 생각할 것이다. 이런 상황이 발생하지 않도록 시간이 오래 걸리는 작업은 비동기로 처리해준다.

### 비동기 처리 방법
#### 1. AsyncTask (Android 11 이후 Deprecated)

<img src="https://t1.daumcdn.net/cfile/tistory/2420B240577D4A720F" width=600/>

- 자체 스레드풀이 있어서 따로 관리하지 않아도 자동으로 스레드를 생성하고 작업을 처리하고 종료된다. 
- Thread, Handler, Message, Runnable 등을 직접 다루지 않아도, 메인 스레드와 별개로 "비동기(Asynchronous) 실행"이 필요한 작업에 사용할 수 있다.
- AsyncTask들의 작업은 모두 단 하나의 스레드에서 처리되므로 비교적 짧은 task처리에 알맞다. 만약 병렬 처리를 원한다면 `myTask.executeOnExcutor(AsyncTask.THREAD_POOL_EXECUTOR)`로 호출하면 된다.
- `doInBackground()`를 제외한 나머지 함수들은 메인 스레드에서 실행되므로, UI 컴포넌트 접근이 가능하다.
	- 하나의 클래스 안에 스레드 동작 부분과 UI 접근 부분을 동시에 정의할 수 있다.
- `AsyncTask.execute()`로 실행하고 `doInBackground(), onProgressUpdate(), onPostExcuted()` 를 직접 호출하면 안된다.

- AsyncTask <Params, Progress, Result>
  - Params : doInBackground 파라미터 타입. execute 메소드의 인자 값
  - Progress : doInBackground 작업 시 진행 단위의 타입. onProgressUpdate 파라미터 타입
  - Result : doInBackground 리턴값. onPostExecute의 파라미터 타입

#### AsyncTask 동작 과정

- execute() 명령어를 통해 AsyncTask를 실행한다.
- AsyncTask로 백그라운드 작업을 실행하기 전에 onPreExecuted()가 실행된다.
  - 이 부분에서는 이미지 로딩 작업이라면 로딩 중 이미지 띄워놓기 등 스레드 작업 이전에 수행할 동작을 구현한다.
- 새로 만든 스레드에서 백그라운드 작업을 수행한다. 
  - execute() 메소드를 호출할 때 사용된 파라미터를 전달받는다.
- doInBackground() : AsyncTask를 시작하면 자동으로 실행되는 코드 부분 (스레드)
  - 비동기 작업을 수행한다. `Background thread`
  - 진행 상태를 UI에 업데이트하도록 하려면 publishProgress 메소드를 호출한다.
- onProgressUpdate() : AsyncTask가 동작하는 중간중간 상태를 업데이트하는 부분 (주로 UI 업데이트에 사용) `UI thread`
  - publishProgress()가 호출될 때마다 자동으로 호출된다.
- doInBackground 메소드에서 작업이 끝나면 onPostExecuted()로 결과 파라미터를 리턴하면서 그 리턴값을 통해 스레드 작업이 끝났을 때의 동작을 구현한다.
- onPostExecute() : AsyncTask가 종료되면 (doInBackground()가 완료되면) 실행되는 부분 `UI thread`

#### 2. RxJava

- Reactive Java에서 따온 이름으로, Reactive Programming을 자바에서 구현하기 위해서 등장한 라이브러리이다.

  > Reactive Programming (반응형 프로그래밍)
  >
  > 비동기 데이터 흐름을 중시하는 프로그래밍으로 외부에서 자유롭게 데이터 입출력을 해도 메인 스레드를 방해하지 않는 것을 중시하는 프로그래밍

- `Observable`이 발생하는 이벤트를 이벤트 스트림에 전달하고, `Observer`는 이벤트 스트림을 관찰하다가 원하는 이벤트를 감지하면 이에 따른 동작을 수행하는 방식이다.
- 비동기 이벤트를 매우 쉽게 처리할 수 있다.
- 이벤트나 데이터를 쉽게 가공 및 분배할 수 있다.

- 안드로이드에 RxJava를 적용하려면 App단의 build.gradle에 다음을 implementation 해야 한다.

  - 최신 버전 확인 : [RxJava](https://github.com/ReactiveX/RxJava), [RxAndroid](https://github.com/ReactiveX/RxAndroid)

  ```groovy
  dependencies {
      implementation 'io.reactivex.rxjava3:rxandroid:3.0.0'
      implementation 'io.reactivex.rxjava3:rxjava:3.1.2'
  }
  ```
  
#### 3. Coroutine
> [Coroutine은 안드로이드 개발자 문서에서 공식적으로 AsyncTask 대신 사용할 것을 권하고 있다.](https://developer.android.com/reference/android/os/AsyncTask)

- 안드로이드에서는 Kotlin 1.1 부터 Coroutine을 지원하고 있다.
- 디스패처를 사용하여 코루틴 실행에 사용되는 스레드를 확인한다.
- 적절한 코루틴 콘텍스트를 통해 코루틴 스코프를 지정하여 알맞은 작업을 비동기적으로 수행할 수 있다.
- 기본적으로 코루틴 스코프도 액티비티 생명주기를 무시하며 실행된다. 따라서 액티비티 수명주기에 취소해야 할 경우, CoroutineScope.cancel()을 호출해야 한다.

- Coroutine의 사용을 위해서는 App단의 build.gradle에 다음을 implementation 해 주어야 한다.

  ```groovy
  implementation 'org.jetbrains.kotlinx:kotlinx-coroutines-core:1.5.2'
  implementation 'org.jetbrains.kotlinx:kotlinx-coroutines-android:1.5.2'
  ```

[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#android)

## ANR

Application Not Responding의 약자로 애플리케이션이 반응하지 않을 때 발생한다. 앱의 메인 스레드(UI 스레드)가 일정시간 어떤 작업에 묶여 있으면 사용자의 입력처리를 할 수 없고 사용자는 앱이 응답하지 않는 경험을 한다.

### 발생 원인

- 앱이 입력 이벤트 또는 BroadcastReceiver에 5초 이내로 응답하지 않는 경우
  - 가장 나중에 입력된 이벤트 기준으로 5초 이내에 응답하지 않으면 발생
  - 예를들어, `12시 00분 5초`에 A이벤트 발생, `12시 00분 8초`에 B이벤트가 발생했고 아무 응답이 없다면, `12시 00분 10초`가 아닌 `12시 00분 13초`에 ANR이 발생
    프로그램이 응답하지 않을 때, 입력을 한 번 더 하면 그제서야 '응답없음 대화상자'가 뜨는 이유임
- 포그라운드에 활동이 없을 때 BroadcastReceiver가 10초 내에 실행을 완료하지 못한 경우

### ANR 회피 및 대처 방법

1. 장시간 작업이 필요한 경우, 메인 스레드 외 다른 스레드에서 비동기 처리를 해야한다.
   - 워커 스레드 생성
   - AsyncTask 사용
   - Coroutine 사용
2. 사용자에게 작업의 진행 과정을 안내하여(프로그레스 바, 로딩 ui) 기다리게 한다.
   - 워커 스레드에서 메인 스레드로 진행과정 UI 작업 전달
     - 메인 스레드의 핸들러로 Runnable 전달
     - Activity.runOnUiThread(Runnable)
     - View.post(Runnable)
   - 진행과정 안내를 위해 AsyncTask의 `onProgressUpdate()`를 구현하고 `publishProgress()` 호출
   - IO 스코프에서 장기 작업을 진행하고 Main 스코프에서 UI 작업 실행

[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#android)

## 메인 스레드와 워커 스레드

안드로이드에는 메인 스레드와 워크 스레드가 있다. 이 두 스레드는 각각 다른 목적으로 사용된다.

### 메인 스레드 (Main thread)

> 기본적으로 작업이 실행하는 **단일 스레드**. 어플리케이션 실행 시 생성된다.

- 안드로이드의 이벤트 생성 및 처리를 담당하고, 안드로이드에서 발생되는 여러 이벤트를 관련된 위젯으로 연동시킨다.
- **유일하게 화면의 UI를 업데이트** 하는 작업이 진행된다.
- 단일 스레드이기 때문에, 작업들이 순차적으로 진행된다. 만약 오래 걸리는 작업을 수행하게 되면 그 시간만큼 UI관련 작업이 처리되지 못하므로 응답하지 않는 상태가 된다. (안드로이드에선 5초 이상 앱이 반응하지 않으면 `ANR`에러를 발생시킨다.)  
⇒ **긴 시간이 걸리는 작업은 반드시 다른 스레드에서 처리**(API 11+) 해야 한다. (ex. 네트워크 통신, DB작업)

> [메인 스레드와 UI스레드는 같진 않다.](https://github.com/GumiMobile/CS-Study/blob/main/Android/Problem%20List/%EB%A9%94%EC%9D%B8%20%EC%8A%A4%EB%A0%88%EB%93%9C%EC%99%80%20%EC%9B%8C%EC%BB%A4%20%EC%8A%A4%EB%A0%88%EB%93%9C.md#%EB%A9%94%EC%9D%B8-%EC%8A%A4%EB%A0%88%EB%93%9C--ui-%EC%8A%A4%EB%A0%88%EB%93%9C)

### 워커 스레드 (Worker thread)

> 특정한 목적을 위해 따로 생성해서 동작하는 스레드

- UI 스레드를 블락하지 않기 위해서, 긴 시간이 필요한 작업의 경우(네트워크 통신, db작업) 별도의 스레드에서 수행해야 한다. 이때 사용하는 스레드가 워커 스레드이다.
- UI 관련 함수는 스레드에 안전하지 못하므로 **UI 관련 작업은 절대 하면 안된다**. 필요한 경우 메인 스레드에 작업을 요청해야 한다.
(워커 스레드에서 UI변경 작업을 수행하면 `android.view.ViewRootImpl$CalledFromWrongThreadException`이 발생한다.)
- 디버깅이 어렵다.

#### 메인 스레드에게 Runnable 객체로 UI변경 작업을 보내는 방법

- `Activity.runOnUiThread(Runnable)`
- `View.post(Runnable) or postDelayed(Runnable, long)`
- `Handler(Looper.getMainLooper()).post(Runnable) or postDelayed(Runnable, long)`

```kotlin
fun onClick(v: View) {
    Thread(Runnable {
        // a potentially time consuming task
        val bitmap = getBitMapFromServer("image.png")
        imageView.post {
            imageView.setImageBitmap(bitmap)
        }
    }).start()
}
```

#### API 요청을 수행하는 스레드

보통 `background thread` 에서 수행한다.

> background thread는 main이 아닌 모든 스레드를 통칭해서 부른다.   
> worker thread는 하나하나의 일하는 thread이고, background에서 동작한다.

<br>

[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#android)


## Bundel, Context

### Bundle

- Bundle이란 Map형태로 구현된 데이터의 묶음(Bundle)이다.
- Map형태라 key 값이 String이며, value값에는 Int, String과 같은 간단한 타입부터 Serializable, Parcelable 같은 복잡한 타입이 들어올 수 있다.

#### 역할
- Android에서는 activity간에 데이터를 주고 받을 때 Bundle클래스를 사용하여 전송한다.
- Activity가 중단될 때 `savedInstanceState`메서드를 호출하여 임시로 데이털르 저장하고, Activity 생성시 `Bundle savedInstanceState`객체를 가지고 다시 생성한다.
	
### Context

- 애플리케이션 환경에 대한 글로벌 정보를 갖는 추상클래스이며 실제 구현은 `Android 시스템`에 의해 제공된다.
- 리소스, 데이터베이스, preferences 등에 대한 접근을 제공하며, 액티비티 실행, 브로드캐스트, 인텐트 수신 등과 같은 애플리케이션 수준 작업에 사용된다.
- Context를 잘못 사용하면 앱이 비정상 종료되거나 메모리 누수가 발생하기도 한다.
- Context는 크게 Application Context와 Activity Context 두 종류로 나뉜다.

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FWVBRX%2FbtqWxfzYqKQ%2FIzwkE5gnxovo8rKrv7z02K%2Fimg.jpg" width="600"/>

#### Application Context
- 싱글턴 인스턴스이며, 액티비티에서 getApplicationContext()를 통해 접근 가능
- 애플리케이션 라이프사이클에 묶여있으며, 현재 컨택스트가 종료된 이후에도 컨택스트가 필요한 작업이나 액티비티 스코프를 벗어난 컨택스트가 필요한 작업에 적합하다.

#### Activity Context
- Activity Context는 activity 내에서 유효한 컨택스트이다. 
- 액티비티 라이프사이클과 연결되어 있다. 
- 액티비티와 함께 소멸해야 하는 경우에 사용한다.
	- 예를 들어, 액티비티와 라이프사이클이 같은 오브젝트를 생성해야 할 때 액티비티 컨택스트를 사용할 수 있다.


##### 그 외
- Service : 실행시 고유한 context를 가짐
- BroadcastReceiver : 자기자신이 context자체는 아니다. 리시버가 브로드캐스트를 처리할 때마다 새로운 context가 생성된다.
- ContentProvider : 자기자신이 context는 아니다. getContext()를 통해 context를 가져올 수 있다.

[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#android)


## MVC, MVP, MVVM 패턴

### 디자인 패턴

- 프로그램의 로직이 복잡해짐에 따라 컴포넌트 간의 의존도가 높아지고 유지보수가 힘들어짐
- **화면에 보여주는 로직과 데이터가 처리되는 로직을 분리**시킴으로써 코드의 재활용성을 높이고 불필요한 중복을 막기 위해 디자인 패턴을 사용한다
- MVC, MVVM, MVP는 비즈니스 로직과 사용자 인터페이스, 데이터를 나눠 설계

  > 비즈니스 로직
  >
  > 데이터와 사용자 인터페이스 사이에서 일어나는 정보 교환을 담당하는 역할. 즉 사용자의 요청에 따른 데이터 입력, 수정, 조회 등을 처리하는 루틴
  
### MVC ( Model - View - Controller )

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F7IE8f%2FbtqBRvw9sFF%2FAGLRdsOLuvNZ9okmGOlkx1%2Fimg.png" alt="MVC" width="300"/>


> MVP와 MVVM이 MVC에서 나온 개발방법론이기 때문에 가장 기본 방식이다.

- Model
	- 프로그램에서 실제 데이터 및 조작 로직을 처리하는 부분
	- 데이터와 비즈니스 로직을 포함하는 개념
	- controller & View 와 직접적인 관련은 전혀 없고, 간접적으로 전달한다.
	- View 또는 Controller에 묶이지 않은 독립적인 존재. 재사용이 가능하다

- View
	- 사용자에게 보여지는 UI 부분 (모델에서 처리한 데이터 및 결과를 반환받아 화면 구성)
	- 비즈니스 로직은 포함되지 않는다.
	- App 및 UI와의 상호작용에서 controller와 통신함
	- 유저가 어떤 Action을 해도 View는 알수없음
	
- Controller
	- 사용자의 요청을 해석하고 처리하여 결과를 반환하는 부분
	- 모델과 뷰를 느슨하게 연결하여 프로그램의 데이터 흐름을 관장
	- Model을 통해 가져온 데이터를 View에게 전달한다.
	- Model에게 직접 영향을 줄 수 있다.
	- 다수의 View를 선택할 수 있다.
	- 안드로이드에서는 주로 activity나 fragment로 표현된다.


- 동작

    (1) 사용자의 Action이 Controller에 들어온다.
    
    (2) Controller는 사용자의 Action을 확인하고, Model을 업데이트한다.
    
    (3) Controller는 Model을 나타내줄 View를 선택한다.
    
    (4) View는 Model을 이용하여 화면을 나타낸다.
	

> 참고) MVC에서 View가 업데이트 되는 방법
> 
> - View가 Model을 이용하여 직접 업데이트 하는 방법
> - Model에서 View에게 Notify하여 업데이트 하는 방법
> - View가 Polling으로 주기적으로  Model의 변경을 감지하여 업데이트 하는 방법

- 특징
	- Controller는 여러 개의 View를 선택할 수 있는 1:N 구조이다.
	- Controller는 View를 선택할 뿐 직접 업데이트하지 않는다.(View는 Controller를 모른다)
	
- 장점
	- 디자인 패턴 중 쉽고 단순한 편으로 보편적으로 많이 사용되는 디자인 패턴이다.
	- 모델, 뷰 분리
	- 모델의 비종속성으로 재사용 가능
    
- 단점
	- Controller가 Android API에 종속되어 테스트가 어렵다
	- View와 Model 사이의 의존성이 높다. View와 Model의 높은 의존성은 어플리케이션이 커질수록 복잡해지고 유지보수가 어려워질 수 있다.

### MVP ( Model - View - Presenter )

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FclZlsT%2FbtqBTLzeUCL%2FIDA8Ga6Yarndgr88g9Nkhk%2Fimg.png" alt="MVP" width="300" />

- View
	- 기본적인 것들은 MVC와 동일하나, MVC 패턴의 뷰에서 사용자 입력을 프레젠터에게 전달하는 역할 추가
	- Activity/Fragment가 View에 포함된다.
	- Presenter를 이용해 데이터를 주고받기 때문에 Presenter에 매우 의존적임.
	
- Model
   - 프로그램 내부적으로 쓰이는 데이터를 저장하고, 처리하는 역할을 함.(MVC 패턴과 동일)
   - View 또는 Presenter 등 다른 어떤 요소에도 의존적이지 않은 독립적인 영역임.

- Presenter
	- View와 Model 사이에서 data를 가공하고 전달하는 역할을 수행한다.
	- View가 요청한 정보를 Model로부터 받고 가공하여 View에게 전달한다. (Data만 전달하며 어떻게 보여줄지는 View가 담당.)
	- 모델과 뷰를 매개체라는 점에서 Controller와 유사하지만, View에 직접 연결되는 대신 인터페이스를 통해 상호작용 한다는 점이 다름.
		- 인터페이스를 통해 상호작용 하므로 MVC가 가진 테스트 문제와 함께 모듈화/유연성 문제 역시 해결할 수 있음.

- 동작

	(1) 사용자의 Action이 View를 통해 들어온다.
  
	(2) View는 data를 Presenter에게 요청한다.
  
	(3) Presenter는 Model에게 data를 요청한다.
  
	(4) Model은 Presenter에게 요청받은 data를 반환한다.
  
	(5) Presenter는 View에게 data를 반환한다.
	
	(6) View는 Presenter가 응답한 데이터를 이용하여 화면을 나타낸다.	

- 특징
	- Presenter는 View와 Model의 인스턴스를 가지고 있어 둘을 연결하는 역할을 한다.
	- Presenter와 View는 1:1 관계이다.
	- 단순 Interface이기 때문에 테스트가 용이하고 모듈화/유연성 문제가 해결되었다.

- 장점
	- View와 Model을 분리하고 Presenter를 통해서만 데이터를 전달받기 때문에 MVC 패턴의 단점이었던 View와 Model의 의존성 문제를 해결했다.
	
- 단점
	- 뷰-프레젠터 1:1관계 생성, 즉, 뷰-프레젠터의 강한 의존성 생성
		- 이를 해결하기위해 MVVM 패턴 사용
	- Android API를 참조해서는 안된다.(권장)
	- Controller와 같이 코드가 집중되면 성능이 저하되고 유지보수가 어려워진다.


[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#android)
