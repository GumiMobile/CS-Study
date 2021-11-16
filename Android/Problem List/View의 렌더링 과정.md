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

## 김민수

### 뷰의 렌더링이 이뤄지는 계층

1. **Window**

   하나의 화면에서 **여러 개**의 Window를 가질 수 있다. Window는 그래픽이 그려지는 창이며, 1개의 Surface를 갖고 있다. Window는 `WindowManager`에 의해 관리되며, `WindowManager`는 **사용자의 입력 이벤트**를 처리한다.

2. **Surface**

   Surface는 화면을 구성할 픽셀을 보유한 객체이다. 각 Surface는 1개 이상(보통 2개)의 buffer를 갖는다. 각 버퍼의 내용은 Surface Flinger에 의해 Z-order에 따라 화면에서 합성된다.

   > 이중 버퍼 렌더링
   >
   > 화면에 출력될 화면 데이터는 프레임 버퍼에 저장된다. 하나의 버퍼만 가질 경우 이미지가 반복해서 그려지거나, 이미지 렌더링이 덜 끝났음에도 화면 주사율 때문에 렌더링을 다시 해야할 때, 렌더링이 미완된 버퍼가 재 렌더링되며 화면이 깜빡이는 Flicker 현상이 발생할 수 있다. 따라서 버퍼 2개를 두어, 다음 화면 출력 대비를 한다.

3. **Canvas**

   실제 UI의 비트맵이 그려지는 공간이다.

4. **View**

   View는 Window안에 있는 UI 엘리먼트이다. Window가 다시 그려져야한다면 Surface는 locked 되고 Canvas를 반환한다. 이후 Draw Traversal이 뷰 계층을 따라 내려가며 각 뷰를 Canvas에 전달하여 뷰를 그린다. 해당 과정이 끝나면 Surface는 unlocked되고 Surface의 버퍼는 포그라운드로 전환되어 Buffer의 내용이 Surface Flinger에 의해 화면에 합성된다.

### 안드로이드가 뷰를 그리는 방법

액티비티가 포커스를 받으면, 액티비티는 자신 레이아웃의 루트노드를 시스템에 제공하여 드로잉 과정을 처리한다. 루트 노드부터 **레이아웃 트리를 따라 내려가며** 렌더링 과정이 진행된다. 레이아웃을 그리는 과정은 크게 2가지 과정으로 이뤄진다.

1. Measure Pass, measure(widthMeasureSpec: Int, heightMeasureSpec: Int)

   - 뷰의 크기를 알아내기 위해 호출하며 뷰의 크기 계산을 위해 onMeasure()를 호출한다.

   - 매개변수인 measureSpec의 모드에 따라 onMeasure()에서 뷰의 크기를 계산

     - MeasureSpec.AT_MOST

       상위 요소가 하위 요소에 최대 크기를 적용하는데 사용

     - MeasureSpec.EXACTLY

       상위 요소가 하위 요소에 정확한 크기를 적용하는데 사용, 하위 요소는 반드시 이 크기를 사용해야함

     - MeasureSpec.UNSPECIFIED

       상위 요소가 하위 요소 View의 크기를 판별하는데 사용

   - measure()이 반환되면 뷰는 크기를 갖는다.(정확히는 onMeasure()이 끝나면)

2. Layout Pass, layout(left: Int, top: Int, right: Int, bottom: Int)

   - setFrame()을 통해 View의 left, top, right, bottom(부모로부터 상대적 위치)을 set한다.
   - 자식 뷰의 크기나 위치를 (재)할당 해야하면, onLayout(changed: Boolean, l, t, r, b)을 호출한다.
     - changed: 새로운 사이즈나 위치인지? true: false

### 뷰의 수명주기

<img src="https://cdn-images-1.medium.com/max/1600/1*hKqtBgx594fylFgX-jMDQA.png" width=600/>

#### 생성자

- View(context: Context)

  코드로 뷰를 간단히 생성할 때 사용하는 생성자로 Context는 현재 앱의 테마, 리소스등을 구성하는데 사용된다.

- View(context: Context, attrs: AttirbuteSet)

  XML로 부터 뷰를 inflate할 때 호출되는 생성자로 XML파일에서 별도로 지정한 속성을 제공한다.

  > AttributeSet
  >
  > XML 문서의 태그와 연관된 속성의 모음. /values/attr.xml 에 저장하여 사용한다. 

#### Attachment / Detachment

View가 Window에서 연결되거나 분리될 때의 단계이다. 이에 대한 콜백은 다음과 같다

- onAttachedToWindow()

  View가 Window에 연결되면 호출된다. View가 활성화 될 수 있고, 드로잉 할 표면이 있음을 알고있는 단계이다.

- onDetachedFromWindow()

  View가 Window에서 분리될 때 호출된다. 이 메소드는 ViewGroup에서 View 제거를 호출하거나 액티비티가 Destroyed 일 때 호출된다.

#### Measure()

​	위 과정에서 설명 완료

#### Layout()

​	위 과정에서 설명 완료

#### Draw()

​	ViewGroup.childDraw()에서 호출된다. onDraw()를 호출하여 실제 View를 그린다.

### 뷰의 업데이트

- invalidate(): 뷰의 변화가 생겨 다시 그려야할 때 호출한다.
- requestLayout(): 뷰를 처음부터 그려야 할 때 (ex: 뷰가 레이아웃을 벗어남) 호출한다.
