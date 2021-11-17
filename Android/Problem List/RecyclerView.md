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