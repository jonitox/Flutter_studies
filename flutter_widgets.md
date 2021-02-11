## Widgets   

- *MaterialApp 위젯*    
app을 Material theme으로 Setup하는 widget, named aurgments를 받아 인스턴스화. 여러 Theme을 지정 가능.     
이 위젯 트리 내의 위젯들은 해당 theme을 따름.      
title:    
theme: ThemeData / app의 theme을 설정. Theme정보를 담은 클래스.    
(각 위젯들에서 app의 theme을 적용할때 Theme.of(context).(..)로 참조해서 쉽게 main Theme을 사용.)      
home: Widget / app의 첫화면으로 띄어질 스크린(위젯) 지정.    
routes: Map<String,WidgetBuilder> // 라우팅할 페이지들의 목록및 builder를 지정된 name으로 참조할수있도록 목록 생성.     
initialRoute: String // app의 첫화면으로 띄어질 스크린을 라우팅name으로 지정.(home과 동일, default는 '/'.)             
(WidgetBuilder는 (BuildContext)=>Widget 형태의 위젯 생성함수.) (Naviagor의 pushNamed에서 이름으로 참조될때 사용.)         
ex) routes: {'/': (ctx)=>CategoriesScreen(_availableMeals), '/category-meals': (ctx) => CategoryMealsScreen(),  },    
(모든 name을 사용자지정. 단, '/'는 initialRoute에서 default로 사용되는 첫화면의 위젯의 이름.)       
(tip: 모든 이름은 '/../'처럼 web의 convention을  자주 사용. 이때, 해당 이름들을 호출시 미세한 오타방지를 위해,     
각 라우팅의 이름을 해당 위젯 클래스 내에 static const로 선언하여, 클래스에 직접 접근해 이름을 참조해 호출 가능.     
ex) 페이지 위젯 내에, static const routeName = '/category-meals'; 선언 후, (모든 라우팅 이름을 클래스의 routeName으로 참조)      
route: {CategoryMealsScreen.routeName : (ctx)=>CategoryMealsScreen(_availableMeals),},로 테이블 작성, pushNamed(CategoryMealsScreen.routeName), 로 페이지 생성.)   
(또한, 위와 같이 main.dart의 변수(_availableMeals)를 route테이블에서 라우팅할 위젯의 생성자에 전달 가능. 즉, main에서 state관리를 하는경우, 다른곳에서 라우팅을하더라도 main의 변수를 route테이블을 통해 전달가능.)        
onGenerateRoute: (RouteSettings) =>Route // app내에서 pushNamed로 라우팅 시도 시, route테이블에 없는 이름으로 라우팅하는 경우 실행되는 네비게이션 액션 명시.     
(해당 라우팅 시도의 setting을 인자로 전달. 실행할 라우팅객체를 반환. 라우팅테이블이 app사용 동안 dynamic하게 변경 및 결정되는 경우 등에 사용 가능.)     
onUnknownRoute: (RouteSettings)=>Route // 모든 다른 알수 없는 라우팅에 대해 실행할 라우팅 명시.            
(invalid한 라우팅이거나 pushNamed로 테이블에 없는 이름으로 라우팅하는데, onGenerateRoute가 명시되있지않는 경우 등.)       
(일반적으로, web의 존재하지 않는페이지 접속 등의 fallback을 처리하는 것과 비슷한 역할. 에러페이지를 보여주거나, 초기화면으로 돌려줄수있음.)          
( ex) onUnknownRoute: (settings) { return MaterialPageRoute( builder: (ctx) => CategoryMealsScreen(),  ); }, )    


- *CupertinoApp*     
app을 Cupertino Theme으로 set up     
theme: CupertinoThemeData /

- *Scaffold*   
material style의 페이지 Setup(스타일링)을 도와주는 스크린 위젯, 배경 색 등 지정 가능.      
appBar: preferredSizedWidget(ex)Appbar(...)) (화면 상단의 appBar위젯 지정)     
body : Widget (appBar밑의 화면의 body부분에 표현될 위젯)       
floatingActionButtion : Widget(일반적으로, floatingActionButton)(body를 덮어 표시될 button, 버튼의 위치 default는 우측 하단)    
floatingActionButtonLocation: FloatingActionButtonLocation(상기 버튼의 위치 지정. floating버튼의 위치를 나타내는 객체.)    
bottomNavigationBar: BottomNavigationBar // 화면 하단에 표시될 NavigationBar(TabBar)지정. ((하단 tab스타일 구성시 사용.)    
drawer: Widget // 화면 왼쪽(or오른쪽)에 숨겨져있다가 표시될 위젯(일반적으로, Drawer)명시. Drawer사용시, appBar에 drawer(햄버거)버튼 생성.     

- *CupertinoPageScaffold*
cupertino style 페이지 Setup 위젯   
child: Widget / scaffold의 body와 동일.   
navigationBar: preferredSizedWidget(ex)CupertinoNavigationBar) /  scaffold의 appbar와 동일     
(아이폰의 상단에 notch가 있는(device status가 양옆에 표시.) 모델 등의 경우, navigationBar는 os UI를 respect해 자동적으로 밑에       
rendering되지만 child는 해당 부분까지 available한 영역으로 인식해 Bar와 겹쳐서 rendering됨. body부분의 위젯을 safeArea()로 씌워 해결.)    

