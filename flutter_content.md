## Class, Method, enum etc.    

- *runAPP()*     
widget인스턴스를 받아 build를 호출하여 실행해 화면에 띄어주는 함수. main()에서 메인widget실행   

- *Build(Buildcontext context)*    
위젯의 생성함수. 위젯(trees)을 필수로 return. 모든 위젯은 반드시 @override. Buildcontext는 메타정보를 담은 객체로, 자동 전달됨.   

- *Buildcontext*    
각 위젯에 대한 정보와 위젯 트리상의 위치 및 주변 위젯에 대한 정보 등을 포함한 메타 정보 객체.     
context객체를 통해, deep한 위젯 트리 상에서, 모든 정보를 생성자를 통해 전달하지 않아도, 특정 위치의 위젯에 포함된 정보를 참조할수 있음.     
ex) MediaQuery, Theme에서 context를 이용해 inheritedWidget(app에 대한 정보를 포함한 internal위젯)에 직접 접근.      

- *State 클래스*     
Stateful위젯에 대응쌍관계인 class. extends State<StatefulWidget(대응하는 Statefulwidget 객체입력)>로 생성(generic)    
Stateful위젯이 re-build되어도 객체가 다시생성되지않음. data를 유지하며 Stateful위젯의 상태 저장.   
값이 변동되는 변수와 위젯의 build함수를 포함. Stateful위젯 첫 생성시뿐만 아니라 state가 변해 UI를 다시 표시할때도 build함수가 실행돼 위젯을 다시 생성.   
getter인 widget.()로 현재 대응되는 stateful위젯에 접근가능.    

- *Color & Colors*    
Color / 색을 표현하는 binary값을 가지는 class. 각 object는 특정색깔을 표현.  
Color.fromRGBO(r,g,b,opacity) / Color 객체를 r,g,b값으로 생성. 직접 색상의 rgb값 찾아서 생성할때 사용.      
Colors / 여러 Color객체을 static으로 선언해둔 utility class. 즉, 객체화 없이 Colors.black 등으로 여러색의 Color객체 사용 가능.    
(Color.)withOpacity(double) // 해당 Color의 불투명도를 바꾼 Color 반환     

- *setState()*    
 state클래스 내에 존재. state내의 변수를 변경한 후 UI를 re-rendering하는 함수.   
 (build()/UI에 영향이 있는)state내의 변수를 변경하는 anonymous함수를 argument로 받음.   
 호출시 변경한 값에 맞는 현위젯 UI를 re-build. ex) setState( ( ) { idx++; } );      
(변경된 변수로 인해 위젯build내의 UI가 바뀌는 자식위젯만 re-build, Stateless위젯의 input data가 변경되면 해당위젯도 re-build)    
(build내의 변하지 않는 위젯은 re-paint. 새로 생성은 안하지만 다시 표시하는작업.)   

 - *Stateful/Stateless위잿*   
공통점: 생성되어 UI를 rendering. 외부에서 데이터를 받을수 있다.(생성자)   
차이점: stateless는 외부로부터의 인풋데이터가 변할시에만 widget을 re-rendering하고 stateful은 internal state가 변할때에도 re-rendering하여 현재 UI변경 가능(ex)setState를 통해).   

- *createState()*   
 Stateful은 반드시 대응 state클래스를 인스턴스화. @override createState()   
 ex) State<Statefulwidget>(= myApp) createState() => myApp(); (State객체를 return해야함!)   
 
- *@required*    
flutter가 제공하는 함수의 각 argument에 대한 decorator로 호출시 반드시 필요한 인자임을 명시. 없다면 호출불가.(error)     
'package:flutter/foundation.dart'에 명시됨.(혹은 'package:flutter/material.dart'도 포함)   

- *showModalBottomSheet()*   
현 화면에서 다른 위젯을 modalBottomSheet으로 띄워주는 함수. 
@required context: BuildContext(현 클래스의 메타정보를 전달해주어야함. 함수를 호출하는부분(?)의 BuildContext를 전달)   
@required builder: (BuildContext)->Widget (띄울 위젯을 생성하는 함수, 이 함수의 argument로 modalSheet(?)의 context가 전달됨.)    
TextField를 modalBottomSheet으로 생성시 인풋이 다른 field로 넘어가면 사라짐. => TextField를 stateful로 ($$$$$)
TextField에서 onSubmitted 등으로 입력을 완료했을때 자동적으로 modalSheet가 꺼지게 하려면, Naviagor.of(context).pop(); 으로 sheet탈출.       
ModalSheet이 가지는 maxHeight존재.(tip: 위젯의 크기가 modalSheet의 크기보다 커지는 경우 scrollable하게, 위젯을 scrollView로 선언가능.)    

