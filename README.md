# 요약
 __Flutter/Dart__ 를 공부하며 개념들을 정리한 문서입니다. 이전까지 OOP언어인 C, C++, JAVA 등을 다뤄보았고, java를 기반으로한 안드로이드 네이티브앱 개발 경험이 있습니다. 따라서, Flutter/Dart만의 특징 위주로, 실제 개발시 _주관적으로_ 필요한 개념 위주로 정리해 작성한 문서입니다.          
      
## Concept
__1. UI as code (Widget)__   
__2. CrossFlatform__   
__3. Dart(OOP)__   

## tips for development    

- *flutter 공식문서 : Widgets info*   

- *pub.dev : packages info*    
각 패키지에서 API reference를 누르면 사용법, 기능 등이 적힌 문서 확인 가능.

- *같은 폴더 내 파일 import시 import './aa.dart';*    

- *현폴더 상위폴더로 이동시 import '../aa.dart';*     

- *함수, 변수 trl+click : 해당 함수or변수가 포함된 파일 열기*    

- *ctrl + . : 빠른 수정(위젯을 다른 위젯으로 감쌀때)*    

- *ctrl + space : 입력가능함 named arguments목록 출력*   

- *괄호뒤에 항상 ,를 붙이고 auto-formatting을 이용.*   

- *debugging*    
syntax Error : 빌드에러/ runtime Error(Invalid Index) : check problems console/ logical Error : use print() or IDE debugger   

- *Dev tools*   
debug시 명령창에 dart:Open devtoools 입력하여 app의 Widget tree, UI details, performance details확인 가능  
Select Widget Mode: UI를 클릭하면 dev tools의 트리에서 찾아 띄워주는 helper   
Debug Paint: Widget UI가 설정된 방식을 보여주는 helper   
Paint Baselines: text 등의 높낮이를 보여주는 helper   
Repaint Rainbow: UI상에서 위젯이 repaint되면 rainbow색상순으로 표시해줌.   
Performance/memory: 퍼포먼스, 메모리 사용량 등 확인 가능   
      
 - *file per widget*   
 관용적으로 1위젯당 1파일(무조건 같이쓰는경우 등은 예외)쓰는게 권장.   
 복잡합 위젯일수록 한 파일&클래스로 묶어 한 위젯으로 만들어 관리하는게 유리.   

- *secreen상 노란색bar error*   
위젯 UI가 빠져나옴가 devie의 page boundary를 침범했음을 의미. UI설정을 수정해야함.     

- *_*    
관용적으로, 함수 호출시 인자로 받지만, 사용안할 변수의 이름으로 _ 사용.   

--------------------------------------------------------------------------------------------------------
## Dart

- *arrow function*    
한줄 함수(함수의 명령이 한줄)이면 =>로 표현가능. ex) void main() => runApp(myApp());  ( 반환값이 있다면 결과값을 return )    

- *anonymous function*    
익명함수, 재사용하지 않는 함수에 활용. ( arguments ) { body }로 표현가능. ex) (value) { print(value); };    

- *final/const*   
final: runtime시에 initialized된 이후부터 변경x (run time constant)   
const: compile단계에서 변경x (compile time constant)    
주의) const a = [1,2,3]; : object의 포인터를 저장하는 a 및 a의 object인 [1,2,3] 변경불가   
var a = const [1,2,3]; : object [1,2,3] 변경불가. but, a에 다른 포인터값 저장 가능.   

- *named arguments*   
함수 호출시 전달할 인자들 중 일부를 변수 이름으로 명시하여 전달받고자 하는 경우, 함수 선언시 {}로 묶어 선언.   
ex) void func(int a, {int b, int c =3}){} => func(10, b=3, c=4)로 호출.   
- *named arguments for constructor*     
contructor에서 클래스 내의 property의 이름으로 named arguments를 선언 가능. this를 이용해 같은 이름으로 받을 것임을 명시.    
ex) class AAA { String name; int age; AAA({this.name, this.age}); }

