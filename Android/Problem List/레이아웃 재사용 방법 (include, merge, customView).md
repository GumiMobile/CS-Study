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

## 김현수

### 레이아웃 재사용
- UI가 특정 레이아웃 구성을 여러 위치에서 반복할 때 재사용 가능한 효율적인 레이아웃 구성을 생성하여 적절한 UI 레이아웃에 포함할 수 있다.

- 전체 레이아웃을 효율적으로 재사용하려면 <include/> 및 <merge/> 태그를 사용하여 현재 레이아웃에 다른 레이아웃을 삽입할 수 있다.

### include

한 레이아웃 파일의 내용을 다른 레이아웃에 삽입할 때 사용된다.

1. 재사용할 xml파일 작성 (example.xml)
2. 작성한 xml파일을 새로운 xml 파일에 추가

```xml
<include
        android:id="@+id/include1"
        android:layout_width="match_parent"
        android:layout_height="100dp"
        layout="@layout/example"/>
```

### merge
merge 태그가 포함된 레이아웃이 다른 레이아웃에 추가될 때 merge 노드가 사라지고 자식뷰들만 새 부모 레이아웃에 직접 추가된다.
단일 루트노드를 생성하기 위한 불필요한 중첩을 제거한다.

include와 merge를 결합하여 깊게 중첩된 레이아웃 계층 구조를 만들지 않고 유연, 재사용 가능한 레이아웃을 정의하고 생성할 수 있다.

```xml
<!-- add1.xml -->
<?xml version="1.0" encoding="utf-8"?>
<merge xmlns:android="http://schemas.android.com/apk/res/android">
	
    <TextView
        android:text="add1"
        android:layout_width="match_parent"
        android:layout_height="wrap_content" />
</merge>

<!-- add2.xml -->
<?xml version="1.0" encoding="utf-8"?>
<merge xmlns:android="http://schemas.android.com/apk/res/android">

    <TextView
        android:text="add2"
        android:layout_width="match_parent"
        android:layout_height="wrap_content" />
</merge>

<!-- activity_main.xml -->
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <include
        android:id="@+id/add1"
        layout="@layout/add1"/>

    <include
        android:id="@+id/add2"
        layout="@layout/add2"/>

</LinearLayout>
```


### CustomView
1. CustomView의 클래스를 만든다.
2. attrs.xml 설정
	- custom하게 만들어줄 attribute를 설정한다.
	- res/values 밑에 attrs.xml파일을 만들고 아래와 같은 형식으로 정의한다.
	
	```xml
	<declare-styleable name="LoginButton">
        <attr name="bg" format="reference|integer" />
        <attr name="symbol" format="reference|integer" />
        <attr name="text" format="reference|string" />
        <attr name="textColor" format="reference|integer" />
	</declare-styleable>
	```
3. CustomView에서 attrs를 읽어오고 오버라이딩 메서드(onMeasure, onDraw 등)를 구현한다.

## 우지현

### 레이아웃 재사용

- 안드로이드에서는 다양한 위젯을 통해 재사용 가능한 작은 상호작용 요소를 제공하지만, 특수 레이아웃이 필요한 큰 구성요소를 재사용해야 할 수도 있다.
- 그럴 때는 `<include />` 및 `<merge />` 태그를 사용하여 다른 레이아웃을 현재 레이아웃 내에 삽입하면 된다.
- 레이아웃을 재사용하면 재사용 가능한 복잡한 레이아웃을 만들 수 있으므로 특히 유용하다.

### `<include>` 태그

1. 재사용해야 할 레이아웃을 새로운 xml 파일에 정의 (titlebar.xml)

   ```xml
       <FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
           xmlns:tools="http://schemas.android.com/tools"
           android:layout_width="match_parent"
           android:layout_height="wrap_content"
           android:background="@color/titlebar_bg"
           tools:showIn="@layout/activity_main" >
   
           <ImageView android:layout_width="wrap_content"
                      android:layout_height="wrap_content"
                      android:src="@drawable/gafricalogo" />
       </FrameLayout>
   ```

2. 다른 레이아웃 xml 파일에 해당 레이아웃을 `<include>` 태그로 작성

   ```xml
       <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
           android:orientation="vertical"
           android:layout_width="match_parent"
           android:layout_height="match_parent"
           android:background="@color/app_bg"
           android:gravity="center_horizontal">
   
           <include layout="@layout/titlebar"/>
   
           <TextView android:layout_width="match_parent"
                     android:layout_height="wrap_content"
                     android:text="@string/hello"
                     android:padding="10dp" />
   
           ...
   
       </LinearLayout>
   ```

포함된 레이아웃의 루트 뷰에 사용되는 모든 레이아웃 매개변수 (`android:layout_*` 속성)를 <include/> 태그에 지정하여 재정의할 수도 있다. 그렇지만 <include> 태그를 사용하여 레이아웃 속성을 재정의하려면 다른 레이아웃 속성이 적용되도록 `android:layout_height` 및 `android:layout_width`를 모두 재정의해야 한다.

```xml
    <include android:id="@+id/news_title"
             android:layout_width="match_parent"
             android:layout_height="match_parent"
             layout="@layout/title"/>
```

<include> 태그를 사용해서 레이아웃을 재사용할 경우, titlebar.xml의 ImageView만 들어가는 것이 아니라 ImageView를 감싸고 있는 FrameLayout까지 모두 들어가서 layout의 깊이가 깊어진다는 단점이 있다.

### `<merge>` 태그

<merge> 태그는 재사용될 레이아웃을 다른 화면에 포함시킬 때 필요 없는 레이아웃을 제거할 수 있도록 도와준다. <include> 태그를 사용하여 레이아웃을 재사용할 경우, 레이아웃의 깊이가 깊어진다는 단점이 있었다. 이러한 점을 해결하기 위해서는 <merge> 태그를 사용하면 된다.

