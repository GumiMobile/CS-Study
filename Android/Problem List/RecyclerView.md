# RecyclerView

## 김현수

### RecyclerView

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcppFDM%2FbtqFeNNBLAn%2FvmQ8iqjZ5GQ0zKgIsKE6gk%2Fimg.png"/>

- 목록을 화면에 출력해주고 동적으로 표현해주는 컨테이너 
- 리사이클러 뷰는 사용자가 아래로 스크롤 한다고 가정했을 때, 맨 위에 존재해서 이제 곧 사라질 뷰 객체를 삭제하지 않고 아래쪽에서 새로 나타날 뷰 위치로 객체를 이동시킨다. 즉 뷰 객체 자체는 재사용하고 뷰 객체가 담고 있는 데이터(채팅방 이름)는 새로 갱신한다.

### RecyclerView 주요 클래스

- Data

	데이터의 값을 저장하는 클래스

- LayoutManager

	아이템의 항목을 배치

- ViewHolder 

	데이터 값에 따라 변경되어 보여질 뷰

	Adapter에 의해 생성되며 미리 생성된 뷰홀더 객체가 있는 경우 새로 생성하지 않고 이미 만들어져있는 뷰홀더를 재사용한다. 이때 데이터가 뷰홀더의 아이템뷰에 바인딩 된다.

	내가 넣고자 하는 데이터를 실제 레이아웃과 연결시키는 기능이다.

	기존 리스트뷰를 이용했을 때는 ViewHolder 라는 개념이 없었고 뷰를 inflate해서 사용해야했다. 따라서 레이아웃의 요소를 가져오려면 하나씩 findViewById 를 해줘야한다는 단점이 있다.

	성능 저하 단점을 해결가능하게 한다.

- Adapter

	데이터와 아이템에 대한 뷰를 생성해주는 기능 → 데이터 관리, 뷰 객체 관리

	한 클래스의 인터페이스를 클라이언트에서 사용하고자 하는 다른 인터페이스로 변환한다.

	어댑터를 사용하면 인터페이스 호환성 문제 때문에 같이 쓸 수 없는 클래스들을 연결해서 쓸 수 있다.

	원본 데이터는 어댑터에 설정해야하며 어댑터가 데이터 관리 기능을 담당한다.

	아이템이 화면에 디스플레이 되기 전 → getView() 메서드가 호출된다.
	getView 메서드는 어댑터에서 가장 중요한 메서드로 이 메서드에서 반환하는 뷰가 하나의 아이템으로 디스플레이 된다.

	> onCreateViewHolder(ViewGroup parent, int viewType) : 뷰 홀더를 생성하고 붙여주는 부분
	> 
	> onBindViewHolder(ListItemViewHolder holder, int position): 재활용 되는 뷰가 호출하여 실행되는 메소드, 뷰 홀더를 전달하고 어댑터는 position의 데이터를 결합시킨다.
	> 
	> getItemCount() : 데이터의 개수를 반환한다.

### ListView 와 RecyclerView의 차이 

| |RecyclerView|ListView|
|---|---|---|
|ViewHolder|ViewHolder 패턴 이용|ViewHolder 패턴 이용할 필요 X|
|Item Layout|가로/세로/지그재그 모두 지원|세로만 지원|
|Item Animation|아이템 애니메이션 처리 클래스 O|아이템 애니메이션 처리 클래스 X|
|Adapter|데이터 제공을 위해 직접 구현|다양한 소스에 대한 어댑터 존재|
|Decoration|많은 구분선 설정|	쉽게 구분 가능|
|Click Event|개별 터치 이벤트 관리O, 클릭 처리 기능 X|클릭 이벤트에 바인딩하기 위한 인터페이스 존재|


> **보통 안드로이드 스튜디오 3.1 부터는 리사이클러뷰를 사용한다.**
>
> → 이전에는 리스트 모양으로 보여줄 때 리스트뷰를 사용했지만, 리싸이클러뷰가 더 많은 장점을 가지고 있기 때문에 리싸이클러뷰를 권장한다.
>
> → 기존의 ListView 는 커스터마이징 하기 힘들다.

## 김민수

### 리사이클러뷰