- *named constructor*   

- *map method*   

- *getter/setter*   

- *...*   
List b; List a = [1,2, ...b]; : b의 각원소를 나누어 a에 이어 붙임. (List a = [1,2,b] : b가 nested list로 선언됨)   

- *as*   
현재 var이 어떤 type이라는 것을 dart에 명시할때   
사용예제) ↓   

- *Map의 val의 type이 object인 경우*   
Map의 val이 서로 다른 클래스인 경우, 해당 map의 특정 key에 대응되는 val에 접근시 val이 한type이더라도 Object type로 읽어짐.   
ex) map qa = {question: 'what's your favorite color?', 'answers': [..]};   
위 선언에서 qa['answers'].map() 불가능. List의 map()사용시, (qa['answers'] as List< String >).map(); 처럼 as로 List를 명시해야함.    

- *DateTime*   
날짜,시간을 저장할수있는 dart의 buil-in class. DateTime.now() : 현재 timestamp로 DateTime객체 생성하는 생성자.     
tip: now()를 debug시 간편하게 unique한 id생성할때 사용가능.   
(DateTime.)(day,month,year) / 해당 dateTime이 나타내는 년,월,일 참조.   
(DateTime.)subract(Duration) / DateTime의 시간에서 Duration만큼의 시간을 빼주는 메소드.   
(DateTime).isAfter(Datetime2) / 다른 DateTime과 비교하여 그 이후인지를 bool로 반환해주는 메소드.   
Duration() / 시간의 duration값을 나타내는 클래스. 생성자에 named argument전달.(days: int, hours: int 등으로 duration의 길이 지정.)      

- *(object.)toString()*    
모든 object에 암묵적으로 포함된 메소드. object를 String으로 변환. double, DateTime등을 String으로 바꿀때 유용.    
ex) double a; a.toString(), Datetime b; b.toString()    
(num.)toStringAsFixed(int) (num을 decimal뒤 숫자갯수를 지정해서 반올림하여 String으로 변환.)     

-*Dart syntax를 character로 명시할때*   
'나 $같은 charcter를 print할시 feature syntax로 인식하지 않으려면 \을 앞에 붙인다. ex) print(' \$ ');   

- *String interpolation $*   
$뒤의 변수를 String으로 변형. 만약 객체 내의 변수에 접근한다면 ${Abc.a}와 같이 {}로 묶어줘야한다.    

- *(List.)add()*    
List뒤에 원소 하나를 추가하는 메소드.    
tip: List가 final이어도 사용 가능.(final List a = []; 에서 a는 List객체의 포인터로 a값 변경 불가지만 객체 수정가능.)    

- *(List).where((var)->bool)*   
현 List의 특정조건을 만족하는 원소들만 iterable을 생성해 반환하는 메소드.   
인자로 (var)-> bool의 함수를 받음. 받은 함수의 argument에 기존 List의 각 원소 전달.   

- *List.generate(int, (index){})*   
List 클래스에 선언되있는 메소드. 특정 조건으로 List를 생성해 반환.   
첫번째 인자로 List의 길이, 두번째 인자로 (index){}함수를 받음. index는 List길이만큼 0부터 매 원소 생성시마다 1씩 증가해 함수에 전달.   
함수body에서 List의 각 원소로 들어갈 object반환.   

- *(double.)parse(String)*   
String을 받아 double로 변환해주는 메소드. 입력String이 double로 변환이 불가능하면 error출력.   

- *(object.)isEmpty*   
해당 오브젝트에 값이 있는지 확인.   

- *(String.)subString(i,j)*   
String [i..j)까지의 String을 반환.   

- *(List).fold(<T>first, (<T>sum, item)-> <T>)*   
 list의 각 원소를 순회하여 특정값을 계산해서 반환하는 메소드.   
 첫번째 인자로, 계산할 값의 초기값 지정. (일반적으로, totalSum을 구할때, 0으로 입력)     