- *AppBar*   
scaffold의 AppBar로 지정할수 있는 material 위젯. 혹은 drawer같은 위젯의 상단(column의 첫째 자식) 등에도 사용 가능.   
title: Widget(일반적으로, Text()) (AppBar에 표시될 타이틀 지정)   
actions: List<Widget> (title옆에 표시될 widget지정. 일반적으로, iconButtons을 사용. 혹은 popUpMenuButton)   
(Appbar.)preferredSize / appBar의 크기를 저장해놓은 객체. (preferredSize.)height으로 appBar의 높이 참고가능.
bottom: PreferredSizedWidget // AppBar의 title밑 하단부분에 들어갈 위젯 명시. 일반적으로, tabBar(안드로이드 스타일의 탭 구성시)     
automaticallyImplyLeading: bool // leading에 표시되는 위젯이 null일때 표시할지 여부. default는 true(leading이 없어도 자동으로 상황에맞는위젯 생성.)     
(leading이 있는 경우는 효력x. 일반적으로, 돌아가는 페이지나 draw등이 없을때 작동안하는 버튼을 없앨 때 fale지정)      

- *CupertinoNavigationBar*   
cupertinoScaffold의 navigationBar로 지정할 수 있는 위젯.    
middle: Widget / AppBar의 title과 유사.   


- *Column/Row*   
여러 위젯을 열/행으로 묶어 배치를 도와주는 위젯. Column(Row) 위젯의 좌우너비(높이)는 child의 너비(높이)중 가장 큰 값을 따름.    
column(Row)의 maxHeight(maxWidth)은 default로 double.infinity(가능한 최대). minHeight은 자식위젯의 높이합으로 설정됨.    
Column/Row는 겹치거나(row안의 row) mix해서 사용가능.      
부모column내의 여러 child와 같이 있는 자식column을 설정할 경우 높이는 자식column내의 children 높이와 fit하게 설정됨.?   
Column(Row)의 mainAxis는 top to bottom(left to right), crossAxis는 left to right(top to bottom)    
children:: list<widgets> (child위젯 입력)    
MainAxisAlignment: MainAxisAlignment, (column의 배정된 UI내 각child의 mainAxis상 배치형태를 결정하는 enum, default는 start)   
(MainAxisAlignment./  center:중앙, end: 밑, spaceAround :각 child위아래로 일정공간 추가)   
crossAxisAlignment: CrossAxisAlignment, (각 child의 corssAxis상에서의 배치형태를 결정하는 enum, default는 center)    
(CrossAxisAlignment./ end: 오른쪽끝, stretch: column의 너비만큼 늘여서 채워 배치.(child가 card등이면 너비를 define해줄수있음.))   
mainAxisSize:  MainAxisSize / mainAxis의 Size 지정. MainAxisSize는 enum으로 max(double.infinity)와 min(children에 fit) 존재.     
 

 - *RaisedButton위젯*   
버튼을 생성하는 위젯
child : Widget 버튼내부에 표현되는 위젯(text, image등) 입력   
onPressed 인자: 버튼 터치시 수행하는 함수(_void_)의 포인터 입력 null이면 button이 enabled.   
(RaisedButon.)icon() / icon과 label로 이루어진 버튼을 생성하는 named생성자. (named argument로는    
onPressed: (){}, icon: Icon(표시될 icon위젯), label: Widget(표시될 label, 일반적으로 text), color:, textColor: 등       
 
 - *FlatButton위젯*      
배경이 없는 버튼. 나머지는 동일.    
(FlatButton.)icon() / raisedButton의 icon생성자와 동일. icon+label형태 버튼 생성.    

- *CupertinoButton*    
cupertino theme의 버튼. default는 flatButton형태.     
color: Colors / background color 지정. 지정해줄시 raisedButton형태로 표현 가능.     

- *IconButton위젯*   
icon모양의 버튼.    
icon : Widget(버튼에 들어갈 icon모양 지정. Icon Widget을 받음.)   
onPressed : (){} (Listner, 다른 버튼과 동일)    

- *floatingActionButton*   
scafold의 floatingActionBUtton에 optimized된 버튼. body를 덮는 버튼.   
child: Widget(버튼이 감쌀 위젯. 일반적으로 Icon)   
onPressed: (){} (Listner, 다른 버튼과 동일)    

- *Icon/Icons*    
Icon은 Icon을 나타낼수 있는 위젯. positional 인자로 IconData를 받음.       
Icons는 여러 icon(material style)의 IconData를 static으로 지정해놓은 class. cupertino모양은 CupertinoIcons에 존재.     
즉, Icon(Icons.add)처럼 생성. 다른 argument로 색깔,사이즈 등을 지정가능, default값은 선택된 아이콘의 theme를 따라 결정.    

- *Text위젯*   
positional arguments로 String을 받음. named arguments로 여러 인자를 받음.      
style : TextStyle(fontSize: 28, fontWeight: Fontweight.bold) (문자의 style결정)   
(null시 app의 theme을 따름. 단,Text는 material위젯으로 cupertinoApp내에서 Text사용시 theme을 불러올수 없음.)    
(TextStyle은 문자Style의정보들을 담은 class(위젯x), Fontweight은 Font크기 값을 표현하고 static값을 묶어둔 utility class)    
textAlign: TextAlign.center (문자열의 배치결정. 단, text위젯이 차지하는 공간기준.) (TextAlign은 enum. center,left,right등 포함) //   
(tip: Text위젯은 UI에서 text크기만큼 공간을 할당받음. 즉, TextAlign.center로 화면 가운데 배치하고싶다면 Text를 Container에 담고 UI공간을 설정해 사용.)    
Text의 theme의 default는 현재 부모페이지의 theme을 따름. 즉, mateiral App 내에서, cupertinoScaffold를 선언해 Text를 추가하면,    
부모인 cupertino Theme을 따르므로,  mateirial App내에 설정해둔 theme을 가져오지 못함. 이런 경우, page를 CupertinoApp에 선언하거나,    
(CupertinoApp의 Theme목록은 mateirial 보다 제한적이므로) material App에 선언하되, text에 직접 style을 Theme.of(context)로 지정.       
softWrap: bool // text가 지정된 라인(가로길이)을 벗어나면, 줄바꿈을할지 여부.    
overflow: TextOverFlow // text가 할당된 공간보다 길어질때 overflow를 어떻게 처리할지 결정.    
(TextOverFlow는 enum으로 ellipsis(...표기), clip(자름), fade(끝부분 fade처리) 등 존재)    


- *TextField*    
user로부터 text input을 String으로 입력 받을 수 있는 위젯. 모든 입력은 String. 입력을 숫자로 변경할시 수동변경 필요.      
decoration: InputDecoration (textfield의 꾸밈효과 지정. InputDecoration은 꾸밈 정보를 포함한 객체)     
( InputDecoration의 arguments에는  labelText: String (field위에 어떤칸인지 정보(String)표시?)  )    
field의 input을 저장하는 두가지 방법: Listener(ex)onChanged) 사용(logic이 complex할때 추천??) / controller 사용     
Lister를 사용하여 함수를 호출해 직접 별도의 변수(String)에 입력값을 저장. / Controller를 연결해 TextEditingController에 자동 저장.      
onChanged: (String){} (모든 keyStroke마다 함수 호출. 현재 field에 있는 String을 인자로 받음.)    
onEditingComplete/onSubmitted/onTap:     
controller : TextEditingController(-> keyStroke마다 Field의 입력을 저장해두는 객체. 객체 내 변수 .text로 저장된 String접근 가능)     
(final myController = TextEditingController()처럼 controller객체를 생성해두고 사용!)    
keyboardType: TextInputType (field를 선택할시 나오는 soft keyboard의 종류 명시. TextInputType은 static값 선언된 class)
(IOS에서 소수점을 포함한 숫자키보드: TextInputType.numberWithOptions(decimal:true))    