리사이클러뷰는 기존의 리스트뷰와는 다르게 `ViewHolder` 패턴을 <u>강제</u>한다. 따라서 리스트뷰에서 비용이 큰 `findViewById()` <u>함수 호출을 줄여</u> 성능을 개선한다. 흔히 리스트뷰와 비교하여 리사이클러뷰는 뷰를 재활용하여 성능을 올린다고 하는데, 이미 리스트뷰 어댑터의 `getView()`에서 `convertView`를 인자로 받아 뷰를 다시 inflating 할 필요가 없다.

```kotlin
// 리스트뷰에서 기본적으로 사용가능한 ArrayAdapter의 getView()

@Override
    public @NonNull View getView(int position, @Nullable View convertView,
            @NonNull ViewGroup parent) {
        return createViewFromResource(mInflater, position, convertView, parent, mResource);
    }

    private @NonNull View createViewFromResource(@NonNull LayoutInflater inflater, int position,
            @Nullable View convertView, @NonNull ViewGroup parent, int resource) {
        final View view;
        final TextView text;

        if (convertView == null) { // convertView null 체크
            view = inflater.inflate(resource, parent, false);
        } else {
            view = convertView;
        }
        // ......
```



> findViewById()가 비용이 큰 이유
>
> findViewById()는 View.findViewTraversal()를 거쳐 각 뷰의 id 값을 체크하여 리턴한다. 문제는 ViewGroup에서 findViewTraversal()이다. ViewGroup에서는 자신의 하위 뷰들에 대해 각각 findViewById()를 호출한다. 따라서 뷰계층이 깊어진다면, findViewById()는 재귀적으로 호출되어 성능 하락을 일으킨다.

### 뷰홀더 패턴

각 뷰 객체를 뷰홀더에 보관함으로써 findViewById()의 반복 호출을 줄인다. findViewById()로 찾은 뷰 객체와 데이터를 연결하고, 이를 저장한다. 따라서 한번 찾은 뷰 객체는 다시 찾지 않는다. 

리스트뷰 또한 뷰홀더 패턴을 사용할 수 있으며, 이를 통해 성능 개선이 가능하다.

### 리사이클러뷰 어댑터, 뷰홀더

``` kotlin
class MyViewHolder(itemView: View): RecyclerView.ViewHolder(view) {
  val textView = itemView.findViewById<TextView>(R.id....) // 뷰홀더에서 데이터와 바인딩될 뷰 객체에 접근하여 멤버로 갖고 있다.
}

class MyAdapter(private var datas: Array<String>): RecyclerView.Adapter<MyViewHolder> {
  override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): MyViewHolder { // 뷰홀더를 생성할 때, 아이템뷰를 inflate하고 뷰홀더의 매개변수로 넣어 리턴한다.
        val view = LayoutInflater.from(parent.context).inflate(R.layout.item, parent, false)
        Log.d("tag1" , "onCreateViewHolder")
        return MyViewHolder(view)
    }

    override fun getItemCount(): Int {
        return datas.size
    }

    override fun onBindViewHolder(holder: MyViewHolder, position: Int) { // 뷰홀더 생성시 할당한 뷰 객체와 데이터만 연결시킨다.
        holder.textField.text = datas[position]
    }
}
```

리사이클러뷰를 사용할 때는 리스트뷰와 같이 어댑터를 사용한다.  `RecyclerView.Adapter`를 상속받아 구현하고 `onCreateViewHolder()` `onBindViewHolder()` `getItemCount()`등 3개의 메소드는 반드시 오버라이딩 해야한다. `onCreateViewHolder()` `onBindViewHolder()`함수로 인해 리사이클러뷰는 뷰홀더 패턴을 <u>반드시 사용</u>해야 한다.

### 리사이클러뷰 vs 리스트뷰

|               | 리사이클러뷰                                      | 리스트뷰                                 |
| ------------- | ------------------------------------------------- | ---------------------------------------- |
| 뷰홀더        | 강제                                              | 권장                                     |
| ItemLayout    | 가로, 세로, 그리드                                | 세로                                     |
| ItemAnimation | O                                                 | X                                        |
| Adapter       | RecyclerView.Adapter<ViewHolder> 를 상속받아 구현 | 사용가능한 여러 어댑터가 이미 존재함     |
| Click Event   | 클릭처리 인터페이스를 직접 구현                   | 클릭 이벤트를 바인딩하는 인터페이스 존재 |


## 이수형

### 리사이클러뷰

기존의 ListView의 커스터마이징이 힘들었고 성능의 문제도 있어서 롤리팝버전 때 새로 추가됨