- *ThemeData*    
Material App의 Theme에 대한 정보를 저장하는 클래스. material Theme에 지정시 다른 위젯에서 Theme.of(context).(label)로 참조해서 사용.   
대부분의 값이 default값을 가짐.     
Cupertino App의 대응클래스는 cupertinoThemeData    
primaryColor: Color / theme의 default로 쓰일 color. 다른 위젯에서 이 색상만 지정가능.    
primarySwatch: Color / theme의 한 색상을 여러 shade가 있는 그룹으로 사용. 위젯에서 theme색 참조시 여러버전으로 참조가능.     
accentColor: Color / 보색으로 쓰일 색상 지정. Material design Theme문서에 여러 조합 검색 가능.    
accentColorBrightness // accentColor의 brightness. (?)명시된 밝기에 따라, accentColor위의 text나 icon의 색깔이 달라짐   
canvasColor: Color /    
errorColor: Color / 에러에 쓰일 색상 지정. default는 red.   
fontFamiliy: String / app의 global폰트family 지정(text의 default폰트?)     
textTheme: TextTheme / app의 text별 theme지정. TextTheme은 여러 text종류별 style을 저장하는 객체.   
accentTextTheme: TextTheme // accentTextTheme 지정.     
ButtonTheme: ButtonTheme / 버튼의 theme지정. ButtonTheme에는 buttonColor, textTheme, shape등 포함.         
(TextTheme(title: TextStyle, body1: TextStyle)처럼 여러 label별 textStyle을 theme으로 지정 가능. 다른 위젯에서 사용시 label로 참조.   
style: Theme.of(context).textTheme.title) 혹은 color : Theme.of(context).textTheme.button.color 와 같이)    
appBarTheme: AppBarTheme / appBar에 사용될 별도의 theme지정. appBarTheme정보를 나타내는 객체. textTheme등 지정 가능.       
ThemeData.light() / 모든 값이 default setting으로 이루어진 ThemeData 반환.      
여러 theme을 나타내는 객체는 (theme.)copyWith({})로 특정 named argument를 overwrite하여 그대로 사용 가능.    
ex) 기본 설정의 textTheme을 수정해서 사용할시, themeData.light().textTheme.copyWith(...)처럼 사용.   
pageTransitionsTheme: PageTransitionsTheme // 모든 페이지 이동시에 사용할 transitionTheme 지정. platform별로 지정 가능. 
// custom transition을 작성해 사용하고자할때, 명시.    
ex)
(PageTransitionsTheme(   
 builders: Map<TragetPlatform, PageTransitionBuilder> // 플랫폼별 모든 페이지라우팅에 사용할 트랜지션빌더를 Map형태로 지정.      
 // ex) builders: {TargetPlatform.iOS: CustomPageTransitionBuilder(), TargetPlatform.android: CustomPageTransitionBuilder(),},      
))     

- *BoxDecoration*   
container의 decoration 인자로 들어가는 decoration에 관한 정보를 표현한 클래스.    
border : BoxBoarder / 일반적으로, 상속한 Boarder객체 입력. Boarder의 정보를 다음 class    
color: Color / 컨테이너의 background color지정.    
borderRadius: BorderRadiusGeometry / 일반적으로, BorderRadius객체 입력. BorderRadius의 정보를 담은 class.   
(BorderRadius.circular(double), BorderRadius.all(), BorderRadius.vertical(), ...)    
shape: BoxShape / 컨테이너의 모양 결정 BoxShape은 enum으로 circle, rectangle(default)등 가능.    
gradient: Gradient / background를 gradient있는 색상으로 지정. 

