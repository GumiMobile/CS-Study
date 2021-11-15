# View의 렌더링 과정

## 김현수

### 안드로이드 화면 구성
안드로이드의 화면은 아래와 같은 단위로 구성된다. 화면을 구성하는 최소 단위는 View이며 최대 단위는 Window이다.
```
Window > Surface > Canvas > View
```

#### Window
Window는 화면 구성의 가장 상위 요소로 무언가를 그릴 수 있는 화면상의 사각 영역을 말한다.

하나의 화면 안에는 여러 개의 Window가 존재할 수 있고, 각각의 Window는 용도에 따라 고유한 타입을 가진다. 이들은 WindowManager에 의해 관리된다.

어플리케이션은 WindowManager와 상호작용하여 Window를 만들 수 있다. WindowManager는 각각의 Window에 Surface를 만들어 어플리케이션에 전달하고, 어플리케이션은 이를 통해 화면을 렌더링한다.

Window는 터치 이벤트, 키 이벤트 등 사용자 이벤트를 받아서 처리할 수 있다.

#### Surface
Surface는 화면에 합성되는 픽셀을 보유한 객체이다.

화면에 보여지는 모든 윈도우(스테이터스 바, 다이얼로그, 액티비티 등)는 자신만의 Surface를 가지고 있으며, Surface Flinger가 각 Surface의 픽셀들을 Z-order에 따라 합성하여 실제 화면에 렌더링하는 역할을 맡고 있다.

Surface는 렌더링시에 더블 버퍼링 방식을 이용하기 위해 일반적으로 두개의 버퍼를 가지고 있다.

#### Canvas
Canvas는 모든 드로잉 메서드를 포함하고 있는 클래스이다. 각종 도형, 선 등을 그리기 위한 모든 로직이 Canvas 내에 포함되어 있다. Canvas는 Bitmap 또는 OpenGL 컨테이너 위에 그려진다.

#### View
View는 Window 내에 존재하는 인터랙티브한 UI 요소를 말한다. (Button, TextView 등)

### View와 ViewGroup

안드로이드에서 UI요소들은 크게 View와 ViewGroup으로 이루어져 있으며 아래와 같은 특징들을 가진다.

- View는 화면상의 UI를 구성한다.
- ViewGroup은 ViewGroup과 View를 포함한다.
- TextView, Button등 기본적인 화면 구성 요소들은 View에 상속되어 구현된다.
- 개별적으로 속성을 가지며 개별 속성은 Parent로부터 상속을 받을 수 있다.
- View는 Window의 Surface를 이용하여 화면을 그리고, 이벤트를 어떻게 처리할지에 대한 구현을 하는 객체이다.

### View의 생명주기

<img src="https://miro.medium.com/max/692/1*abc0UlGj1myFD0eph4pZjQ.png"/>

#### View가 그려지는 방식

1. Constructor
	- CustomView(context: Context) : 코드로 생성하면 호출
	- CustomView(context: Context, attributeSet: AttributeSet) : xml로 생성하면 호출
2. onAttachedToWindow : Parent View가 addView(view: View)를 호출하면 해당 View가 Window에 연결되며, 이 단계부터 id를 통해 접근할 수 있다.
3. onMeasure : View의 사이즈 측정
	- MeasureSpec : Parent View에서 Child View로 요구조건을 보내기 위해 사용
		- MeasureSpec.EXACTLY : 직접적으로 width와 height에 대해 하드코딩 할때
		- MeasureSpec.AT_MOST : Parent가 Child의 Maximum Size를 설정하기 위해 사용한다.
		- MeasureSpec.UNSPECIFIED : Parent에 의해 사용되며 제한이 없기 때문에 Child View가 원하는 사이즈를 가질 수 있다.
4. onLayout : 개별 Child View들의 사이즈 및 위치 할당
5. onDraw : View 그리기 시작, Canvas와 Paint를 사용하여 그리기 시작하며 Canvas는 모양을 그리고 Paint는 색을 칠한다. (리소스를 많이 사용하기 때문에 할당된 객체를 재사용할 수 있도록 해야한다.)

#### View Update
- invalidate() : View에 변화가 생겨서 다시 그려야 할때 사용한다.
- requestLayout() : View를 처음부터 그려야 할때 사용한다.

#### Animation
View의 Animation은 frame 단위로 View에 애니메이션으로 변화가 있을때마다 invalidate()를 호출하여 뷰를 그린다. 대표적으로 애니메이션을 사용하는 클래스는 ValueAnimator 이다.