LayoutManager와 ViewHolder 패턴의 의무적인 사용, Item에 대한 뷰의 변형이나 애니메이션할 수 있는 개념이 추가

### 주요 클래스

#### Adatper

데이터와 아이템에 대한 View를 생성<br/>
다음의 3가지 인터페이스를 구현해야 한다.

- onCreateViewHolder() : 뷰 홀더를 생성하고 뷰를 붙여주는 부분
- onBindViewHolder() : 재활용 되는 뷰가 호출하여 실행되는 메소드, 뷰 홀더를 전달하고 어댑터는 position의 데이터를 결합시킨다.
- getItemCount() : 데이터의 개수 반환

getItemCount() -> onCreateViewHolder() -> onBindViewHolder() 순으로 호출

#### ViewHolder

UI를 수정할때마다 부르는 findViewById를 한번만 호출함으로써 무거운 연산을 줄여주어 앱의 성능을 올리도록 강제

#### LayoutManager

리싸이클러뷰는 다양한 타입의 리스트를 지원하고 커스텀이 가능하다

- LinearLayoutManager
   - Vertical(가로) / Horizontal(세로) 형태로 아이템을 배치한다.

- GridLayoutManager
   - 한 줄에 1개 이상의 이미지를 표시할 수 있지만 아이템의 크기는 줄의 첫 번째 아이템의 크기에 따라서 달라질 수 있다.

- StaggeredGridLayoutManager
   - 그리드 형태의 아이템에 크기를 다양하게 적용할 수 있다.

- Custom LayoutManager
   - 3개의 레이아웃 매니저를 상속받아 구현할 수 있다.

#### Click Detection

리사이클러 뷰 자체에는 Click에 대한 이벤트 처리를 자체적으로 할수 없어서 onClickListener를 사용한다


### 리스트뷰와의 차이

- ViewHodler 패턴이 강제된다
- 모든 LayoutManager를 지원한다
- 데이터를 제공하기 위한 Custom Adapter구현이 필요하다
- Click에 대한 처리를 리스너를 사용해 구현해야한다

## 우지현

### RecyclerView

<img src="https://wooooooak.github.io/public/img/android/recycler1.png" alt="RecyclerView의 재사용성" width="500" />

- 기존의 ListView는 커스터마이징하기 힘들었고, 구조적인 문제로 성능상의 문제도 있었다.
- RecyclerView는 ListView의 문제를 해결하기 위해 개발자에게 더 다양한 형태로 커스터마이징 할 수 있도록 제공되었다.
- RecyclerView와 ListView의 가장 큰 차이점은 LayoutManager와 ViewHolder 패턴의 의무적인 사용, Item에 대한 뷰의 변형이나 애니메이션 개념이 추가된 것이다.
- RecyclerView는 재사용이 가능한 뷰이다. 
  - 사용자가 아래로 스크롤한다고 가정했을 때, 맨 위에 존재해서 이제 곧 사라질 뷰 객체를 삭제하지 않고 아래쪽에 새로 나타날 뷰 위치로 이동시킨다.
  - 즉, 뷰 객체 자체를 재사용하는 것인데, 중요한 점은 뷰 객체를 재사용할 뿐이지 뷰 객체가 담고 있는 데이터는 새로 갱신된다.

### 주요 클래스

#### Adapter

RecyclerView는 universal한 adapter를 사용하여 데이터 소스를 처리한다. 이것은 RecyclerView의 유연성을 보여준다. 다음의 3가지 인터페이스를 구현해야 한다.

- onCreateViewHolder(ViewGroup parent, int viewType) 
  - 뷰 홀더를 생성하고 뷰를 붙여주는 부분
  - 새롭게 싱성될 때만 호출된다.
- onBindViewHolder(CustomViewHolder holder, int position) 
  - 재사용되는 뷰가 호출하여 실행되는 메소드
  - 뷰 홀더를 전달하고 어댑터는 position의 데이터를 결합시킨다.
- getItemCount()
  - 데이터의 개수 반환

getItemCount() -> onCreateViewHolder() -> onBindViewHolder() 순으로 호출된다.

#### ViewHolder

리스트뷰에서는 뷰홀더 패턴을 권장했다. UI를 수정할 때마다 부르는 findViewById()를 뷰홀더 패턴을 이용해 한 번만 호출함으로써 리스트뷰의 지연을 초래하는 무거운 연산을 줄여주었다. 이 문제를 리사이클러뷰에서는 뷰홀더 패턴을 항상 사용하도록 강제함으로써 해결했다.