- *CupertinoTextField*   
placeHolder : / label Text지정    

- *Container위젯*   
위젯을 담아 UI에 표현시 보이지않는 공간(Layout)관리 및 꾸밈효과를 돕는 위젯. named arguments를 받아 생성.   
child: Widget (감쌀 위젯),   
width: double (UI공간에서 할당받을 너비) (tip: double.infinity로 화면상 가능한 최대너비를 할당가능, 다른 UI가 안겹칠때까지),   
margin: EdgeInsetGeometry (boarder바깥쪽인 margin 설정)   
(> EdgeInset: 위젯의 margin길이 정보를 표현한 class. 여러 named constructor로 생성 가능. ex) EdgeInset.all(20) :모든방향 20)   
color: Color / 단순 background color지정. 좀더 디테일한 설정 필요시, decoration인자 사용.(decoration의 color와 동일, decoration이 있다면 에러발생.)         
decoration: Decoration (일반적으로, 상속하는 BoxDecoration을 객체로 입력/boarder나 gardient등 위젯을 꾸미는 정보를 담은 클래스)    
Padding: EdgeInsetGeometry(boarder안쪽인 padding 설정)     
alignment: AlignmentGeometry / container내 child의 배치를 지정.    

-*Stack위젯*   
여러 위젯을 서로 위아래로 덮어서(3차원상에서) 표현할수 있게하는 위젯. Stack의 크기는 가장 큰 child의 크기와 동일.         
children: [] / 포갤 위젯목록 지정. 첫번째 원소가 가장 아래에 배치.   

- "Positioned위젯"    
Stack내에서만 사용할 수 있는 위젯으로, 감싼 위젯이 전체 Stack 공간 내에서 특정 위치에 배치될수 있게 돕는 위젯.     
top(bottom,right,left): double // 전체 stack 기준으로 위(아래,오른쪽,왼쪽)로부터 얼만큼 떨어져있는지 지정.     

- *Card위젯*   
위젯을 담아 shadow를 주어 배치하는 content container위젯. UI공간은 default로 child크기만큼 할당.   
card의 부모 위젯이 child(card)의 UI공간을 define한다면 부모 위젯을 따름.   
즉, parent로 container같은 위젯을 써 할당하거나, child의 UI크기를 변경.      
child : Widget(감쌀위젯)   
elevation: double (shadow의 세기 조절)     
color: Color(background color지정)   
margin: EdgeInsetsGeometry (위젯 margin지정)   
shape: ShapeBorder // Card의 shape지정. ShapeBorder는 Border의 모양을 지정할 수 있는 객체. 예를들어, 모서리가 둥근 사각형은 RoundedRactangleBorder객체.    


- *Center위젯*   
자식위젯을 자신이 차지하는 공간 가운데에 배치하는 layout위젯. named arguments를 받아 생성.   
child : Widget (감쌀 위젯)   
heightFactor : double (자식위젯의 높이에 대한 위젯의 높이 비율, null일 경우 화면에서 차지할수있는 만큼할당)   
widthFactor : double (자식위젯의 너비에 대한 위젯의 너비 비율, null일 경우 화면에서 차질할수있을만큼 할당)    
margin : EdgeInsetsGeometry (card의 margin.)   
(tip : card에 padding 설정 불가능. padding을 주고싶으면 car의 child를 container로 wrap하여 margin or padding입력)    