- *Gradient*    
gradient를 표현한 객체. 색상 gradient하게 표현가능. 상속하는 여러 class존재.   
LinearGradient( // linear한 gradient생성.    
colors: List<Color> // gradient하게 표현될 색상들 지정.    
begin: AlignmentGeometry // 시작 색상 위치 지정. 일반적으로, Alignment객체 사용.    
end: AlignmentGeomtery // 마지막 색상 위치 지정. Alignment는 여러 static값도 저장되있는 클래스로, Alignment.topLeft 등을 포함.    
 
- *Border*   
BoxDecoration의 border 인자로 들어가는 Boarder에 대한 정보를 표현한 클래스.   
Border.all() / Border의 named생성자. color, width등 지정 가능. border의 모든 방향으로 같은 값 적용.   
(예: 예제코드 Meals의 chart_bar에 자세히 구현.)    
 
- *BorderRadius*   
BoxDecoration의 bolderRadius 인자 등으로 들어가는 Border 꼭짓점부분의 곡면Radius에 관한 정보를 표현한 클래스.   
BorderRadius.circular(double) / BorderRadius의 named생성자, 반지름 값을 지정해 원형의 곡면 반지름 지정 가능.      
BorderRadius.all() //    

- *showDatePicker() -> future<DateTime>*    
현재 화면에서 달력의 날짜를 선택할수 있는 overlay창(datePicker)을 띄워주는 flutter내의 함수. (다른 package?)       
입력을 받기위해 대기하며 입력을 받으면 저장하는 future객체를 반환. 즉, 입력을 저장할 객체(d)를 하나 선언해두고, then/await등으로 처리        
showDatePicker(...).then((DateTime){ if(d==null) return; ...})처럼 then에 전달하는 함수의 body에서 입력을 저장하는 방식으로 사용.    
@required context: BuildContext / 현 클래스의 메타정보를 전달해주어야함.    
@required initialDate: DateTime / 창을 띄웠을떄 선택되어있을 날짜 지정. 일반적으로, datetIme.now()   
@required firstDate: DateTime / 선택할수 있는 가장 빠른 날짜 지정.    
@required lastDate: DateTime / 선택할수 있는 가장 느린 날짜 지정.   

- *MediaQuery*    
실행되고 있는 device에 대한 정보들을 참조할수 있는 클래스. Theme과 같이 .of(context)로 메타 정보를 사용해 연결하면 MediaQueryData객체 반환.    
(tip: 한 build내에서 MediaQuery.of(context)를 자주 생성해 참조하는 경우, build함수 내 final로 객체를 생성해 사용하면 performance증가)   
(MediaQuery.of(context).)size / device의 size를 나타내는 객체. size내에 height,weight등 참조 가능.    
(tip1: 위젯의 layout을 device에 "Responsive"하게 지정하려면, device의 크기에서 각종 디폴트 크기(상태바 높이, appBar높이)를 빼준 값에    
0~1사이의 값을 곱해 위젯의 크기로 지정해줌. (전체 채울수 있는 공간에 비례하여 위젯크기 지정).      
(tip2: device의 크기에따라 위젯을 띄우거나 일부만 띄우거나 등등 유동적인 build가능. ex) width > 300 ? text+icon : icon) )    
(MediaQuery.of(context).)padding / app을 감싸는 system UI(status Bar등)의 정보를 나타내는 객체. top(상태바 높이)등 참조 가능.   
(MediaQuery.of(context).)textScaleFactor / 사용자가 device내에서 설정한 text크기(default:1)에 접근 가능.(즉, 이값을 저장해(final변수)  
(tip: 텍스트 폰트 크기에 곱해서 설정해주면, (ex) Fontsize: 20* curSaleFactor) 사용자가 지정한 텍스트설정에 따른 텍스트 표현 가능.)    
(MediaQuery.of(context).)orientation / 현재 device의 orientation(portrait,landsacpe)참조. 현재 Orientation(enum)값 반환.   
(MediaQuery.of(context).)viewInsets / device에 종속적인 display에 대한 정보 참조가능. (일반적으로, keyboard에 해당하는 정보)    
(tip: viewInsets.)bottom을 통해 keyboard가 켜졌을때, keyboard의 길이를 참조할 있음. keyboard가 올라오면 해당 값이 변경.    
즉, stateful내의 TextField내에서 해당값에 따라 padding을 주도록 설정하면 keyBoard가 올라올때 그에 맞게끔 UI를 변경 가능.)         
MediaQuery도 object로 MediaQuery를 호출해 참조하는 모든 build는 orientation, viewInsets등이 변하면 re-build.    

