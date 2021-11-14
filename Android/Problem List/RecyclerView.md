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