- *SingleChildScrollView위젯*   
위젯의 크기가 해당 위젯이 표현될수있는 할당된 UI범위를 벗어나도라도 scrollable하게 표현할수 있게 도와주는 위젯.   
child: Widgetd (위젯 하나를 감싸, 그 위젯이 기존 표현가능(할당된 UI) 범위에서 넘어가더라도 scrollable하게 해줌.)     
ex) column을 특정 크기 내에서 scrollable하게 하려면 : Container( height:300, child: SingleChildScrollView(child: Column(..)), ),   

- *ListView위젯*   
scrollable한 위젯list.   
ListView의 UI크기는 default로 infinity. 즉, container와 같은 부모위젯으로 감싸 크기를 지정해 UI공간을 할당 받아야 함.    
사용하는 2가지 방법: children : []으로 arguments전달 / named생성자인 (ListView.)builder() 사용   
children전달하면, column + scrollView처럼 작동. 전부 widget을 생성해 column을 만들고 scrollable하게 표시.    
builder사용시 UI에 표시되는 위젯만 rendering. (lazily-rendering.) long List일때 더 좋은 performance(memory)   
scrollaDirection: Axis(column/row 지정(scroll방향)(default: vertical =column))   
itemBuilder: (BuildContext, int(index번호))->Widget (ListView의 새로운 아이템을 생성해야할때마다 호출하는 위젯 생성 함수 명시.)   
(자동적으로 buildcontext(메타정보)와 int(index,몇번째 아이템인지)를 선언한 함수에 인자로 전달해줌.)   
(함수에선 아이템으로 표시할 위젯을 반환하도록 작성. index를 이용해 각종 정보에 접근하여 생성가능.)      
itemCount: int (현재 ListView에 포함할 아이템의 갯수 명시.)   

