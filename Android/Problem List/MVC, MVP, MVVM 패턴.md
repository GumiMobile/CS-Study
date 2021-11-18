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

## 우지현

### MVC

#### 구조

> Model + View + Controller

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F7IE8f%2FbtqBRvw9sFF%2FAGLRdsOLuvNZ9okmGOlkx1%2Fimg.png" alt="MVC" width="300"/>

- Model : 어플리케이션에서 사용되는 데이터와 그 데이터를 처리하는 부분
- View : 사용자에게 보여지는 UI 부분
- Controller : 사용자의 입력(Action)을 받고 처리하는 부분

#### 동작

1. 사용자들의 Action들은 Controller에 들어오게 된다.
2. Controller는 사용자의 Action을 확인하고, Model을 업데이트 한다.
3. Controller는 Model을 나타내줄 View를 선택한다.
4. View는 Model을 이용하여 화면을 나타낸다.

참고) MVC에서 View가 업데이트 되는 방법

- View가 Model을 이용하여 직접 업데이트 하는 방법
- Model에서 View에게 Notify하여 업데이트 하는 방법
- View가 Polling으로 주기적으로  Model의 변경을 감지하여 업데이트 하는 방법

#### 특징

- Controller는 여러 개의 View를 선택할 수 있는 1 : n의 구조이다.
- Controller는 View를 선택할 뿐 직접 업데이트하지 않는다. (View는 Controller를 알지 못함)

#### 장점

- 널리 사용되고 있는 패턴이라는 점에 걸맞게 가장 단순하다.
- 단순하다 보니 보편적으로 많이 사용되는 디자인 패턴이다.

#### 단점

- View와 Model 사이의 의존성이 높다. View와 Model의 높은 의존성은 어필리케이션이 커질수록 복잡해지고 유지보수가 어려워질 수 있다.

### MVP

> Model + View + Presenter

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FclZlsT%2FbtqBTLzeUCL%2FIDA8Ga6Yarndgr88g9Nkhk%2Fimg.png" alt="MVP" width="300" />

#### 구조

- Model : 어플리케이션에서 사용되는 데이터와 그 데이터를 처리하는 부분 (MVC 패턴과 동일)
- View : 사용자에게 보여지는 UI 부분 (MVC 패턴과 동일)
- Presenter : View에서 요청한 정보로 Model을 가공하여 View에 전달해주는 부분

#### 동작

1. 사용자의 Action들은 View를 통해 들어오게 된다.
2. View는 데이터를 Presenter에 요청한다.
3. Presenter는 Model에게 데이터를 요청한다.
4. Model은 Presenter에서 요청받은 데이터를 응답한다.
5. Presenter는 View에게 데이터를 응답한다.
6. View는 Presenter가 응답한 데이터를 이용하여 화면을 나타낸다.

#### 특징

- Presenter는 View와 Model의 인스턴스를 가지고 있어 둘을 연결하는 접착제 역할을 한다.
- Presenter와 View는 1 : 1 관계이다.

#### 장점

- View와 Model의 의존성의 없다. MVP 패턴은 Presenter를 통해서만 데이터를 전달받기 때문에 MVC 패턴의 단점이었던 View와 Model의 의존성 문제를 해결했다.

#### 단점

- View와 Model 사이의 의존성은 해결되었지만, View와 Presenter 사이의 의존성이 높아졌다. 어플리케이션이 복잡해질수록 View와 Presenter 사이의 의존성이 강해진다.

### MVVM

> Model + View + View Model

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FCiXz0%2FbtqBQ1iMiVT%2FstaXr7UO95opKgXEU01EY0%2Fimg.png" alt="MVVM" width="300" />

#### 구조

- Model : 어플리케이션에서 사용되는 데이터와 그 데이터를 처리하는 부분 (다른 패턴과 동일)
- View : 사용자에게 보여지는 UI 부분 (다른 패턴과 동일)
- View Model : View를 표현하기 위해 만든 View를 위한 Model. View를 나타내기 위한 Model이자 View를 나타내기 위한 데이터 처리를 하는 부분

#### 동작

1. 사용자의 Action들은 View를 통해 들어오게 된다.
2. View에 Action이 들어오면, Command 패턴으로 View Model에 Action을 전달한다.
3. View Model은 Model에게 데이터를 요청한다.
4. Model은 View Model에게 요청받은 데이터를 응답한다.
5. View Model은 응답 받은 데이터를 가공하여 저장한다.
6. View는 View Model과 Data Binding하여 화면을 나타낸다.

#### 특징

