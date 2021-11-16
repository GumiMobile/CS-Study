# MVC, MVP, MVVM 패턴

## 김현수

|패턴|설명|
|:---|:---|
|MVC (Model - View - Controller)|Model 은 데이터, View 는 XML 파일, Controller 는 Activity|
|MVP (Model - View - Presenter)|Model 은 데이터, View 는 Activity, Presenter 는 Model 과 View 를 연결해주는 매개체|
|MVVM (Model - View - ViewModel)|Model 은 데이터, View 는 Activity, ViewModel 은 Model 과 View 를 연결해주는 매개체|


### MVC ( Model - View - Controller )
<img src="https://images.velog.io/images/its-mingyu/post/4a07a5c4-541f-49c9-af8b-9020f0f7c21c/%EA%B7%B8%EB%A6%BC1.png" width="250px"/>

- View

    (1) 사용자에게 보여지는 UI를 나타낸다.
    
    (2) Model로부터 data를 받아 사용자에게 보여준다.

- Model
 
    (1) 어플리케이션에서 사용되는 data와 그 data를 처리한다.
    
    (2) View에 의존적이지 않기 때문에 재사용이 가능하다.

- Controller

    (1) 사용자의 입력(Action)을 받고 처리한다.

    (2) 주로 Activity나 Fragment로 표현된다.

    (3) Model의 data 변화에 따라 View를 선택한다.

- 동작

    (1) 사용자의 Action이 Controller에 들어온다.
    
    (2) Controller는 사용자의 Action을 확인하고, Model을 업데이트한다.
    
    (3) Controller는 Model을 나타내줄 View를 선택한다.
    
    (4) View는 Model을 이용하여 화면을 나타낸다.

- 특징

    (1) Controller는 여러 개의 View를 선택할 수 있는 1:N 구조이다.
    
    (2) Controller는 View를 선택할 뿐 직접 업데이트하지 않는다.(View는 Controller를 모른다)
    
    (3) View와 Model을 완벽하게 분리하고, Model 테스트가 용이하다.

- 단점

    (1) Controller가 Android API에 종속되어 테스트가 어렵다
    
    (2) View를 변경하면 Controller도 변경해야 한다.
    
    (3) 많은 코드들이 Controller에 집중되면 성능이 저하되고 유지보수가 어려워진다.

### MVP ( Model - View - Presenter )
<img src="https://images.velog.io/images/its-mingyu/post/a8b14cfe-b2cf-4fe9-abc7-a2fecd261e2e/%EA%B7%B8%EB%A6%BC2.png" width="250px"/>

- View

	(1) 기본적인 것들은 MVC와 동일하나, Activity/Fragment가 View에 포함된다.
  
	(2) View를 관리하는 Interface를 추가하여 Presenter를 독립적으로 만들어준다.

- Model

	(1) 어플리케이션에서 사용되는 data와 상태에 대한 business logic을 처리한다.
  
	(2) View에 의존적이지 않기 때문에 재사용이 가능하다.

- Presenter

	(1) View와 Model 사이에서 data를 가공하고 전달하는 역할을 수행한다.
  
	(2) View가 요청한 정보를 Model로부터 받고 가공하여 View에게 전달한다.
  
	(3) Controller와의 차이점은 Interface라는 점이다.

- 동작

	(1) 사용자의 Action이 View를 통해 들어온다.
  
	(2) View는 data를 Presenter에게 요청한다.
  
	(3) Presenter는 Model에게 data를 요청한다.
  
	(4) Model은 Presenter에게 요청받은 data를 반환한다.
  
	(5) Presenter는 View에게 data를 반환한다.

- 특징

	(1) Presenter는 View와 Model의 인스턴스를 가지고 있어 둘을 연결하는 역할을 한다.
  
	(2) Presenter와 View는 1:1 관계이다.
  
	(3) 단순 Interface이기 때문에 테스트가 용이하고 모듈화/유연성 문제가 해결되었다.

- 단점

	(1) View와 Presenter 간의 의존성이 높다.
  
	(2) Android API를 참조해서는 안된다.(권장)
  
	(3) Controller와 같이 코드가 집중되면 성능이 저하되고 유지보수가 어려워진다.


### MVVM ( Model - View - View Model )
<img src="https://images.velog.io/images/its-mingyu/post/4efc6cd5-e64d-463a-9d2a-b481fd01c6e7/%EA%B7%B8%EB%A6%BC3.png" width="250px"/>

- View

	(1) 기본적인 것들은 MVC와 동일하나, Activity/Fragment가 View에 포함된다.
  
	(2) ViewModel을 관찰하여 UI를 갱신한다.
  
	(3) Data Binding을 위해 gradle과 xml을 수정해야한다
  
	(4) 이를 통해 View는 ViewModel에 의해 Model과 유연한 binding이 가능하게 된다.

- Model

	(1) MVC, MVP와 동일하다.
  
	(2) 기존 모델에 business logic을 추가하기 위해 MVVM에 SquareParams라는 Model을 추가했다.
  
	(3) DB 사용이나 Retrofit을 통한 백엔드 API 호출 등을 주로 수행한다.

- ViewModel

	(1) 기본적으로 View에 종속되지 않는다.(그래서는 안된다)
  
	(1) Model을 래핑하고 View에 필요한 Observable data를 준비한다.
  
	(2) View가 Model에 Event를 전달할 수 있도록 Hook(BindingAdapter)을 준비한다.

- 동작

	(1) 사용자의 Action들은 View를 통해 들어오게 된다.
  
	(2) Command pattern으로 ViewModel에 Action을 전달한다.
  
	(3) ViewModel은 Model에게 data를 요청한다.
  
	(4) ViewModel은 응답받은 data를 가공하여 저장한다.
  
	(5) View는 ViewModel과 Data Binding하여 화면을 나타낸다.

- 특징

	(1) Command Pattern과 Data Binding을 사용하여 구현한다.
  
	(2) View와 Model 사이의 의존성이 없다.
  
	(3) View와 ViewModel 사이의 의존성도 없다.
  
	(4) 위처럼 모든 부분이 독립적이므로 모듈화가 가능하다.

- 단점

	(1) ViewModel의 설계가 어렵다.
  
	(2) View가 변수와 표현식 모두에 Binding될 수 있으므로 갈수록 presentation logic이 늘어나 XML이 방대해진다. 이것을 방지하려면 항상 ViewModel에서 직접 값을 가져오는 것이 좋다.