- *ListTile*   
List Tile(네모난 카드 모양)형태로 표현할수 있는 위젯. ListView내의 한 원소를 표현할떄 자주 쓰임.(필수 x, 따로도 사용 가능)   
leading: Widget / title왼쪽에 표시할 위젯(보통 이미지나 가격, circleAvatar등)    
title: Widget / List Tile의 주된 정보를 나타내는 위젯. 주로 Text    
subtitle: Widget / 추가적인 정보를 나타내는 위젯. title밑에 표시. 주로 Text
trailing: Widget / title 오른쪽에 표시할 위젯 (보통 휴지통, 수정 같은 버튼)(아이콘 어러개등을 포함한 row도 가능. 단, row에 컨테이너등으로 width 지정해서.      
(trailing의 사이즈는 undefined.)     

- *SwitchListTile*     
ListTile형태의 switch 위젯. ListView내의 원소로 switch를 표현하고자할때 자주 쓰임. (app의 설정, 필터 등을 구현 할때)     
named argument는 일반적은 switch와 비슷.    
value: bool // switch에 표시될 bool값. (on,off)     
onChanged : (newvalue){} // switch가 toggle될때 실행될 함수.    
title:  Widget // 스위치 타일에 표시할 주된 내용. 일반적으로 Text    
subTitle: Widget // title밑에 표시될 부가적인 내용. 일반적으로 Text
 
- *GridView*   
scrollable + grid. grid형으로 위젯 배치. 마찬가지로 두가지 방식으로 생성가능. children명시 or 빌더 사용.   
children: [],   // gridveiw에 포함되는 자식위젯 명시. 
GridView.builder() // builder로 gridview생성.    
gridDelegate: SliverGridDelegate // grid의 child간의 layout을 지정하는 deligate설정, SliverGridDelegate은 클래스.        
SliverGridDelegateWithMaxCrossAxisExtent(  // 그리드의 가로폭을 지정해 설정하는 deligate을 나타내는 SliverGridDelegate 객체.     
maxCrossAxisExtent : double, // 각 타일의 최대가로폭 지정. 가로폭이 이 값을 안넘게끔 한 row에 타일이 최대로들어감.    
childAspectRatio : double, // 각 child위젯의 세로/가로 비율 지정.    
crossAxisSpacing : double, // 인접한 child간 가로 거리.   
mainAxisSpacing : double, // 인접한 child간 세로 거리. )      
SliverGridDelegateWithFixedCrossAxisCount( // 그리드의 컬럼 개수를 지정하여 그리드를 생성하는 객체.     
crossAxisCount: int // 컬럼개수 지정. 각 타일의 가로 너비는 디바이스에 따라 그리드의 컬럼수가 이값으로 맞춰지게끔 자동으로 정해짐.)      

- *GridTile*    
GirldView에 들어가기에 적합한 모양의 그리드 타일 형태의 위젯.(필수x, 따로 사용 가능)     
child: Widget // 그리드 타일의 main content로 표현될 위젯.      
footer: widget // 그리드 타일의 아래부분에 표시되는 위젯. 일반적으로, GridTileBar    

- *GridTileBar*    
GirdTile의 footer에 일반적으로 사용되는 위젯. 그리드 타일의 바 형태.    
leading : Widget     
title: Widget     
subtitle: Widget
trailing: Widget     

- *GestureDetector*    
감싼 위젯에 발생하는 User input(tap, double tap, ..)을 제어하기 위한 위젯.       
behavior: HitTestBehavior (특정 동작을 제어하기위해 필수?)
onTap: (){}
child: Widget / 감쌀 위젯   
tip: GestureDetector를 사용하여 custom Button 위젯 생성 가능. child를 버튼모양, onTap을 listner로 구성     

- *InkWell*   
GestureDetector + riffle effect
onTap: (){} //
child: // 감쌀위젯./child가 단순container면 물결생성이 안에도 보임. card같은위젯이면 card밖(margin있는곳)만 보임.    
splashColor: Color // 물결의 색깔   
borderRadius: BorderRadiusGeometry // 만약 감싼 위젯에 borderRadius가 있다면, 값을 똑같이 지정해주면 같은 형태의 물결 생성.       


- *dedicated padding()*

- *Image*     
Image파일을 띄어주는 위젯. Image의 출처에 따라 여러 named constructor로 생성.   
(Image.)asset('파일경로 및 이름', height:200, width:double.infinity,fit: BoxFit.cover) / pubspec.yaml에 명시된 asset 내의 파일인 경우.   
(Image.)network(String, fit: ,fit: BoxFit.cover) / 웹상의 이미지를 가져올 경우. positional 인자로 Url(String)입력      
(Image.)file() / file에서 stream을 가져오는 경우?    
각 constructor내의 name argument   
fit: BoxFit / 이미지가 Box내에서 size를 어떻게 fit할지 지정. BoxFit은 여러 값을 지정한 enum.   
(BoxFit.cover : 이미지를 비율유지하면서 확대해서 Box를 채움. 이미지가 짤릴수 있음.)      
(image를 포함하는 Box의 크기는 image에 직접 지정된 height,width거나 미지정시 크기가 define된 부모위젯)         


- *sizedBox*   
특정 크기의 layout을 줄때 사용하는 위젯. child를 가질수 있지만(container처럼 작동), child없이 크기를 줘서 공백을 줄때 자주 사용.    
height: double    

- *FractionallySizedBox*   
부모위젯의 크기에 대한 비율로 전체 위젯의 크기를 지정해줄때 사용하는 위젯.    
차트바 등을 표현할때 (외부패키지를 사용하지 않을때) Stack과 함께 사용하면 유용.         
child: Widget / Box가 감쌀 위젯. 이 위젯의 크기를 비율에 맞게 생성.   
heightFactor: 0~1 / 부모위젯의 크기에 대한 비율 지정. 1이면 부모위젯 전체공간 차지.   

- *FittedBox*    
감쌀 위젯의 크기가 해당 위젯이 차지 가능한 영역을 벗어나지않도록 강제할때(shrink) 사용하는 위젯.   
ex) Container 내의 Text를 감쌀 경우, 문자열 길이가 Container 밖으로 넘어가면 자동으로 Text를 shrink(높이,너비 동시에)하여 한줄로 맞춰줌.       
chld: Widget / 감쌀 위젯   
fit: BoxFit /

- *Padding*   
padding값만 부여가능한 container와 동일한 위젯. 감싼 위젯에 Padding값만 줄 경우 쓰는 위젯.    
Padding: EdgeInsetsGeometry / 일반적으로, 상속한 EdgeInsets.all(10)등으로 생성해 입력. .all()은 named 생성자./ padding에 대한 정보 명시.    
child: Widget/ 감쌀 위젯   

-*Flexible*     
감싼 위젯이 column(row) 내에서 차지할 공간을 특정 조건에 맞게 지정해주는 위젯.   
fit: FlexFit / Flexfit은 enum으로 loose,tight 존재. child위젯이 차지할 공간을 어떻게 설정한지 지정.    
(FLexFit.)loose / 위젯의 디폴트 값으로, explicitly 사용할 필요x. child위젯이 기존 위젯이 필요한 공간만큼만 차지하게 설정.    
(flexFit.)tight / cihld위젯이 column내 차지할 수 있는 최대 공간을 차지하도록 설정.    
tight로 설정된 column의 원소끼리는 전체에서 loose인(flex가 없는) 원소 제외 남는 가능한 공간을 일정 비율로 배정.    
flex : int=1 / 해당 child가 다른 tight로 설정된 child와 비교했을때 차지할 공간의 비율 설정. default는 1.   
tight한 원소끼리 flex가 같다면 위젯의 기존 크기와 상관없이 크기 동일. 즉, 한 위젯의 크기가 다른 위젯에 비해 유동적으로 커지는 것을 방지 가능.        
(loose인 원소에도 flex값을 줄 수있는데, 이 값이 남은 공간을 계산할 flex합에 포함되어, 다른 tight한 원소들이 그외 남은 공간을 최대로 차지.    
하지만 여전히, loose인 원소는 자신이 기존에 필요한 공간만큼만 표시. 즉, tight한 원소들이 있더라도 column내 공백이 생기게됨.)    

- *Expanded*   
fit: FlexFit.tight으로 고정되있는 Flexible위젯. column(row)내 원소의 fit을 tight으로 할시 fit 인자를 생략하고 Expanded를 사용 가능.    
tip: Expanded를 ListView와 같은 infinity height을 같는 위젯에 씌울 경우 error. ?    


- *CircleAvatar*   
원형의 컨테이너와 동일한 위젯. 감싸는 위젯을 원형의 공간 내에 배치.    
radius: double / 원의 반지름 지정. 해당 값이 지정되면 minRadius,maxRadius는 무시. 디폴트값은 20.        
(min)MaxRadius /    
child: Widget / 감쌀 위젯    
backgroundColor: Color / 원의 색깔. default는 Theme의 primaryColor    
backgoundImage:   ImageProvider / 원 안에 image를 입력. imageProvider객체를 받음.       
(ImageProvider는 Image위젯이 아닌 이미지 자체를 전달하는 클래스로, AssetImage(), NetworkImage()등으로 생성.)     
(이름에서 마찬가지로, 폴더에저장된 이미지는 AssetImage, Url로 가져오는 이미지는 NetworkImage로 생성. positional arg로 Url입력.)          
(단, 위젯이아니므로 fit같은 설정 불가능.이미지를 입력하면 알아서 원의 크기에 맞춰짐.)      
 
- *LayoutBuilder*   
위젯을 기존 부모 위젯의 constraints에 responsive하게 크기를 지정해주려할때 사용하는 위젯. 부모위젯의 크기가 유동적이어도 사용 가능.       
builder: (BuildContext, BoxConstraints)-> Widget / 부모위젯의 크기를 참고해 자식위젯을 생성해서 반환하는 함수를 builder에 명시.       
builder의 함수에 context와 현재 부모위젯의 BoxContraint를 인자로 전달. 이 constraint를 참고해 자식위젯의 크기를 설정할 수 있음.    
ex) (생성할 자식위젯의) height: constraints.MaXheight*0.7,     

- *Switch*    
toggle할 수 있는 switch버튼 위젯. 클릭시마다 switch를 toggle에 UI상에서 보여줘야하므로 현재switch value값 저장할 변수와 stateful 필요.     
value: bool / UI상 Switch가 표시할 상태(toggle의 on/off값, true면 켜진 switch.)(일반적으로, 따로 선언한 현재 switch값 변수 전달)    
onChanged: (bool){} / Switch를 눌러서 값을 toggle할때 실행할 함수. 함수의 인자로 변화된 toggle의 value값을 전달해줌.    
activeColor: Color / on되었을때 switch색상 설정.    
(일반적으로, 함수 body의 setState내부에서 전달받은 bool값을 state내의 변수에 저장. Switch의 value값이 변해 switch re-rendered)      
Switch.adaptive : Switch 렌더링시 Platform.IsIos를 확인하여, android/ios style에 맞는 디자인의 스위치 생성.    

- *SafeArea*   
위젯을 감싸 OS 인터페이스 영역(status Bar 등)을 피해서 해당 위젯 생성.     
ex) cupertinioScaffold의 child부분을 감싸 notch부분 피해 생성.     
child: Widget