- *BoxContstraints*    
모든 위젯이 가지는 객체로서, 위젯이 redering되는 범위의 constraint에 대한 정보 저장. 
모든 위젯에 직접 설정해줄 수있으며, 많은 basic 위젯들은 default값을 설정되있음.(ex)ListView의 height은 infinity)    
Height은 minHeight, maxHeight를 가지며, 해당 범위 내에서 rendering 가능. (Weight도 동일)       

- *key*     
각 위젯이 가질수 있는 Key를 나타내는 객체. 일반적으로, 필요하지 않음. key를 명시할 필요가 있는 경우만 사용.    
거의 유일하게, List + Stateful item을 사용할때 필요.    
UniqueKey() / 매 호출시 unique한 key생성.      
ValueKey(String) / String에 따라 서로 다른 key생성. String이 다르면 다른 key가 생성됨.     

- *Navigator*    
flutter앱의 화면이동을 도와주는 클래스. 현위젯과 위젯 트리의 구조를 알기 위해 Navigator.of(context)로 호출.     
(Navigator.of(ctx).)push<T>(Route) // 현 스크린에서 다른 페이지를 생성해 페이지 스택에 추가. Route객체를 받음. T는 pop시에 resolve될 객체에 명시할 타입.        
(Navigator.of(ctx).)pushReplancement(Route) // 현 스크린을 스택에서 제거하며 다른 페이지를 추가 및 이동.(즉, 이전화면으로 갈수없음. login page->main page이동 같은 경우 사용)     
(Navigator.of(ctx).)pushNamed(String, aruments: Object) // 다른페이지의 라우팅을 미리 선언해놓은 이름으로 참조해 추가 및 이동.    
(모든 push함수는 future객체를 반환. 즉, 해당 push가 종료된 이후(push만이 아니라 추가된 페이지가 종료되어 현재 페이지로 돌아오는것까지.) 실행할 루틴을 지정 가능.)      
(Navigator.of(ctx).)pop(Object) // 현재 페이지를 스택에서 제거. 이전 페이지(현재 페이지를 push한)를 다시 띄움.     
(dialog나 modalSheet과 같은 overlay 스크린에서도 pop으로 탈출 가능. 두 경우 모두 내부적으로 스크린으로 다뤄짐.)     
(이때 이전 페이지로 돌아가는 과정에서 Object전달 가능. 해당 Object는 이전페이지의 push에서 생성한 future객체의 then()으로 받을수 있음.     
ex) 이전페이지에서 push시 pushNamed(..).then ((ret){});로 push가 완전히 끝난후(pop된 후), ret(Object)를 받아 실행할 함수 명시 가능.    
해당 함수에 pop의 object전달. pop으로부터 전달된 객체가 없다면(단순 back버튼과 같이), ret == null. 여러 경우가 있다면, 함수내에 필요한 logic구현.)           
(Navigator.of(ctx).)canPop() -> bool // 현재 페이지를 pop할수있는지를 반환. 즉, 현재 페이지 말고 밑에 다른 페이지가 남아있는지 확인가능.    

- *Navigator:pushNamed*    
pushNamed(String,arguments: Object)    
미리 선언해놓은 빌더 name으로 라우팅(materialApp(cupertinoApp)의 routes테이블에 선언.)     
Navigator의 pushNamed로 페이지를 라우팅할시, 같이 전달할 객체들을 명시 가능.(list,map 등)        
(일반적으로, app(main.dart)의 routes에 각 이름으로 선언해놓은 페이지 builder들은 생성할 위젯의 생성자에 전달할값을 미리 알수가 없으므로,    
pushNamed로 해당 페이지 생성시 생성자를 사용해 값을 받지않고, 해당페이지를 생성하는 pushNamed 함수와 argument를 통해 값을 전달받는다.)       
이때, 각 페이지 위젯에서는, ModalRoute객체를 통해 페이지를 라우팅하며 전달된 argument을 저장해 사용.     

