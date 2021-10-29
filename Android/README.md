# Android

* [액티비티, 생명주기](#액티비티-생명주기)
* [액티비티 vs 프래그먼트](#액티비티와-vs-프래그먼트)

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

<br />