두번째 인자로, 각 원소의 아이템으로 계산할 값을 수정하는 함수 입력. 입력할수 있는 함수의 조건은,   
계산할 값의 현재값(sum)과, 현재 사용할 List의 원소(item)을 전달받아, 함수body에서 새로 덮어씌울 계산할값을 return해야함.    
 
-------------------------------------------------------------------------------------------------------------
## Flutter  

- *pubspec.yaml*   
프로젝트의 set-up과 dependencies설정. 파일을 저장하면 자동으로 필요한 파일 및 패키지를 install & get.    
indent로 항목을 구분하기때문에 indent를 잘 지킬 것!    

- *pubspec.yaml: assets*
프로젝트에 사용할 font를 제외한 asset파일(이미지 등) 명시. 당연히, 모든 파일은 프로젝트 폴더 내에 저장되있어야함.     
assets: / 사용될 파일들 명시   
    -파일위치 및 이름    
  
- *pubspec.yaml: fonts*   
프로젝트에 사용할(다운받은) font를 명시. (당연히, assets/fonts/...등에 저장되있어야함.)    
pubspec.yaml에 font 영역 수정.   
fonts:   
   family: '사용할 폰트그룹명' / 한 폰트 그룹 (bold,regular..)의 이름(직접 입력)    
     fonts: / 그룹이 포함할 폰트 파일들을 명시.   
        assets: '파일위치 및 이름'   
        (해당 파일 폰트에 대한 정보를 추가. 다운받은 폰트의 info확인!    
        ex) 특정폰트의 bold 버전은 weight: 700 으로 지정해야 FontWeight.bold로 해당 패밀리의 bold버전 접근가능.)        

- *프로젝트 폴더변경시 hot reload,restart에 적용안됨. restart해야함*   
----------------------------------------------------------------------------------------------------------------

## Function, Class, Rules  

- *package:flutter/material.dart*   
flutter 여러 base위젯 및 함수 등이 포함된 flutter패키지   

- *runAPP()*    
widget인스턴스를 받아 build를 호출하여 실행해 화면에 띄어주는 함수. main()에서 메인widget실행   

- *Build(Buildcontext context)*   
위젯의 생성함수. 위젯(trees)을 필수로 return. 모든 위젯은 반드시 @override. Buildcontext는 메타정보를 담은 객체로, 자동 전달됨.   

- *State 클래스*     
Stateful위젯에 대응쌍관계인 class. extends State<StatefulWidget(대응하는 Statefulwidget 객체입력)>로 생성(generic)    
Stateful위젯이 re-build되어도 객체가 다시생성되지않음. data를 유지하며 Stateful위젯의 상태 저장.   
값이 변동되는 변수와 위젯의 build함수를 포함. Stateful위젯 첫 생성시뿐만 아니라 state가 변해 UI를 다시 표시할때도 build함수가 실행돼 위젯을 다시 생성.   
getter인 widget.()로 대응되는 stateful위젯의 멤버에 접근가능.    

- *Color & Colors*   
Color / 색을 표현하는 binary값을 가지는 class. 각 object는 특정색깔을 표현.  
Color.fromRGBO(r,g,b,opacity) / Color 객체를 r,g,b값으로 생성. 직접 색상의 rgb값 찾아서 생성할때 사용.      
Colors / 여러 Color객체을 static으로 선언해둔 utility class. 즉, 객체화 없이 Colors.black 등으로 여러색의 Color객체 사용 가능.    


- *rule: lifting state up*    
서로 다른 위젯에서 한 state를 변경,사용할떄, 그 state를 두 위젯의 부모 위젯에서 관리함. state는 가능한 higest level의 위젯에서 관리.   

- *rule: split the app into Widgets*   
위젯 trees 생성시 여러 logic이 포함된 complex한 큰 custom위젯을 분리하여 작은 sub-Widge(class) & file로 나누어 관리.      
main.dart는 깔끔한게 좋으며, 모든 위젯을 더 readable한 코드로 관리 가능. 한 코드에 나열하는것보다 실제 performance도 상승.(???)  