- *ModalRoute*     
 현위젯(페이지)의 라우팅정보를 참조할수있는 클래스. 현 위젯의 정보를 알기위해 Modal.of(context)로 사용.     
 ModalRoute.of(ctx).settings.arguments // 현 페이지 라우팅시에 같이 전달된 세팅 중 argument의 값 참조.    
 pushNamed로 해당 페이지가 생성되고 argument로 필요한 데이터가 전달된 경우 ModalRoute를 사용해 데이터를 저장할 수 있음.     
 ex) 위젯의 build내에서(항상?), final routeArgs = ModalRoute.of(context).settings.arguments as Map<String, String>; 
 (arguments는 일반 Object로 특정 type만 전달할시, 해당 type을 알수있게끔 따로 명시)       
 
- *Route*    
새로운 스크린 위젯(페이지)를 빌드해주는 클래스. MaterialPageRoute, CupertinoPageRoute 등이 있음.    
builder: (BuildContext ctx) => Widget // 빌드할 위젯(페이지)을 반환하는 함수인 builder 지정. 빌더에 현위젯의 ctx를 자동으로 전달.   
fullScreenDialog: bool // 페이지를 디폴트값인 slide 애니메이션으로 불러오고 뒤로가기표시를 생성할지(false), 전체화면에 바로 띄울지고 x표시를 생성할지(true)결정.        
ex)MaterialPageRoute(builder: (ctx) { return CategoryMealsScreen(id,title); }, ),
(불러올 새로운 페이지의 생성자를 통해 데이터도 전달 가능.)    

- *Material(Cupertino)PageRoute<T> & CustomPageRoute*     
일반적인 pageRoute클래스를 extends하여 customPageRoute를 생성 가능. 페이지 이동시 transition을 custom하게 구현 가능.    
해당 라우팅을 사용하기 위해선, Navigator.push(ModralPageRoute)를 사용해야함. (route객체를 쓰지않는 pushNamed에선 사용 불가능.)      
```Dart	 
 class CustomRoute<T> extends MaterialPageRoute<T> { // pageRoute<T>은 pop시 resolve되는 값을 generic으로 가짐 
  CustomRoute({
    WidgetBuilder builder,
    RouteSettings settings,
  }) : super(     // 부모인 MaterialPageRoute를 초기화.
          builder: builder,
          settings: settings,
        );
 // route가 사용할 Transition을 override하여 custom하게 정의. // 커스텀라우팅 객체를 Navigator.push에 사용하면, 이 transition을 사용해 페이지로 이동.
  @override
  Widget buildTransitions(
    BuildContext context,
    Animation<double> animation, // 페이지 이동시 제공되는 double형 애니메이션. 이 애니메이션으로 여러 효과 구현.
    Animation<double> secondaryAnimation,
    Widget child, // 페이지 위젯.
  ) {
    if (settings.name == '/') { // 라우팅이 앱의 첫화면(홈)이라면, 그대로 사용.
      return child;
    }
    return FadeTransition( // 화면을 fade-in효과로 transition.
      opacity: animation,
      child: child,
    );
  }
}
```	  
 
 - *PageTranstionBuilder*       
 page의 Transition을 빌드하는 함수를 포함하는 클래스. App의 pageTransitionsTheme에서 사용 가능. 모든 페이지변환시 사용가능.      
 이 클래스를 상속하여 custom pageTransition구현 가능.
 ex)
 ```Dart	 
 class CustomPageTransitionBuilder extends PageTransitionsBuilder {
 // PageTransitionBuilder가 포함하는 Transition의 빌드함수를 override하여 customize.
  @override
  Widget buildTransitions<T>(
      PageRoute<T> route, // PageRoute클래스 내의 buildTransition과 달리, route인자를 추가로 받음. 해당 페이지 transition을 호출한 route가 있다면 전달.     
      BuildContext context,
      Animation<double> animation,
      Animation<double> secondaryAnimation,
      Widget child) {
    if (route.settings.name == '/') {
      return child;
    }
    return FadeTransition(
      opacity: animation,
      child: child,
    );
    // throw UnimplementedError();
  }
}
 ```	  

- *ShapeBorder*    
card의 shape인자로 들어가는 Border의 shape에 대한 정보를 표현한 클래스.    
RoundedRectangleBorder( // 모서리가 둥근 shape을 나타내는 일종의 shapeBorder클래스.     
borderRadius : BorderRadiusGeometry // 각 모서리의 반지름 지정.    
),     