- *ClipRRect*    
위젯을 감싸 rounded-rectangle형태의 clip으로 강제 하는 위젯.    
image,GirdTile등이 (따로 shape이나 border를 지정할 수 없는 위젯들) rounded하게끔 변형할때 사용.    
child: Widget    
borderRadius: BorderRadius // 각 원형 모서리의 반지름 지정.

- *Divider*    
divider line을 만들어주는 위젯.    
thickness: double // 
color: Color //

- *DefaultTabController*    
Tab screen을 컨트롤하는 위젯. child로 screen위젯(scaffold등)을 가짐. screen내의 TabBar()와 TabBarView()간의 작동을 조절.       
Screen내의 TabBar와 TabBarView와 자동적으로 연결되어, TabBar에 선택된 tab에 따라 TabBarView의 child중 하나를 선택.     
(일반적으로 컨트롤러가 screen을 감싸고, TabBar는 AppBar의 bottom에, TabBarView는 scaffold의 body에 사용하는 안드로이드스타일 탭 구성.)      
(stateful,stateless 모두에서 사용 가능.)    
length: int // screen내의 tab수 명시. 해당 값은 TabBar.tabs와 TabBarView.children의 길이와 같아야함.     
child: Widget // 감쌀 tab을 포함하는 screen위젯.     

- *TabBar*    
material design의 TabBar. 일반적으로, scaffold AppBar의 bottom에 사용.       
tabs: List<Widget> // 일반적으로, 2개 이상의 tab의 List명시    

- *Tab*    
TabBar에 사용되는 각 Tab위젯. tab내에 icon, text등을 포함 가능.    
icon: Icon //
text: String //

- *TabBarView*     
TabBar와 연동하여, 각 tab이 선택되었을때 표시할 페이지뷰(혹은 위젯)를 명시하는 위젯. 선택된 tab에따라 다른 위젯을 표시.     
children: List<Widget> // 페이지로 표시될 위젯의 List. 길이가 controller의 length값과 같아야함.     
 (TabBarView가 scaffold의 body로 들어가는 일반적인 경우, 각 child는 (scaffold와 같은) 페이지가 아니어도 가능. body부분에 들어갈 위젯이면 충분.)           
 

- *BottomNavigationBar*     
scaffold의 BottomNavigationBar에 들어가는 일종의 TabBar위젯. 아래쪽탭 스타일을 구성할때 사용.      
NavigtaionBar를 이용하여 tab_screen을 구성하는 경우, screen을 수동으로(onTap) 바꾸기위해 "반드시 stateful위젯과 함께 사용."    
items: List<BottomNavigationBarItem> // tab의 각 버튼 리스트.    
onTap : (int){} // 각 tab버튼이 눌렸을때 호출하는 함수 명시. 눌려진 버튼의 index를 함수에 전달함. 이 함수에서 setState를 호출해 화면을 바꿔줌.    
 (ex) void _selectPage(int index) { setState(() { _selectedPageIndex = index;}); }를 onTap에 명시,    
 final _pages = [Screen1(),Screen2()];와 같이 page에 들어갈 위젯 목록을 미리 선언해두고, scaffold의 body에 _pages[_selectedPageIndex]로 선언하면,    
 자동으로, 화면이 tab될떄마다 rendering되어짐.  )     