- *Widget List made by map method*    
리스트의 원소들로 한 종류의 위젯 리스트를 만들어 (Column같은 layout에) 이어붙이는 경우   
ex)  ...( qa['answers'] as List< String > ).map( ( answer ) { return Answer( _answerQuestion, answer );  } ).toList(),   
map method를 사용하여 질문의 선택지 목록(List< String >)로부터 각각의 String에 대한 버튼위젯 리스트(iterable)를 생성해 기존의 layout에 연결   
(qa: Map, 질문(key='question', val:String)과 선택지(key='answers', val:List)를 key로 가짐)   
(Answer: Custom 버튼 위젯, onpressed에 쓰일 함수포인터와 button에 표시할 string(answer)을 받음.) 

- *setState()*    
 state클래스 내에 존재. state내의 변수를 변경한 후 UI를 re-rendering하는 함수.   
 (build()/UI에 영향이 있는)state내의 변수를 변경하는 anonymous함수를 argument로 받음.   
 호출시 변경한 값에 맞는 현위젯 UI를 re-build. ex) setState( ( ) { idx++; } );      
(변경된 변수로 인해 위젯build내의 UI가 바뀌는 자식위젯만 re-build, Stateless위젯의 input data가 변경되면 해당위젯도 re-build)    
(build내의 변하지 않는 위젯은 re-paint. 새로 생성은 안하지만 다시 표시하는작업.)   

 - *Stateful/Stateless위잿*   
공통점: 생성되어 UI를 rendering. 외부에서 데이터를 받을수 있다.(생성자)   
차이점: stateless는 외부로부터의 인풋데이터가 변할시에만 widget을 re-rendering하고 stateful은 internal state가 변할때에도 re-rendering하여 현재 UI변경 가능(ex)setState를 통해).   

- *rule: use final for Stateless*   
Stateless내 (final이 아닌) 변수를 생성,변경가능. but. 객체 재생성이 아니면 UI 반영(rebuild)불가. 즉 관용적으로 모든 변수 final로 사용(stateless는 한번 생성해 보여주는위젯. EX)text )    

- *createState()*   
 Stateful은 반드시 대응 state클래스를 인스턴스화. @override createState()   
 ex) State<Statefulwidget>(= myApp) createState() => myApp(); (State객체를 return해야함!)   
 
 - *onPressed로 인자를 전달받는 함수를 실행할 때*   
onPressed로 인자를 전달받는 함수를 실행하는 버튼을 생성할시, 해당 함수를 실행하는 void형 anonymous function를 사용.   
ex) onPreesed : () => answerQuestion(answer['score']),   

- *@required*    
flutter가 제공하는 함수의 각 argument에 대한 decorator로 호출시 반드시 필요한 인자임을 명시. 없다면 호출불가.(error)     
'package:flutter/foundation.dart'에 명시됨.(혹은 'package:flutter/material.dart'도 포함)   

- *rule: styling*   
전부 basic 위젯의 argument로 처리. 특정 argument가 없는 위젯이라면 container같은 위젯으로 wrap하여 styling한다.   

- *showModalBottomSheet()*   
현 화면에서 다른 위젯을 modalBottomSheet으로 띄워주는 함수.   
@required context: BuildContext(현 화면에서의 메타정보를 전달해주어야함. 함수를 호출하는부분의 BuildContext를 전달)   
@required builder: (BuildContext)->Widget (띄울 위젯을 생성하는 함수, 이 함수의 argument로 modalSheet(?)의 context가 전달됨.)    
TextField를 modalBottomSheet으로 생성시 인풋이 다른 field로 넘어가면 사라짐. => TextField를 stateful로 ($$$$$)
TextField에서 onSubmitted 등으로 입력을 완료했을때 자동적으로 modalSheet가 꺼지게 하려면, Naviagor.of(context).pop(); 사용

