# 요약
 __Flutter/Dart__ 를 공부하며 개념들을 정리한 문서입니다. 이전까지 OOP언어인 C, C++, JAVA 등을 다뤄보았고, java를 기반으로한 안드로이드 네이티브앱 개발 경험이 있습니다. 따라서, Flutter/Dart만의 특징 위주로, 실제 개발시 _주관적으로_ 필요한 개념 위주로 정리해 작성한 문서입니다.          
      
## Concept
__1. UI as code (Widget)__   
__2. CrossFlatform__   
__3. Dart(OOP)__   
   
## Dart
- *arrow function*    
한줄 함수(함수의 명령이 한줄)이면 =>로 표현가능. ex) void main() => runApp(myApp());  ( 반환값이 있다면 결과값을 return )    

- *anonymous function*    
익명함수, 재사용하지 않는 함수에 활용. ( arguments ) { body }로 표현가능. ex) (value) { print(value); };    

- *final/const*   
final: runtime시에 initialized된 이후 변경x (run time constant)   
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


## Flutter (추후: widget, class, rule 등 구분해서 나눌것!)    
- *runAPP()*    
widget인스턴스를 받아 build를 호출하여 실행해 화면에 띄어주는 함수. main()에서 메인widget실행   

- *Build(Buildcontext context)*   
위젯의 생성함수. 위젯(trees)을 필수로 return. 모든 위젯은 반드시 @override. Buildcontext는 메타정보를 담은 객체로, 자동 전달됨.   

- *MateriaApp위젯*    
Material theme의 widget클래스, named aurgments를 받아 인스턴스화.   

- *Scaffold*   
기본페이지구성(스타일링) 위젯   

- *Column/Row*   
여러 위젯을 열/행으로 묶은 Layout위젯, children 인자: list<widgets>형태로 여러 위젯을 입력받음 (children: [ ... ],)   
 
 - *RaisedButton위젯*   
버튼을 생성하는 위젯
child : Widget 버튼내부에 표현되는 위젯(text, image등) 입력   
onPressed 인자: 버튼 터치시 수행하는 함수(_void_)의 포인터 입력 null이면 button이 enabled.   

 - *FlatButton위젯*      
배경이 없는 버튼. 나머지는 동일.  

 - *onPressed로 인자를 전달받는 함수를 실행*   
onPressed로 인자를 전달받는 함수를 실행하는 버튼을 생성할시, 해당 함수를 실행하는 void형 anonymous function를 사용.   
ex) onPreesed : () => answerQuestion(answer['score']),


 - *Stateful/Stateless위젯*   
공통점: 생성되어 UI를 rendering. 외부에서 데이터를 받을수 있다.(생성자)   
차이점: stateless는 외부로부터의 인풋데이터가 변할시에만 widget을 re-rendering하고 stateful은 internal state가 변할때에도 re-rendering하여 현재 UI변경 가능(ex)setState를 통해).   

- *State 클래스*     
 attatched: Stateful위젯에 대응쌍관계인 class   
 generic: extends State<StatefulWidget(대응하는 Statefulwidget 객체입력)>로 생성   
 persistent: Stateful위젯이 re-build되어도 객체가 다시생성되지않음. data를 유지하며 Stateful위젯의 상태 저장.   
 role: 값이 변동되는 변수와 위젯의 build함수를 포함. Stateful위젯 첫 생성시뿐만 아니라 state가 변해 UI를 다시 표시할때도 build함수가 실행돼 위젯을 다시 생성.   

- *setState()*    
 state클래스 내에 존재. state내의 변수를 변경한 후 UI를 re-rendering하는 함수.   
 (build()/UI에 영향이 있는)state내의 변수를 변경하는 anonymous함수를 argument로 받음.   
 호출시 변경한 값에 맞는 현위젯 UI를 re-build. ex) setState( ( ) { idx++; } );      
(변경된 변수로 인해 위젯build내의 UI가 바뀌는 자식위젯만 re-build, Stateless위젯의 input data가 변경되면 해당위젯도 re-build)    