- [Command 패턴](https://ko.wikipedia.org/wiki/%EC%BB%A4%EB%A7%A8%EB%93%9C_%ED%8C%A8%ED%84%B4)과 Data Binding 두 가지 패턴을 사용하여 구현되었다.
- Command 패턴과 Data Binding을 이용하여 View와 View Model 사이의 의존성을 없앴다.
- View Model과 View는 1 : n 관계이다.

#### 장점

- View와 Model 사이의 의존성이 없다.
- Command 패턴과 Data Binding을 사용하여 View와 View Model 사이의 의존성 또한 없다.
- 각각의 부분이 독립적이기 때문에 모듈화하여 개발할 수 있다.

#### 단점

- View Model의 설계가 쉽지 않다.

## 김민수

### 디자인 패턴

- 프로그램의 로직이 복잡해짐에 따라 컴포넌트 간의 의존도가 높아짐

- 유지보수가 힘들어짐

- 이를 해결하기 위한 방법론으로 아키텍처 디자인 패턴 등장

- MVC, MVVM, MVP는 비즈니스 로직과 사용자 인터페이스, 데이터를 나눠 설계

  > 비즈니스 로직
  >
  > 데이터와 사용자 인터페이스 사이에서 일어나는 정보 교환을 담당하는 역할. 즉 사용자의 요청에 따른 데이터 입력, 수정, 조회 등을 처리하는 루틴

### MVC

모델 - 뷰 - 컨트롤러로 이뤄진 디자인 패턴

<img src=https://magi82.github.io/images/2017-2-24-android-mvc-mvp-mvvm/mvc.png width=600/>

- Model
  - 데이터와 비즈니스 로직을 포함하는 개념
  - 컨트롤러에 의해 수행되는 요소
- View
  - 모델에서 처리한 데이터 및 결과를 반환받아 화면 구성
  - 비즈니스 로직은 포함되지 않는다.
- Controller
  - 사용자의 요청을 해석하고 처리하여 결과를 반환
  - 모델과 뷰를 느슨하게 연결하여 프로그램의 데이터 흐름을 관장

#### 장점

- 모델, 뷰 분리
- 모델의 비종속성으로 재사용 가능
- 디자인 패턴 중 쉽고 단순한 편

#### 단점

- 모델, 뷰를 분리하였지만 직, 간접적 의존 관계 발생, 이를 해결하기 위해 MVP패턴 사용

### 안드로이드에선?

<img src=https://media.vlpt.us/images/jojo_devstory/post/820c2322-6762-421f-932a-d21b986fb976/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202020-03-09%20%EC%98%A4%EC%A0%84%2012.07.13.png width=600/>

장점

- 액티비티, 프래그먼트에서 대부분의 로직을 처리하므로 이를 잘 처리하면 개발기간이 짧아진다.

단점

- 스파게티 코드가 될 확률이 높다.
- 컨트롤러(액티비티, 프래그먼트)가 안드로이드 api에 깊히 종속되어 테스트가 어렵다.



### MVP

모델 - 뷰 - 프레젠터로 이뤄진 디자인 패턴

<img src=https://magi82.github.io/images/2017-2-24-android-mvc-mvp-mvvm/mvp.png width=600/>

- Model
  - MVC 패턴의 모델과 같음
- View
  - MVC 패턴의 뷰에서 사용자 입력을 프레젠터에게 전달하는 역할 추가
  - 프레젠터를 통해 전달 받은 결과로 화면 구성
- Presenter
  - 모델과 뷰 사이의 매개체
  - 뷰로부터 전달받은 이벤트를 처리하고 모델 업데이트
  - 모델의 로직 수행 결과를 뷰에 전달하여 UI 변경

#### 장점

- MVC에서 문제가 된 뷰-모델 의존성 해결

#### 단점

- 뷰-프레젠터 1:1관계 생성, 즉, 뷰-프레젠터의 강한 의존성 생성, 이를 해결하기위해 MVVM 패턴 사용

### MVVM

모델-뷰-뷰모델로 이뤄진 디자인 패턴

<img src="https://magi82.github.io/images/2017-2-24-android-mvc-mvp-mvvm/mvvm.png" width=600/>

- Model
  - 다른 패턴들과 비슷하다.
  - 모델은 뷰, 뷰 모델 계층을 신경 쓸 필요 없다.
- View
  - 뷰모델을 observe 하고 있고, 여러 뷰가 하나의 뷰 모델을 참조 할 수 있다.
  - 뷰모델에게 사용자 액션을 전달한다.
- ViewModel
  - 뷰에서 사용할 메소드, 필드를 갖는 뷰의 추상화된 모델
  - MVC의 컨트롤러, MVP의 프레젠터를 대신하여 데이터 바인딩, Obsevable을 통해 자신의 상태를 뷰에게 알려 뷰의 갱신을 일으킬 수 있다.

#### 장점

- 뷰와 모델이 서로를 전혀 신경 쓰지 않기에 유닛 테스트 용이

#### 단점

- 뷰모델 설계가 어렵다.


## 이유진

**화면에 보여주는 로직과 데이터가 처리되는 로직을 분리**시킴으로써 코드의 재활용성을 높이고 불필요한 중복을 막기 위해 다음의 디자인 패턴을 사용한다. `model`과 `view`을 구성하는 방법과 의존성을 제어하는 방법에 차이가 있다.

> why? 로직을 분리시키지 않으면 규모가 커졌을 때 코드가 엉켜서 가독성이 떨어지고 파악하기 어려워진다.
> 

### MVC 패턴

![https://magi82.github.io/images/2017-2-24-android-mvc-mvp-mvvm/mvc.png](https://magi82.github.io/images/2017-2-24-android-mvc-mvp-mvvm/mvc.png)

- MVP와 MVVM이 MVC에서 나온 개발방법론이기 때문에 가장 기본 방식이다.
- Controller : 사용자의 입력을 받고 처리하는 부분
    - Model을 통해 가져온 데이터를 View에게 전달한다.
    - Model에게 직접 영향을 줄 수 있다.
    - 다수의 View를 선택할 수 있다.
    - 안드로이드에서는 주로 activity나 fragment로 표현된다.
- Model : 프로그램에서 실제 데이터 및 조작 로직을 처리하는 부분
    - controller & View 와 직접적인 관련은 전혀 없고, 간접적으로 전달한다.
    - 독립적인 존재
- View : 사용자에게 제공되는 UI 부분
    - controller에게 영향을 줄 수 없다.
- View와 Model은 서로 영향을 주지 않는다!! (observer를 이용하면 간접적으로 가능)
- 사용자의 입력은 Controller로 들어온다. 이를 통해 Model을 업데이트하고 불러온 다음 View를 선택해서 화면에 보여준다.
- ⇒ 규모가 커질수록 Controller도 커진다.
- ⇒ view와 controller가 강한 결합을 갖는다. (안드의 경우 어쩌면 뷰의 확장) view를 변경하면 controller에서도 변경해야한다.

### MVP 패턴

![https://magi82.github.io/images/2017-2-24-android-mvc-mvp-mvvm/mvp.png](https://magi82.github.io/images/2017-2-24-android-mvc-mvp-mvvm/mvp.png)

- 사용자 입력을 view를 통해 받는다.
- presenter가 사용자가 요청한 정보를 model로부터 가져와 view로 전달한다.
- view와 presenter는 서로를 참조하고, presenter는 model을 조작할 수 있다. (MVC와 비슷)
- view와 Model은 서로에게 영향을 줄 수 없다. 오로지 presenter을 통해서 전달한다.
- view와 model이 영향을 주지 않는 것은 성공했지만, view-presenter이 강한 결합을 가지게 되고, view마다 presenter가 존재한다.
- ⇒ 코드가 길어진다. (view마다 presenter가 있어서)
- ⇒ 시간이 지날수록 presenter에 추가 로직이 모인다. 거대하고 다루기 어려운데 분리하기도 어려워진다.

### MVVM 패턴

![https://magi82.github.io/images/2017-2-24-android-mvc-mvp-mvvm/mvvm.png](https://magi82.github.io/images/2017-2-24-android-mvc-mvp-mvvm/mvvm.png)

- 사용자는 view를 통해 입력하고, view의 값을 바탕으로 viewModel의 값이 변경된다.
- viewModel : view를 표현하기 위해 만들어진 view를 위한 model
    - view에 영향을 주지 않는다.
    - model의 데이터를 수정한다.
    - Controller와 비슷한 역할
    - 특정 view에 맞춰진 model로 view는 viewModel만 고려한다.
    - 원리) view에 입력이 들어오면 command패턴(명령)을 통해 viewModel에 명령을 내리고, `DataBinding`덕에 viewModel 값이 변화하면 바로 View의 정보가 바뀐다. (자동갱신)
- view
    - model에 영향을 주지 않는다.
    - viewModel의 데이터를 변경한다.
- dataBinding과 명령을 통해 의존성을 해결한다.
- ⇒ view가 변수와 표현식 모두에 바인딩 가능해서 시간이 갈수록 관계없는 로직을 xml에 추가하게 될 수도 있다. 따라서 viewBinding 표현식에서 계산하지 말고 항상 부모 모델에서 값을 가져오는 게 좋다.