- *ThemeData*    
Material App의 Theme에 대한 정보를 저장하는 클래스.    
primaryColor: Color / theme의 default로 쓰일 color. 다른 위젯에서 이 색상만 지정가능.    
primarySwatch: Color / theme의 한 색상을 여러 shade가 있는 그룹으로 사용. 위젯에서 theme색 참조시 여러버전으로 참조가능.     
accentColor: Color / 보색으로 쓰일 색상 지정. Material design Theme문서에 여러 조합 검색 가능.    
fontFamiliy: String / app의 global폰트family 지정     
textTheme: TextTheme / app의 text별 theme지정. TextTheme은 여러 text종류별 style을 저장하는 객체.   
(TextTheme(title: TextStyle, body1: TextStyle)처럼 여러 label별 textStyle을 theme으로 지정 가능. 다른 위젯에서 사용시 label로 참조.     
appBarTheme: AppBarTheme / appBar에 사용될 별도의 theme지정. appBarTheme정보를 나타내는 객체. textTheme등 지정 가능.       
ThemeData.light() / 모든 값이 default setting으로 이루어진 ThemeData 반환.      
여러 theme을 나타내는 객체는 (theme.)copyWith({})로 특정 named argument를 overwrite하여 그대로 사용 가능.    
ex) 기본 설정의 textTheme을 수정해서 사용할시, themeData.light().textTheme.copyWith(...)처럼 사용.   

- *BoxDecoration*   
container의 decoration 인자로 들어가는 decoration에 관한 정보를 표현한 클래스.
border : BoxBoarder / 일반적으로, 상속한 Boarder객체 입력. Boarder의 정보를 다음 class    
color: Color / 컨테이너의 background color지정.    
borderRadius: BorderRadiusGeometry / 일반적으로, 상속한 BorderRadius객체 입력. BorderRadius의 정보를 담은 class.    

- *Border*   
BoxDecoration의 border 인자로 들어가는 Boarder에 대한 정보를 표현한 클래스.   
Border.all() / Border의 named생성자. color, width등 지정 가능. border의 모든 방향으로 같은 값 적용.   
 
- *BorderRadius*   
BoxDecoration의 bolderRadius 인자로 들어가는 Border 꼭짓점부분의 곡면Radius에 관한 정보를 표현한 클래스.   
BorderRadius.circular(double) / BorderRadius의 named생성자, 반지름 값을 지정해 원형의 곡면 반지름 지정 가능.    

-------------------------------------------------------------------------------------------------------------------------
## Packages   
- *intl*   
help internationalization and localization facilities, including message translation, plurals and genders, date/number formatting and parsing, and bidirectional text.    
DateFormat.yMMd().format(dateTime) / DateTime객체를 pattern에 맞게 formatting하여 String으로 반환.    
DateFormat은 class로서 생성자 인자로 패턴(ex)yyyy-MM-dd)을 받거나 여러 pre-configured pattern을 named생성자(ex).yMMD())로 지정.   
DateFormat.E().format(dateTime) / DateTime을 요일만 표시(Mon,Tue,...).      
format은 class내부 메소드. date를 해당 패턴을 가진 String으로 반환.    


--------------------------------------------------------------------------------------------------------------------------
## Widgets   

- *MateriaApp(Cupertino)위젯*    
app을 Material theme으로 Setup하는 widget, named aurgments를 받아 인스턴스화.   
title:    
theme: ThemeData / app의 theme을 설정. Theme정보를 담은 클래스.    
(각 위젯들에서 app의 theme을 적용할때 Theme.of(context).(..)로 참조해서 쉽게 main Theme을 사용.)      


- *Scaffold(CupertinoPageScaffold)*   
페이지 Setup(스타일링) 위젯, 배경 색 등 지정 가능.      
appBar: preferredSizedWidget(ex)Appbar(...)) (화면 상단의 appBar위젯 지정)
body : Widget (appBar밑의 화면의 body부분에 표현될 위젯)   
floatingActionButtion : Widget(일반적으로, floatingActionButton)(body를 덮어 표시될 button, 버튼의 위치 default는 우측 하단)    
floatingActionButtonLocation: FloatingActionButtonLocation(상기 버튼의 위치 지정. floating버튼의 위치를 나타내는 객체.)    

