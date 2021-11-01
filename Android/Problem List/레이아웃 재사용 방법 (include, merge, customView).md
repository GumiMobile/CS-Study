# 레이아웃 재사용 방법
## 김민수

### Raw 레이아웃 재사용

`<include/>` `<merge/>` 태그를 이용하여 레이아웃에 다른 레이아웃을 넣을 수 있다. Raw 레이아웃을 재사용하면 복잡한 화면을 구성하는데 도움이 된다.

#### 재사용 가능한 레이아웃 만들기

``` xml
reuse_item1.xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:background="@color/titlebar_bg"
    android:orientation="vertical"
    tools:showIn="@layout/activity_main" >

    <ImageView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:src="@drawable/gafricalogo" />
    <TextView
        android:id="@+id/textView1"
        android:text="Second include"
        android:textAppearance="?android:attr/textAppearanceMedium"/>
    
</LinearLayout>

또는
reuse_item2.xml
<TextView xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/textView2"
    android:text="First include"
    android:textAppearance="?android:attr/textAppearanceMedium"/>
```

위젯이 여러 개라면 반드시 `ViewGroup`으로 감싸줘야한다. 또한 xml의 최상위 요소는 `xmlns:android="http://schemas.android.com/apk/res/android"` 네임스페이스를 지정해야 한다.

> tools:showIn 속성에 정의된 xml을 열었을 때, include된 재사용 레이아웃을 보여주는 속성이다. 컴파일 시에 제거된다.

#### include

``` xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/main_container"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical" >

    <!-- First include file -->
    <include layout="@layout/reuse_item1" />

    <!-- Second include file -->
    <include 
        android:id="@+id/the_text"
        android:layout_height="match_parent"
        android:layout_width="match_parent"
        layout="@layout/reuse_item2" />

</LinearLayout>
```

재사용 가능한 레이아웃은 `<include/>`를 이용하여 레이아웃에 삽입할 수 있다. `layout` 속성에 넣을 레이아웃 이름을 할당한다. 또한 include 태그 내의 `android:layout_`으로 시작하는 속성을 모두 재정의 가능하다. 다만 재정의한 속성을 적용하려면 `layout_height`, `layout_width`를 **반드시** 재정의 해야한다.

첫번째 include 태그의 레이아웃은 리니어 레이아웃을 갖는 reuse_item1.xml이다. 따라서 리니어 레이아웃 내에 리니어 레이아웃이 삽입된다. 이는 뷰계층이 더 깊어지는 문제가 발생하고, 리니어 레이아웃을 불필요하게 렌더링하는 부작용이 있다.

#### merge

```xml
reuse_item1.xml
<merge xmlns:android="http://schemas.android.com/apk/res/android">
    
    <ImageView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:src="@drawable/gafricalogo" />
    <TextView
        android:id="@+id/textView1"
        android:text="Second include"
        android:textAppearance="?android:attr/textAppearanceMedium"/>
    
</merge>
```

뷰 그룹의 중복을 방지하려면, reuse_item1.xml의 뷰 그룹인 리니어 레이아웃 대신 merge로 대체한다. include 시, 시스템은 merge 태그를 무시하고 `<inclde/>` 대신 이미지뷰와 텍스트뷰를 레이아웃에 직접 배치한다.

#### custom view

커스텀 뷰를 구현하는 방법은 두가지가 있다.

- ViewGroup을 상속받고 만들어둔 xml 추가
- View 상속

``` kotlin
// view group 상속
class CustomView: ConstraintLayout { 
    lateinit var view: View
    
    constructor(context: Context): super(context)
    constructor(context: Context, attrs: AttributeSet): super(context, attrs)
    constructor(context: Context, 
                attrs: AttributeSet, 
                defStyleAttr: Int): super(context, attrs, defStyleAttr)
    constructor(context: Context,
                attrs: AttributeSet,
                defStyleAttr: Int,
                defStyleRes: Int): super(context, attrs, defStyleAttr, defStyleRes)
    
    init {
        init()
    }

    fun init() {
        view = inflate(context, R.layout.custom, this)
        //do something
    }
}

// View 상속
class customView(context: Context, attrs: AttributeSet): View(context, attrs) {
    //
}
```

CustomView 클래스는 컨스트래인트 레이아웃을 상속받는다. 그 후 view라는 멤버변수를 초기화하며 커스텀 레이아웃을 자신에게 inflate한다.

또는 View 자체를 상속받아 구현할 수 있다.

```xml
activity_main.xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <com.example.MyApp.CustomView
        android:id="@+id/customView"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"/>

    <!-- ... -->
</RelativeLayout>
```

xml에서 커스텀뷰의 클래스패스를 태그로 사용하면 custom view를 배치할 수 있다.