- *Scaffold.of(context)*        
현재 위젯에서 제일 가까운 Scaffold위젯(즉, 현위젯을 포함하는 페이지)에 연결하는 메소드. 해당 scaffold의 상태 참조 및 여러 메소드 호출 가능. (ScaffoldState반환)        
단, Scaffold를 반환하는 build내에서 직접 참조하면 error. scaffold내의 자식위젯의 build나 위젯 내에서 참조해야함.(context를 이용해 above의 scaffold를 탐색하므로)           
(context가 나타내는 현 위젯이 Scaffold인 경우 자신의 scaffold를 참조하는 방법:      
https://medium.com/@ksheremet/flutter-showing-snackbar-within-the-widget-that-builds-a-scaffold-3a817635aeb2)     
(Scaffold.of(context).)showSnackBar(SnackBar); // 해당 scaffold페이지에 snackBar를 표시.     
(Scaffold.of(context)).hideSnackBar(); // 현재 snackBar가 있다면, snackBar를 숨김 // snackBar를 표시하는 버튼을 빠르게누를때 바로바로 뜨게할떄 사용.      

- *showDialog*    
화면 내에서 dialog를 띄어주는 함수.    
context: BuildContext // 화면을 띄어주는 현 위젯의 context 전달.     
builder: (ctx) => Widget // dialog로 띄워줄 위젯을 빌드하는 함수 명시. 일반적으로, 함수는 AlertDialog반환.        

- *TextEditingController*     
TextField에서 사용되는 field의 input을 관리하는 controller. 해당 컨트롤러는       
ex) state객체 내에서 final _myController = TextEditingController(); 선언 후, TextField의 controller의 인자에 전달하여 명시.     
TextField의 매 key stroke마다 controller에 입력값 저장.     
(TextEiditingController.) //text로 현재의 입력값(String) 참조 가능.       
또한, 선언한 controller는 state소멸시 dispose 필요. dispose()내에서 (TextEiditingController.)dispose();         

- *FocusNode*     
element(TextField등)에 attach할 수 있는 Focus객체. controller와 비슷하게 생성 및 사용. Focus객체를 통해 해당 element의 focus 이동 및 관리.          
ex) state 객체 내에 final _priceFocusNode = FocusNode(); // 와 같이 선언 후,      
TextField(...,focusNode: _priceFocusNode,), // element의 Focus인자에 명시.      
// 다른 곳에서 이 Focus로 이동하려면 FocusScope.of(context).requestFocus(_priceFocusNode); 호출.    
// 또한, state내에서 생성후, state소멸시 memory leak방지를 위해, (FocusNode.)dispose호출.    
@override    
void dispose(){...; _priceFocuseNode.dispose(); super.dispose();}     
//     
(FocusNode.)addListner((){}) // FocusNode의 Focus상태 변화에 반응하는 리스너 생성. 리스너의 인자로, focus변할때마다 호출할 함수를 전달. 일반적으로, initState에서 리스너 생성.       
(FocusNode.)removeListner((){}) // FocusNode의 (메모리 leak방지를 위해)리스너 소멸. 소멸할 리스너를 찾기위해 함수의 인자로, 생성시 사용한 함수포인터를 명시.     
(단, 똑같이 dispose내에서 리스너를 소멸시키는데, 해당 FocusNode자체를 dispose하기 전에 리스너부터 dispose해줌.)    


- *FocusScope*   
Focus의 Scope에 접근하기 위한 객체. 해당 위젯이 포함된 페이지의 FocusScope에 접근시 FocusScope.of(context) 메소드 사용.        
(FocusScope.of(context).)requestFocus(FocusNode) // 인자로 전달된 FocusNode로 Focus이동.     
(FocusScope.of(context).)unFocus() // 현재 페이지의 설정된 Focus를 전부 해제. (즉, textInput등에 focus가 있어 keyBoard가 떠있다면, 해제하고 keyboard소멸)         

- *KeyBoardType: TextInputType*      
input을 받을때 쓰일 KeyBoard의 type 지정.     
TextInputType.multiline // 여러줄의 키가 있는 일반적인 pc형태 키보드. 완료버튼은 엔터키(줄바꿈)로 고정. 즉, multiLine 사용시, textInputAction명시 불가능.       