```xml
    <merge xmlns:android="http://schemas.android.com/apk/res/android">

        <Button
            android:layout_width="fill_parent"
            android:layout_height="wrap_content"
            android:text="@string/add"/>

        <Button
            android:layout_width="fill_parent"
            android:layout_height="wrap_content"
            android:text="@string/delete"/>

    </merge>
```

이제 이 레이아웃을 다른 레이아웃에 <include> 태그로 포함하면 시스템에서 <merge> 요소를 무시하고 <include> 태그 대신 두 개의 버튼을 직접 레이아웃에 배치한다.

### Custom View

1. CustomView의 기본으로 쓰일 레이아웃 xml 파일을 생성한다.

2. attr.xml 파일에 custom하게 만들어줄 attribute를 설정해준다.

   ```xml
       <resources>
          <declare-styleable name="PieChart">
              <attr name="showText" format="boolean" />
              <attr name="labelPosition" format="enum">
                  <enum name="left" value="0"/>
                  <enum name="right" value="1"/>
              </attr>
          </declare-styleable>
       </resources>
   ```

3. CustomView를 코드적으로 구현하기 위한  클래스를 작성한다.

4. 다른 레이아웃에 해당 뷰를 추가한다.

<br>
	
## 이유진
### 다른 레이아웃을 현재 레이아웃 내에 삽입하는 방법
`<include/>` 및 `<merge/>`태그를 사용하여 다른 레이아웃을 현재 레이아웃 내에 삽입하면 된다.

#### <include> 태그 사용
복잡한 레이아웃을 여러 파일로 나눌 수 있고, 재사용이 가능하다.
```xml
<!-- reuse_item.xml -->
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
      xmlns:tools="http://schemas.android.com/tools"
      android:id="@+id/container"
      android:layout_width="match_parent"
      android:layout_height="wrap_content"
      android:orientation="vertical" >

    <TextView
      android:id="@+id/textView"
      android:text="text view" />

    <ImageView 
      android:id="@+id/imageView"
      android:layout_width="wrap_content"
       android:layout_height="wrap_content"
       android:src="@drawable/gafricalogo" />

</LinearLayout >
```

```xml
<!--main_activity.xml-->
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
      android:id="@+id/main_container"
      android:layout_width="match_parent"
      android:layout_height="match_parent"
      android:orientation="vertical" >

            <include layout="@layout/container"/>

</LinearLayout>
```
포함된 레이아웃의 루트 뷰에 사용되는 모든 레이아웃 매개변수(`android:layout_*`속성)를 `<include/>`태그에 지정하여 재정의 할 수 있다. 단, 다른 레이아웃 속성이 적용되도록 `android:layout_height`와 `android:layout_width`를 모두 재정의해줘야 한다.
```xml
<include android:id="@+id/news_title"
     android:layout_width="match_parent"
     android:layout_height="match_parent"
     layout="@layout/title"/>
```

reuse_item에 뷰가 하나일 때는 문제가 안 생기지만, 두 개 이상인 경우엔 레이아웃으로 감싸주게 되므로 레이아웃의 중복이 생길 수 있다. (위 예시에선 vertical LinearLayout이 중복됨) 이는 뷰 계층을 깊게해서 부하를 가중시킨다.
```xml
<LinearLayout>
   <TextView> <!-- include를 사용하지 않은 기존 뷰 -->
   <LinearLayout>
        <TextView>
        <ImageView>
   </LinearLayout>
</LinearLayout>
```

#### <merge> 태그 사용
<merge> tag는 <include>태그에서 발생하는 중복 문제를 처리하는 최상위 요소를 제공하는 더미 태그이다. <merge>를 사용하면, include되는 순간 merge태그가 사라지고 내용물만 바로 붙여진다.

merge를 사용해서 reuse_item.xml 을 다음과 같이 고칠 수 있다.
```xml
<!-- reuse_item.xml -->
<merge>

    <TextView
      android:id="@+id/textView"
      android:text="text view" />

    <ImageView 
      android:id="@+id/imageView"
      android:layout_width="wrap_content"
       android:layout_height="wrap_content"
       android:src="@drawable/gafricalogo" />

</merge>
```
이렇게 하면 다음과 같이 렌더링된다. (레이아웃 중복 해결)
```xml
<LinearLayout>
    <TextView> <!-- include를 사용하지 않은 기존 뷰 -->
    <TextView>
    <ImageView>
</LinearLayout>
```
단, merge는 뷰 그룹을 생성하지 않아서 include와 달리 **id사용이 제한적이다**. `merge`태그가 사라지면서 include할 때 id도 날아간다. 이에따라 붙여주는 include마다 구분이 사라져서 각 merge의 내부 뷰 역시 같은 id로 둘 수 없게 된다. 즉, `reuse_item.xml`의 내부 위젯에 접근할 수 없다. 따라서, merge는 한 xml에서 두 번 사용하지 않는 게 좋다.  

merge태그는 여러 activity에서 똑같이 생긴 긴 분량의 태그가 한 번 씩 사용될 때 사용하면 효과적이다.

### CustomView
요구사항에 맞는 view를 직접 만을 때 사용한다.

#### 커스텀뷰를 만드는 기본적인 원리
1. 기존에 존재하는 View클래스를 상속한다.
2. onDraw(), onMeasure(), onKeyDown()과 같이 시작하는 키워드가 'on'인 슈퍼 클래스 메서드를 오버라이드한다.
3. 새로 만든 커스텀 뷰를 사용한다. xml 레이아웃 등에 사용한다.