- *rule: use final for Stateless*   
Stateless내 (final이 아닌) 변수를 생성,변경가능. but. 객체 재생성이 아니면 UI 반영(rebuild)불가. 즉 관용적으로 모든 변수 final로 사용(stateless는 한번 생성해 보여주는위젯. EX)text )    

- *createState()*   
 Stateful은 반드시 대응 state클래스를 인스턴스화. @override createState()   
 ex) State<Statefulwidget>(= myApp) createState() => myApp(); (State객체를 return해야함!)   

- *Text위젯*   
positional arguments로 String을 받음. named arguments로 여러 인자를 받음.      
style : TextStyle(fontSize: 28, fontWeight: Fontweight.bold) (문자의 style결정)   
(TextStyle은 문자Style의정보들을 담은 class(위젯x), Fontweight은 Font크기 값을 표현하고 static값을 묶어둔 utility class)   
textAlign: TextAlign.center (문자열의 배치결정. 단, text위젯이 차지하는 공간기준.) (TextAlign은 enum. center,left,right등 포함)   
(tip: Text위젯은 UI에서 text크기만큼 공간을 할당받음. 즉, TextAlign.center로 화면 가운데 배치하고싶다면 Text를 Container에 담고 UI공간을 설정해 사용.)   

- *Container위젯*   
위젯을 담아 UI에 표현시 보이지않는 공간(Layout)에 대한 관리를 돕는 위젯. named arguments를 받아 생성.   
child: Widget (감쌀 위젯),   
width: double (UI공간에서 할당받을 너비) (tip: double.infinity로 화면너비전체를 할당가능),   
margin: EdgeInsetGeometry (주변 margin의 길이)   
( EdgeInset: container의 margin정보를 표현한 class. 여러 named constructor로 생성 가능. ex) EdgeInset.all(20) :모든방향 20)     

- *Color & Colors*   
Color: 색을 표현하는 binary값을 가지는 class. 각 object는 특정색깔을 표현.  
Colors: 여러 Color객체을 static으로 선언해둔 utility class. 즉, 객체화 없이 Colors.black 등으로 여러색의 Color객체 사용 가능.    

- *rule: lifting state up*    
서로 다른 위젯에서 한 state를 변경,사용할떄, 그 state를 두 위젯의 부모 위젯에서 관리함. state는 가능한 higest level의 위젯에서 관리.   

- *Widget List made by map method*    
리스트의 원소들로 한 종류의 위젯 리스트를 만들어 (Column같은 layout에) 이어붙이는 경우   
ex)  ...( qa['answers'] as List< String > ).map( ( answer ) { return Answer( _answerQuestion, answer );  } ).toList(),   
map method를 사용하여 질문의 선택지 목록(List< String >)로부터 각각의 String에 대한 버튼위젯 리스트(iterable)를 생성해 기존의 layout에 연결   
(qa: Map, 질문(key='question', val:String)과 선택지(key='answers', val:List)를 key로 가짐)   
(Answer: Custom 버튼 위젯, onpressed에 쓰일 함수포인터와 button에 표시할 string(answer)을 받음.)   

- *Center위젯*   
자식위젯을 자신이 차지하는 공간 가운데에 배치하는 layout위젯. named arguments를 받아 생성.   
child : Widget (감쌀 위젯)   
heightFactor : double (자식위젯의 높이에 대한 위젯의 높이 비율, null일 경우 화면에서 차지할수있는 만큼할당)   
widthFactor : double (자식위젯의 너비에 대한 위젯의 너비 비율, null일 경우 화면에서 차질할수있을만큼 할당)   

- *rule: split the app into Widgets*   
위젯 trees 생성시 여러 logic이 포함된 complex한 큰 custom위젯을 분리하여 작은 sub-Widge(class) & file로 나누어 관리.      
main.dart는 깔끔한게 좋으며, 모든 위젯을 더 readable한 코드로 관리 가능. 한 코드에 나열하는것보다 실제 performance도 상승.(???)   