하지만 실제로 앱의 퍼포먼스를 향상시켜주지만 최신의 디바이스에서 뷰홀더 패턴을 사용하지 않은 리스트뷰나 리사이클러뷰의 성능 차이는 미세하다.

단지 차이점은 리사이클러뷰는 뷰홀더 패턴이 강제된다는 것이다. 이전의 리스트뷰는 선택적이었지만 성능 차이가 너무 컸기 때문에 변화된 것으로 생각된다. 간과하기 쉬운 점은 뷰홀더 패턴을 사용한 리스트뷰와 리사이클러뷰의 성능은 같다는 것이다.

#### LayoutManager

- LinearLayoutManager
  - Vertical(세로) / Horizontal(가로) 형태로 아이템 배치
- GridLayoutManager
  - 한 줄에 1개 이상의 이미지를 표시할 수 있지만 아이템의 크기는 줄의 첫 번째 아이템의 크기에 따라서 달라질 수 있다. (고정시에는 모두 고정)
- StaggeredGridLayoutManager
  - 그리드 형태의 아이템에 크기를 다양하게 적용할 수 있다.
- Custom LayoutManager
  - 3개의 레이아웃 매니저를 상속받아 구현할 수 있다.

#### ItemDecoration

RecyclerView.ItemDecoration 클래스를 통해 divider를 원하는 아이템에 추가할 수 있게 되었다. 조금 복잡하지만 동적인 데코레이팅이 가능해졌다.

#### ItemAnimation

RecyclerView.ItemAnimator 클래스를 통해 애니메이션을 핸들링할 수 있게 되었다. 이 클래스를 통해서 아이템의 삽입, 삭제, 이동에 대한 커스터마이징이 가능하고, 또한 DefaultItemAnimator가 제공되므로 커스터마이징 없이 사용할 수도 있다.

notifyItemChanged(), notifyItemInserted(), notifyItemRemoved()를 통해 특정 아이템에 대한 애니메이션을 발생시킬 수 있다.

#### Click Detection

RecyclerView.OnItemTouchListener는 리사이클러뷰의 터치 이벤트를 감지한다. 좀 복잡하지만 개발자에게 터치 이벤트를 인터셉트하는 제어권한을 주게 되었다. 

안드로이드 공식 문서에서는 터치 이벤트를 인터셉트함으로써 리사이클러뷰에게 전달하기 전에 조작하여 유용하게 사용될 수 있다고 한다. 리스트뷰에서 아이템 클릭 시 콜백 받을 수 있는 리스너는 리사이클러뷰에 존재하지 않는다. 즉, 리사이클러뷰에는 클릭 이벤트에 대한 처리를 자체적으로 할 수 없기 때문에 onClickListener를 사용한다.

#### DiffUtil

### RecyclerView vs ListView

|                 | <center>RecyclerView</center>                                | <center>ListView</center>                                    |
| :-------------: | ------------------------------------------------------------ | ------------------------------------------------------------ |
|   ViewHolder    | ViewHolder 패턴을 이용한다.                                  | ViewHolder 패턴을 이용할 필요가 없다.                        |
|   Item Layout   | 가로, 세로, 그리드 모두 지원한다.                            | 세로 방향만 지원한다.                                        |
| Item Animation  | 아이템 애니메이션을 처리하는 클래스가 있다.                  | 아이템이 추가/제거될 때에 적용할 수 있는 애니메이션이 없다.  |
|     Adapter     | 데이터를 제공하기 위한 사용자 정의 구현이 필요하다.          | 배열/DB 결과에 대한 ArrayAdapter/CursorAdapter와 같은 다양한 소스에 대한 어댑터가 존재한다. |
|   Decoration    | RecyclerView.ItemDecoration 객체를 사용하여 더 많은 구분선을 설정할 수 있다. | android:divider 속성을 이용하여 리스트에 있는 아이템들을 쉽게 구분할 수 있다. |
| Click Detection | 개별 터치 이벤트를 관리하지만 클릭 처리 기능이 내장되어 있지 않다. | 목록의 개별 항목에 대한 클릭 이벤트에 바인딩하기 위한 AdapterView.OnItemClickListener 인터페이스가 있다. |