- *Matrix4*     
contrainer의 transform에 사용되는 클래스     
Matrix4.rotationZ(double Radian) // 회전각을 입력한 Matrix4생성. (named생성자)   
(Matrx4.)translate(double x, [double y, double z]) // 현 Matrix4객체의 위치 변화값 지정하는 메소드    

- *AnimiationController*      
animation의 동작을 control하는 컨트롤러.    
State객체 내의 initState에서 생성하고, dispose에서 (AnimationController.)dispose()를 호출해 소멸. // (?) stateless에서 사용.     
AnimationController(         
vsync : TickerProvider // animation이 포함된 현 오브젝트(위젯의 state)를 명시. 즉, 일반적으로, this
// vsync: this가 동작하기위해선, 현 state객체가 TickerProvider여야함. 따라서, state객체에 SingleTickerProviderStateMixin를 mixin       
// SingleTickerProviderStateMixin는 내부적으로 현 위젯에 애니메이션이 표시될수있는지 등을 결정하거나, 위젯이 애니메이션 표시기간등을 인지 할 수 있게끔 도와주는 메소드등을 포함하는 클래스        
duration: Duration(milliseconds: 300,) // 애니메이션이 변화하는 시간길이 지정. 일반적으로 300 msec       
)        
이렇게 생성된 animationController는 Animation객체의 parent로 attach. 해당 animation객체는 listner가 추가되어있어야 UI로 반영.            
(Controller.)forward()로 애니메이션을 시작. (Controller.)reverse()로 역방향 실행 시작.         


- *Animation<T>*       
시간에 따라 변화하는 애니메이션을 표현한 클래스. Animation은 generic으로 <T>는 변화할 값의 클래스. (예를들어, 크기가 변하는 경우, Animation<Size>)       
(Animation.)value로 값이 변하는 T에 접근 가능.      
(Animation.)addListner((){}) // 애니메이션의 리스너 추가. 애니메이션컨트롤러에 의해 애니메이션이 값이 변할때마다(16milisec) 호출할 함수를 전달.    
 // 일반적으로, 별도의 리스너는 AnimationBuilder 미사용시, 수동으로 애니메이션을 사용할때 사용. 즉, stateful필요. state내에서 선언하여 직접 build호출을 위해 사용.          
 // stateful위젯을 다시 build하기위해 함수로는 setState((){}) 명시. 이후 stateful 위젯 내에서 애니메이션객체의 value에 접근해, 변화하는 값으로 container 크기 등을 지정.       

- *Tween<T>*     
시작과 끝, 두 값으로 변화하는 값을 표현하는 클래스. generic으로 값의 타입을 명시. .animate을 호출해 이 값을 기반으로 변화하는 animation 생성 가능.     
 Tween<T>(    
 begin: T // 시작에 쓰일 오브젝트.    
 end: T // 끝에 쓰일 오브젝트.
)       
 (Tween.)animate(Animation<double>) // Animation을 받아 tween이 지정한 값의 value에 따라 변화되는 animation을 생성, 반환. 일반적으로, CurvedAnimation을 사용.
 
 - *CurvedAnimation*     
 controller와 시간에 따라 값이 변하는 속도를 지정할 수 있는 애니메이션.     
 parent: AnimationController // controller지정. controller의 메소드에 따라 애니메이션을 변화시킴.     
 curve: Curve // 시간에 따라 변하는 속도를 지정. Curve는 Curves(static값을 지정한 클래스)내의 값을 사용.      
 ex) Curves.linear : 일정하게 변함. Curves.fastOutSlowIn : 느리게시작해서 점점빨라짐        
 
 - *Size*    
 width와 height값을 지정할 수있는 클래스.     
 
 - *Offset*    
 상대적 위치를 나타낼 수있는 클래스. Offset(int dx, int dy)로 생성.     
 
 - *ImageProvider*
Image위젯이 아닌 이미지 자체를 담아 제공하는 클래스로, AssetImage(), NetworkImage()등으로 생성.      
이름에서 보이듯, 폴더에 저장된 이미지는 AssetImage, Url로 가져오는 이미지는 NetworkImage로 생성. positional arg로 Url입력.     
단, 위젯이아니므로 fit같은 설정 불가능. 이미지를 전달하면 알아서 전달된 위젯에 따라 크기가 맞춰짐.       