currentIndex: int // 눌려진 버튼의 index를 명시. (일반적으로, 버튼의 index를 저장하는 변수를 생성해두고,     
 Tab될때마다(state이 바뀔때마다) 변화된 index를 명시해줘야, navigationBar도 해당 index의 tab이 눌려진 상태를 표시해줌.)     
(un)selectedItemColor : Color // (미)선택된 아이템의 색깔 지정.    
(un)selectedFontSize : double // (미)선택된 아이템의 폰트사이즈 지정.    
 type: BottomNavigationBarType // 네비게이션 바의 타입 지정. BottomNavigationBarType는 enum으로 default는 fixed(버튼들이 고정.). 이 외에 shifting(누르는쪽으로 shift, 나머지는 아이콘만.) 등 존재.      
 
 - *BottomNavigationBarItem*      
BottomNavigationBar의 items 리스트에 포함되는 각 item(tab)에 대응하는 클래스.    
material design내에서 BottomNavigationBar사용시 icon, text는 not-null이어야함.    
icon: Icon // tab버튼에 표시되는 아이콘 지정.    
title: Text // tab버튼에 표시되는 title(일반적으로, text)     
backgroundColor: Color // 버튼의 bacgkround Color지정.(일반적으로 tab의 bg색은 네비게이션바의 bg색을 따르지만,      
바의 type이 shift면, 버튼색이 표시되므로 따로 명시 필요.)      

- *Drawer*    
scaffod의 drawer인자에 사용되는 drawer위젯 클래스. 일반적으로, drawer에 pushReplacement를 써서 screen switch 돕는 위젯목록을 생성.     
drawer 밖은 자동으로 backdrop생성(세기 지정 가능), 밖을 누르면 자동으로 navigator pop 호출.(drawer에서 나가짐.)    
child: Widget // drawer에 표시될 위젯.(ex)여러 개의 링크 목록을 배치시, 여러 ListTile(onTap 포함, drawer에 적절한 스타일)을    
자식위젯으로 하는 Column을 사용 가능, onTap에서 navigator로 페이지 이동. push가 아닌 screen switch(pushReplacement))            
elevation: double // drawer 뒤의 backdrop(어두워지는 배경)의 정도 지정.      

- *PopupMenuButton*     
누르면 옆으로 메뉴 목록이 overlay되는 버튼. appBar의 action 등에 사용하기 적합.       
icon: Icon // 버튼의 아이콘 모양 지정.     
itemBuilder : (BuildContext) => List<PopupMenuEntry> // 메뉴의 빌더 명시. 함수에서 메뉴에 들어갈 각 목록(PopupMenyEntry)을 리스트로 반환.      
 onSelected : (value){} // 메뉴 item이 선택됬을때 호출할 함수 지정. 선택된 item의 value값을 전달.      
(PopupMenuButton의 item인 PopupMenuEntry에는 일반적으로, PopupMenuItem 사용.)     

- *PopupMenuItem*     
PopupMenuButton의 item으로 사용되는 위젯.    
child: Widget // item으로 명시될 위젯. 일반적으로 Text나 ListTile    
value: dynamic // 해당 item이 가질 value 입력. 아이템이 입력됬을때 이 value를 PopupMenuButton의 onSelected함수에 전달함.   
(일반적으로, value 0~k의 index를 사용하거나 enum으로 선언해서 관리. onSelected에서도 enum으로 받음.)        

- *Chip*    
material 디자인의 클립을 표현하는 위젯.    
label: Widget // 클립 내부의 주요 content가 되는 위젯. 일반적으로 text

- *Spacer*     
column, row등에서 사용할수있는 남은 공간을 최대로 차지하는 flex위젯. flex지정 가능. child는 없고 공간만 차지하는 위젯.          
int flex= 1로 다른 flexible 존재시 default로 다른 flexible과 남은 공간을 균등하게 차지.
ex) Row의 children에 Spacer()를 추가하면, Spacer가 다른 (flexible이 아닌)child제외 남는 가능한 공간을 전부 차지.     

- *Dismissable*    
감싼 위젯을 화면에서 swipe하여 지울수있도록 도와주는 위젯. swipe되면 UI상에서 위젯이 지워진다. 실제 데이터상에선 아무일도일어나지않음.(onDismissed에서 명시)          
key: Key // dismissable의 키 지정. // 일반적으로, ValueKey 지정. // staeful & List issue참조.  
(dismissable은 내부적으로 sateful이고, 일반적으로 list의 item으로 사용되므로, 지워졌을때 타일이 사라지고, 다음 타일들의 상태를 유지하며 채울 수 있도록)    
child: Widget // 감쌀 위젯     
background: Widget // 위젯이 swipe될때 뒤의 배경으로 표시될 위젯. ex) 삭제 구현시 일반적으로, 빨간색 배경과 휴지통 아이콘을 포함한 Container     
direction: DismissDirection // swipe되는 방향 지정. default는 DismissDirection.horizontal(양쪽)      
onDismissed: (DismissDirection){} // swipe되어 지워질때 호출할 함수 명시. 함수인자로 swipe된 방향 전달.(방향에 따라 다른 처리 가능.)     
(swipe시 UI상에서만 지워지기때문에, 일반적으로 실제 데이터상에서 지워주는 작업을 함수에서 명시.)      
confirmDismiss: (DismissDirection)=> Future<bool> // dismiss시, dismiss를 다시 확인받는 함수 명시. 함수에 dismiss direction전달      
(함수의 return인 Future값이 true면 dismiss, false면 취소. 예를들어, 함수에서 showDialog()를 return하고 dialog는 Future<true,false>를 return)    
(참고: showDialog는 Future객체를 반환하고 이때 generic값은 dialog pop시에 전달되는 값이므로 즉, Navigator.of(ctx).pop(true/false)로 dialog탈출)