- *AppBar*   
AppBar로 지정할수 있는 material(종속적?) 위젯.   
title: Widget(일반적으로, Text()) (AppBar에 표시될 타이틀 지정)   
actions: List<Widget> (title옆에 표시될 widget지정. 일반적으로, iconButtons을 사용. 혹은 popUpMenuButton)   

- *Column/Row*   
여러 위젯을 열/행으로 묶어 배치를 도와주는 위젯. Column(Row) 위젯의 좌우너비(높이)는 child의 너비(높이)중 가장 큰 값을 따름.    
column(Row)의 UI높이(너비)는 default로 double.infinity(가능한 최대). Column/Row는 겹치거나(row안의 row) mix해서 사용가능.   
? 부모column내의 여러 child와 같이 있는 자식column을 설정할 경우 높이는 자식column내의 children 높이와 fit하게 설정됨.?   
Column(Row)의 mainAxis는 top to bottom(left to right), crossAxis는 left to right(top to bottom)    
children:: list<widgets> (child위젯 입력)    
MainAxisAlignment: MainAxisAlignment, (column의 배정된 UI내 각child의 mainAxis상 배치형태를 결정하는 enum, default는 start)   
(MainAxisAlignment./  center:중앙, end: 밑, spaceAround :각 child위아래로 일정공간 추가)   
crossAxisAlignment: CrossAxisAlignment, (각 child의 corssAxis상에서의 배치형태를 결정하는 enum, default는 center)    
(CrossAxisAlignment./ end: 오른쪽끝, stretch: column의 너비만큼 늘여서 채워 배치.(child가 card등이면 너비를 define해줄수있음.))   
  
 
 -*Flexible/Expanded*   
   Column/Row의 child를 warp하여 위젯간 차지하는 공간 등 지정 가능.    
   
 - *RaisedButton위젯*   
버튼을 생성하는 위젯
child : Widget 버튼내부에 표현되는 위젯(text, image등) 입력   
onPressed 인자: 버튼 터치시 수행하는 함수(_void_)의 포인터 입력 null이면 button이 enabled.   

 - *FlatButton위젯*      
배경이 없는 버튼. 나머지는 동일.  

- *IconButton위젯*   
icon모양의 버튼.    
icon : Widget(버튼에 들어갈 icon모양 지정. Icon Widget을 받음.)   
onPressed : (){} (Listner, 다른 버튼과 동일)    

- *floatingActionButton*   
scafold의 floatingActionBUtton에 optimized된 버튼. body를 덮는 버튼.   
child: Widget(버튼이 감쌀 위젯. 일반적으로 Icon)   
onPressed: (){} (Listner, 다른 버튼과 동일)

- *Icon/Icons*    
Icon은 Icon의 정보를 포함하는 위젯. positional 인자로 IconData를 받음.   
Icons는 여러 icon의 IconData를 지정해놓은 class.   
즉, Icon(Icons.add)처럼 생성. 다른 argument로 색깔,사이즈 등을 지정가능, default값은 선택된 아이콘의 theme를 따라 결정.   

- *Text위젯*   
positional arguments로 String을 받음. named arguments로 여러 인자를 받음.      
style : TextStyle(fontSize: 28, fontWeight: Fontweight.bold) (문자의 style결정)   
(TextStyle은 문자Style의정보들을 담은 class(위젯x), Fontweight은 Font크기 값을 표현하고 static값을 묶어둔 utility class)    
textAlign: TextAlign.center (문자열의 배치결정. 단, text위젯이 차지하는 공간기준.) (TextAlign은 enum. center,left,right등 포함) //   
(tip: Text위젯은 UI에서 text크기만큼 공간을 할당받음. 즉, TextAlign.center로 화면 가운데 배치하고싶다면 Text를 Container에 담고 UI공간을 설정해 사용.)    

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