- *SnackBar*     
전체 페이지의 SnackBar로 표현될수있는 위젯.       
content: Widget // 스낵바의 main content가 되는 위젯. 일반적으로 text     
duration: Duration // 스낵바가 생성되고 사라지기까지 표시되는 시간을 지정.     
action: SnackBarAction // 스낵바에 표시될수 있는 action(버튼). SnackBarAction위젯을 받음. 일반적으로, UNDO 같은 버튼 명시.      

- *SnackBarAction*     
SnackBar의 action에 쓰이는 위젯.      
label: String // action에 쓰일 라벨(text).    
onPressed: (){} // 버튼 터치시 호출할 함수. 일반적으로, 방금 operation(결과로, 스낵바를 띄운)의 undo     

- *AlertDialog*     
Dialog로 쓰이는 일반적인 위젯.     
title: Widget // dialog의 title. 일반적으로, Text 혹은 Row(icon+Text)     
content: Widget // dialog의 main content. 일반적으로, 위젯을 감싼 singleChildScrollView 명시. (content가 dialog크기보다 커지는 경우 방지.)      
actions: [] // dialog의 아래쪽에 사용될 위젯명시. 일반적으로, TextButton/FlatButton    

- *Form*    
user input의 submit, validation 등을 묶어서 관리할수 있도록 도와주는 위젯.      
child: Widget // Form이 감쌀 위젯. 일반적으로,textField와 저장버튼 등을 포함하는 scrollable column/listview
key: Key // Form에 key지정. 일반적으로, form외부에서, form의 state에 접근하여, validate, submit등을 관리하기위해, GlobalKey<FormState>를 생성하여 입력.       
(ex) state객체 내부에 _form = GlobalKey<FormState>(); 후 Form(key: _form),처럼 명시)     
 (GlobalKey<FormState>는 FormState의 data를 refer할 수 있는 key. GlobalKey는 위젯과 상호작용할수있는 key로 거의 Form에만 사용.        
 GlobalKey<FormState>.currentState.save() // Form내의 각 Field의 onSaved함수 호출.      
  GlobalKey<FormState>.currentState.validate() // Form내의 각 field의 validator호출. 모든 validate 통과시 true반환. 하나라도 error시 false반환.    
autoValidate: bool // 모든 keyStroke마다 각 field의 validator를 호출할지 지정.    

- *TextFormField*    
Form에 사용될수있는 TextField위젯. default width constraint는 사용가능한 최대로, row안에 직접 사용시 error. 이런 경우, 길이를 지정하나, expanded로 감싸 사용.           
decoration: InputDecoration // textField의 꾸밈 지정. label, hint text 지정 가능.     
textInputAction: TextInputAction // 입력시 soft Keyboard의 완료 버튼이 어떤모양일지 명시.(next, done등. 실제 동작은 따로 명시필요.(onFieldSubmitted,FocusNode))    
keyBoardType: TextInputType // 입력시 toggle되는 soft keyboar의 type지정.    
focusNode: FocusNode // 해당 TextField의 FocusNode명시. 명시된 Focus로 다른곳에서 이곳으로 Focus이동 가능.         
onFieldSubmitted : (value){} // textField의 완료버튼(next,done..)을 눌렀을때 호출할 함수. 해당 함수에 field에 입력된 값 전달.     
onEditingComplete: (){} // (?) onFieldSubmitted와 다른점?     
maxLines: int // text Field를 클릭해 입력시, input field UI의 크기(라인 수) 지정. default는 1.        
onSave: (String){} // (formKey.)currentState.save() 호출시 현 text field에서 호출되는 함수. field의 현재 입력값을 함수에 전달.      
validator: (String)=>String // (formKey.)currentState.validate() 호출시(혹은, autoValidate설정시 매keyStroke마다) 호출되는 현 입력값에 대한 validate확인함수 명시.      
(Field의 현입력값을 함수에 전달. 함수는 String을 반환하는데 값이 null이면 validate시 현 필드의 error가 없는것으로 인식. null이 아닌 String은 error메시지로 표시됨.)      
(validate 호출 후 fail시에 즉시 UI에 표시되는 각 error 메시지는 각 TextFormField의 decoration: InputDecoration에서 꾸밈(지속시간, 텍스트스타일 등) 지정 가능.)      
controller: TextEditingController // controller로 입력값을 관리시(ex)Form의 input은 onSaved등으로 관리하지만, 따로 현재입력값을 읽을필요가있다면), controller 명시.       
initialValue: String // TextField에 처음 저장되있을 text. (단, Field의 controller가 non-null이면 동시 사용 불가능. controller값으로 field값이 채워지므로.)     
(controller가 있는 textField의 초기값을 주고 싶다면, field가 포함된 위젯 초기화시(state라면, initState나 didChangeDependecies) (controller.)text = (String)으로 초기화.)       