- *Container위젯*   
위젯을 담아 UI에 표현시 보이지않는 공간(Layout)관리 및 꾸밈효과를 돕는 위젯. named arguments를 받아 생성.   
child: Widget (감쌀 위젯),   
width: double (UI공간에서 할당받을 너비) (tip: double.infinity로 화면상 가능한 최대너비를 할당가능, 다른 UI가 안겹칠때까지),   
margin: EdgeInsetGeometry (boarder바깥쪽인 margin 설정)   
(> EdgeInset: 위젯의 margin길이 정보를 표현한 class. 여러 named constructor로 생성 가능. ex) EdgeInset.all(20) :모든방향 20)     
decoration: Decoration (일반적으로, 상속하는 BoxDecoration을 객체로 입력/boarder나 gardient등 위젯을 꾸미는 정보를 담은 클래스)   
Padding: EdgeInsetGeometry(boarder안쪽인 padding 설정)

-*Stack위젯*   
여러 위젯을 서로 위아래로 덮어서(3차원상에서) 표현할수 있게하는 위젯.   
children: [] / 포갤 위젯목록 지정. 첫번째 원소가 가장 아래에 배치.   


- *Card위젯*   
위젯을 담아 shadow를 주어 배치하는 content container위젯. UI공간은 default로 child크기만큼 할당. 
card의 부모 위젯이 child(card)의 UI공간을 define한다면 부모 위젯을 따름.   
즉, parent로 container같은 위젯을 써 할당하거나, child의 UI크기를 변경.      
child : Widget(감쌀위젯)   
elevation: double (shadow의 세기 조절)     
color: Color(background color지정)   
margin: EdgeInsetsGeometry (위젯 margin지정)   

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
scrollaDirection: Axis(column/row 지정(scroll방향)(default: vertical, column))   
itemBuilder: (BuildContext, int(index번호))->Widget (ListView의 새로운 아이템을 생성해야할때마다 호출하는 위젯 생성 함수 명시.)   
(자동적으로 buildcontext(메타정보)와 int(index,몇번째 아이템인지)를 선언한 함수에 인자로 전달해줌.)   
(함수에선 아이템으로 표시할 위젯을 반환하도록 작성. index를 이용해 각종 정보에 접근하여 생성가능.)      
itemCount: int (현재 ListView에 포함할 아이템의 갯수 명시.)   

- *GridView*   
scrollable + grid. grid형으로 위젯 배치   

- *ListTile*   

- *GestureDetector*    
감싼 위젯에 발생하는 User input을 제어하기 위한 위젯.   
behavior: HitTestBehavior (특정 동작을 제어하기위해 필수?)

- *InkWell*   

- *dedicated padding()*

- *Image*     
Image파일을 띄어주는 위젯. Image의 출처에 따라 여러 named constructor로 생성.   
(Image.)asset('파일경로 및 이름', fit: BoxFit.cover) / pubspec.yaml에 명시된 asset 내의 파일인 경우.   
(Image.)network() / 웹상의 이미지를 가져올 경우.   
(Image.)file() / file에서 stream을 가져오는 경우?    
각 constructor내의 name argument   
fit: BoxFit / 이미지가 (크기가 define된 부모위젯)Box내에서 size를 어떻게 fit할지 지정. BoxFit은 여러 값을 지정한 enum.    

- *sizedBox*   
특정 크기의 layout을 줄때 사용하는 위젯. child를 가질수 있지만(container처럼 작동), child없이 크기를 줘서 공백을 줄때 자주 사용.   
height: double   

- *FractionallySizedBox*   
부모위젯의 크기에 대한 비율로 전체 위젯의 크기를 지정해줄때 사용하는 위젯.     
child: Widget / Box가 감쌀 위젯. 이 위젯의 크기를 비율에 맞게 생성.   
heightFactor: 0~1 / 부모위젯의 크기에 대한 비율 지정. 1이면 부모위젯 전체공간 차지.   
