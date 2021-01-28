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

- *ctrl + . : 빠른 수정(위젯을 다른 위젯으로 감싸거나 custom 위젯으로 추출할때)*    

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
 관용적으로 1위젯당 1파일(무조건 같이쓰는 위젯끼리는 1파일에 묶어도 괜찮음.)쓰는게 권장.   
 복잡합 위젯일수록 한 파일&클래스로 묶어 한 위젯으로 만들어 관리하는게 유리.   

- *secreen상 노란색bar error*   
위젯 UI가 빠져나옴가 devie의 page boundary를 침범했음을 의미. UI설정을 수정해야함.     

- *_*    
관용적으로, 함수 호출시 인자로 받지만, 사용안할 변수의 이름으로 _ 사용.   

- *good code*    
readable / split widgets, use builder method(긴 코드의 자식위젯을 생성하는 함수를 클래스내에 따로 선언해 build에서 호출, 비슷한 위젯을 여러번 만들때도 유용)                   
performance / use const widget,

- *split Widgets vs use Builder method* (정확히 예를들면?)        
한 위젯 내에서 긴 코드의 자식 위젯을 분리하거나, 클래스 내에 builder method를 쓰는 것은 큰 차이는 없음.    
다만, 만일, 해당 자식 위젯이, state나 theme,mediaQuery 등을 변경해, 다시 build된다면, 해당 위젯만 다시 build되는(부모 위젯 build는 호출하지않음.)    
split Widgets가 builder함수 사용보다 좀더 optimize가능함.    

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
getter // 클래스 내에서, 변수를 이용해 특정 값들을 도출해 반환할시. (ex) enum변수를 적절한 String으로 변환할때 등)     


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
날짜,시간을 저장할수있는 dart의 buil-in class. DateTime(year,hours, ..)처럼 생성. positional argument로 날짜 입력.   
years를 제외한 인자는 defautl값(1) 존재. 즉, DateTime(2021)처럼 생성시 해당 년도의 1월 1일 00시 .. 로 초기화.    
DateTime.now() : 현재 timestamp로 DateTime객체 생성하는 생성자.   
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
인자로 (var)-> bool의 함수를 받음. 받은 함수의 argument(var)에 기존 List의 각 원소 전달. 이 함수의 return값이 true인 원소들로만 채움.     

- *(List.)firstWhere((var)->bool)*    
현 List의 원소들 중 특정 조건을 만족하는 순서상에서 첫 원소를 반환하는 메소드.     
전체원소 중 특정 조건을 만족하는 여러 원소 중 하나만 찾거나, 딱 하나의 특정 원소만 찾을 때 유용       
(var)->bool의 함수를 받음. 함수의 argument로 각 원소 전달. 해당 함수의 return값이 true인 첫 원소를 반환.    

- *(LIst.)indexWhere((var)->bool)*    
List의 원소들 중 특정 조건을 만족하는 순서상 첫 원소의 index를 반환하는 메소드. 만족하는 원소가 없다면 -1반환.       

- *(List).contains(Object)*     
현 리스트내에 특정 원소(Object)가 있는지 확인해주는 bool 반환 메소드.     
리스트 내에 인자로 전달한 Object와 같은 객체가 있다면 true반환. 없으면 false반환.    

- *(List.)removeWhere((var)->bool)*   
현 리스트의 원소들중 특정 조건을 만족하는 원소를 지우는 메소드(남은 원소들은 다시 첫원소부터 인접하게 채워짐.)    
인자로 (va)->bool의 함수를 받음. 받은 함수의 argument에 List의 각 원소 전달. 함수 body에서 return하는 값이 true인 원소들을 지움.   

- *(List).removeAt(int)*    
List의 특정 index의 원소 삭제. List의 길이는 1 감소하고, 빈 자리는 자동적으로 채워짐.    

- *(List.)any((var)->bool)*     
List내에 특정 조건을 만족하는 원소가 하나라도 있을시, true반환. 한개도 없다면 false를반환하는 bool메소드.    

- *List.generate(int, (index){})*   
List 클래스에 선언되있는 메소드. 특정 조건으로 List를 생성해 반환.   
첫번째 인자로 List의 길이, 두번째 인자로 (index){}함수를 받음. index는 List길이만큼 0부터 매 원소 생성시마다 1씩 증가해 함수에 전달.   
함수body에서 List의 각 원소로 들어갈 object반환.   

- *(List).reversed*     
현 List의 원소 순서를 뒤집어 iterable로 반환. List로 변환하려면 reversed.toList() 사용.        

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
 
 - *Future<T>*    
 <T>형 객체를 비동기적으로 저장하는 객체. 해당 변수가 저장되길 기다린다.
  (Future.)then((var) {}) / 변수가 저장될때, 실행하는 함수 선언. 실행 함수의 인자로 저장된 객체를 전달    
 then? wait? await?    
  
  - *if in List*   
  [if(a>1) B, C, ]처럼  if문이 성립할시 직후(괄호{}는 쓰지 않음.)의 원소를 포함하도록 선언 가능.    
  (flutter에서 children내의 특정 위젯을 포함시킬지 결정하는데 유용.)      
  
  - *mixin*    
  한 클래스에서 다른 클래스를 상속하지않고, 클래스의 몇가지 feature를 사용하고자할때, with키워들 사용.    
  클래스명과 상속한 클래스명 뒤에 with (...)와 같이 명시.    
  
  - *Random*     
  random수를 생성할수있게 돕는 클래스. 특정 조건으로 난수를 생성. import 'dart:math' 필요.       
  Random().nextInt(k) / 0~k-1범위의 정수를 하나 생성해 반환하는 함수.    
  
  
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
        ex) 특정폰트의 bold 버전은 weight: 700 으로 지정해야 fontWeight: FontWeight.bold로 해당 패밀리의 bold버전 접근(지정된게 없다면?)가능.)        

- *프로젝트 폴더변경시 hot reload,restart에 적용안됨. restart해야함*   

- *portrait mode만 지원하고싶은 경우*    
'package:flutter/services.dart'를 import한 후,
WidgetsFlutterBinding.ensureInitialized();
SystemChrome.setPreferredOrientations([DeviceOrientation.portraitUp, DeviceOrientation.portraitDown,]);   
(WidgetsFlutterBinding.ensureInitialized()은 다음줄인 Orientation설정 함수가 정상적으로 작동하도록 설정.)   
(SystemCrhome은 system wide한 설정 가능하도록 하는 클래스, setPreferredOrientation()으로 앱의 지원 mode 설정.      
mode목록을 List로 전달.  DeviceOrientation은 여러 모드를 지정가능한 enum.)

- *adaptive by flutter*     
특정 위젯들은 OS에 맞는 style로 adaptive하게 생성될 수 있도록, (widget.)adaptive() 형태로 특별한 named 생성자 제공.    
해당 위젯이 build되는 os에 맞게끔 style이 설정되어 rendering된다. os에 맞게끔 생성하되 특정 부분만 메인 색상 등으로    
별도로 지정하는 방법도 가능. 가능한 위젯 목록 : switch,      

- *OS != Theme*   
특정 Theme이 해당 os에서 자주쓰이는 style일 순 있어도, 반드시 IOS에서는 Cupertino 위젯을 써야한다는 것은 아님.    
말그대로 style이기 때문에, theme은 optional. IOS사용자에게 cupertino가 익숙한 style인 것은 사실.       

- *Platform 참조*    
dart가 제공하는 'dart:io'를 import하면, 해당 파일이 실행되는 os를 참조할수있는 객체인 Platform에 접근가능.   
(Platform.)isIOS/isAndroid 등으로 현재 os가 IOS인지 확인 가능. 해당 값들은 bool.        

- *변수에 Widget을 ternary expression으로 저장시 참조문제*    
상황에 따라 다른 위젯을 렌더링하기 위해 변수에 위젯을 ternary expression으로 선언할 시, 같은 위젯이 아니라면    
변수 type을 Widget으로 인식. 즉, 두 위젯이 비슷한 위젯으로 공통된 property를 갖더라도 해당 property는 변수를 통해 참조 불가능.    
이런 경우, 변수의 type을 explicitly 명시해준다.   
ex) final PreferredSizeWidget appBar = Platform.isIOS? CupertinoNavigationBar() : AppBar(); / appBar.preferredSized 참조가능.   

- *material/Cupertino theme 종속성*   
material 기반으로 작성된 위젯중 일부는 다른 material 위젯만을 조상 위젯으로 가질 수 있음.    
ex) IconButton은 mateiral디자인으로, cupertino디자인 위젯에서 쓰려면 custon Icon Button을 생성해야함.      
ex) 반대로, 강제성이 없는 일부 Cupertino위젯을 MaterialApp+ Scaffold에서 사용 가능.   

- *adaptive한 위젯들은 custom으로 관리해 code 간결화*    
adaptive하게 비슷한 위젯을 여러번 사용할시 매번 ternary expression쓰는 것보다 adaptive custom 위젯 클래스를 만들어 사용.     
ex) theme에 맞는 flat Button 생성시 매번 os확인하여 생성하는 코드를 복사하는 것보단, adaptive_flat_button클래스를 만들어     
위젯 내에서 OS를 확인하고 필요한 정보를 받아 버튼을 렌더링하는 것이 코드관리에 효율적.   

- *rebuilding Widget tree는 비효율적이지 않음. How flutter is.*          
Widget tree는 단순히 configuration tree. render object를 전부 rebuild하는것은 inefficien하지만     
Widet tree을 rebuild하는 것은 괜찮음. render object는 를 통해 바뀐 widget을 확인해 rendering 중 바뀐 부분만 수정.    
그래도, 큰 app에서는 최대한 바뀌는 부분의 위젯 트리만 rebulid하도록 위젯들을 적당히 split하는게 좋음.    

- *const Widget*    
Widget tree의 모든 위젯은 property가 전부 final이라면 const 생성자를 붙일수 있음. (정확히는 dart의 featurue)    
(ex) class Abc{ final a; final b; const Abc(this.a,this.b);}) (위젯의 경우 생성이후 변하지 않는 immutable이라는 의미)    
또한, const 생성자를 가진 클래스는 const 객체를 생성할 수있음. (단, 생성자에 전달할 argument가 dynamic이 아닌 const인 경우.)     
즉, widget도 const로 생성할 수 있는데, 이런 경우 해당 위젯을 생성하는 부모 위젯가 re-build되더라도, 해당 위젯은 re-build되지 않음.    
(위젯트리에서 새로 instantiation되지 않음.) 즉, 큰 performance향상은 아니지만, 큰 app에서 사소한 차이는 보일 수 있기때문에,        
const로 생성할 수있는 위젯/객체는 const로 생성하는 습관 추천. app을 완성하고 추가하는 방식으로.       

- *Widget state(lifecycle)*    
Stateful위젯은 생성자를 통해 첫 생성시, state객체를 생성 -> state객체에서 initState()를 호출 후, build()      
-> setState(), build()를 호출하며 위젯 유지. 이때, stateful위젯이 다시 생성될때마다 state에서 didUpdateWidget()호출 후, build()     
-> Stateful위젯이 변경이 아닌 소멸될때,state의 dispose() 호출.
(initState, didUpdateWidget, dispose()를 override하여 사용시, 함수내에서 super.initState()를 선언해 상속한 state객체의 동작을    
명시해줘야함.)

- *app lifecycle*     
app의 state에는 inactive, paused, resumed, suspending존재.      
app으 state를 확인하고자 할땐, stateful 위젯의(main app이 아니라도?) state객체에서 with WidgetsBindingObserver를 mixin한 후,    
클래스 내에서 listner인 didChangeAppLifecycleChange(AppLifeCycleState)를 override. 해당 함수는 app의 state가 변할때마다 호출되고,     
변화된 state에 대한 정보를 포함한 객체를 인자로 전달받음. 위 Listner를 stateful위젯에서 동작하게하기위해, stateful위젯의 생성과 소멸시,    
즉 initState(), dispose()에서 각각 해당 위젯에 attach된 listner를 생성과 소멸하도록 선언해야함. (memory leak방지)         
생성: WidgetsBinding.instance.addObserver(this); 소멸: WidgetsBinding.instance.removeObserver(this);     
(stateful위젯이 생존해있는 동안 app State가 변할때마다 이 위젯 내의 didChange..함수를 호출하도록 명시.)    

- *list & stateful item*     
list의 각 item이 stateful위젯인 경우, 한 item을 삭제시, 해당 item에 대응되는 state가 삭제되지않고, 다음 item에 attach되는 issue존재.     
그 이유는, item이 삭제되고 위젯트리가 변경된 후, element tree를 다시 확인하는 과정에서, element는 대응되는 위젯을 type으로 인식함.     
이때 List의 item이 삭제되면 다음 item이 자동으로 위부터 채워지므로, 다음 stateful item이 지워진 item의 (state attached)element로 인식.    
해결 방법: key이용. element는 key가있다면, 대응되는 위젯을 타입+key로 인식.
ex) item인 stateful위젯의 생성자로 unique한 key전달. 이때, ListView의 builder로 item생성하지 말고(버그?), map이용해 생성.    
Abc({..., Key key}): super(key: key) 
item에 직접 해당하는 topmost의 stateful위젯의 생성자에 전달받아 상속한 부모위젯인 stateful위젯에 key값을 명시.      
(: super(..) 처럼 상속 부모클래스의 초기화를 특정인자를 명시하는 것을 initializer list라고 함. 주로, key쓸때만 사용)      
이때, 매 item생성시, UniqueKey()를 쓰면, item목록이 변하지않더라도 ListView가 갱신될때마다,(ListView의 조상위젯이 rebuild되는 등)      
각 item에 다시 새로운 key를 입력하므로, element들이 기존에 대응되는 위젯을 찾지못해, 새로운 위젯을 매번 다시 생성.       
따라서, ValueKey(..)를 씀. ValueKey의 인자로는 unique한 key인 id등을 쓰면 유용.     
 
 - *pages as a stack*    
 page들은 stack으로 관리됨. topmost Page가 화면에 표시. stack은 현 페이지에서 push(), pop() 등으로 관리.          
push, pop은 navigator의 도움을 받아 사용. 스택이 쌓여있다면, topmost페이지에 뒤로가기버튼 자동생성.(버튼은 단순 Navigator.of(context).pop()실행)          

- *initState & context*    
staeful위젯의 state를 처음 생성시 필요한 변수의 초기화를 initState내에서 진행하는 경우.    
(생성자가 아니라, initState에서 초기화할 필요가 있는 경우 중에, 예를들어, modalRoute.of(context)로 argument를 받아 스크린 생성하는데,    
argument로 state내의 변수를 build함수 내에서 초기화하지않고(stateful이라서), 첫 위젯 생성시에 한번만 초기화하고 싶은 경우)    
해당 초기화에 위젯의 context값을 사용한다면, error발생. 그 이유는, initState가 state위젯이 fully create되기 전(context가 생성되기 전)에 실행되기 떄문.     
(initState에서 다른 일반 property나 widget(대응stateful객체)들은 사용가능. context는 아직 미생성.)          
해결법: didChangeDependencies에서 초기화    
state처음 생성시 필요하나 변수를 initState가 아닌 didChangeDependencies에서 초기화(모든 변수(context포함)가 초기화되었을때마다 호출되므로).      
단, didChangeDependencies는 매 state dependency가 변할때마다 호출되므로, state의 생존 동안 초기화를 매번 진행하지않기 위해,    
bool _loadedInitData =false; 와 같이 bool면수를 사용. didChangeDependencies내에서 _loadedInitData가 false일때만 초기화를 진행하고 bool값을 true로 바꾸고,    
그 이후에 다시 호출되더라도, bool값이 true이므로 초기화를 진행하지않도록 구현.       

- *didChangeDependencies*    
sateful위젯의 state객체의 dependency(reference)가 변할때마다 호출되는 함수. state내 모든 위젯 및 변수가 처음 initialized되었을 때도 호출.    


----------------------------------------------------------------------------------------------------------------

## Function, Class, Rules  

- *package:flutter/material.dart*    
flutter 여러 base위젯 및 함수 등이 포함된 flutter패키지   

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
@required context: BuildContext(현 클래스의 메타정보를 전달해주어야함. 함수를 호출하는부분(?)의 BuildContext를 전달)   
@required builder: (BuildContext)->Widget (띄울 위젯을 생성하는 함수, 이 함수의 argument로 modalSheet(?)의 context가 전달됨.)    
TextField를 modalBottomSheet으로 생성시 인풋이 다른 field로 넘어가면 사라짐. => TextField를 stateful로 ($$$$$)
TextField에서 onSubmitted 등으로 입력을 완료했을때 자동적으로 modalSheet가 꺼지게 하려면, Naviagor.of(context).pop(); 으로 sheet탈출.       
ModalSheet이 가지는 maxHeight존재.(tip: 위젯의 크기가 modalSheet의 크기보다 커지는 경우 scrollable하게, 위젯을 scrollView로 선언가능.)    

- *ThemeData*    
Material App의 Theme에 대한 정보를 저장하는 클래스. material Theme에 지정시 다른 위젯에서 Theme.of(context).(label)로 참조해서 사용.          
primaryColor: Color / theme의 default로 쓰일 color. 다른 위젯에서 이 색상만 지정가능.    
primarySwatch: Color / theme의 한 색상을 여러 shade가 있는 그룹으로 사용. 위젯에서 theme색 참조시 여러버전으로 참조가능.     
accentColor: Color / 보색으로 쓰일 색상 지정. Material design Theme문서에 여러 조합 검색 가능.    
canvasColor: Color /    
errorColor: Color / 에러에 쓰일 색상 지정. default는 red.   
fontFamiliy: String / app의 global폰트family 지정(text의 default폰트?)     
textTheme: TextTheme / app의 text별 theme지정. TextTheme은 여러 text종류별 style을 저장하는 객체.   
(TextTheme(title: TextStyle, body1: TextStyle)처럼 여러 label별 textStyle을 theme으로 지정 가능. 다른 위젯에서 사용시 label로 참조.   
style: Theme.of(context).textTheme.title) 혹은 color : Theme.of(context).textTheme.button.color 와 같이)    
appBarTheme: AppBarTheme / appBar에 사용될 별도의 theme지정. appBarTheme정보를 나타내는 객체. textTheme등 지정 가능.       
ThemeData.light() / 모든 값이 default setting으로 이루어진 ThemeData 반환.      
여러 theme을 나타내는 객체는 (theme.)copyWith({})로 특정 named argument를 overwrite하여 그대로 사용 가능.    
ex) 기본 설정의 textTheme을 수정해서 사용할시, themeData.light().textTheme.copyWith(...)처럼 사용.   

- *BoxDecoration*   
container의 decoration 인자로 들어가는 decoration에 관한 정보를 표현한 클래스.
border : BoxBoarder / 일반적으로, 상속한 Boarder객체 입력. Boarder의 정보를 다음 class    
color: Color / 컨테이너의 background color지정.    
borderRadius: BorderRadiusGeometry / 일반적으로, BorderRadius객체 입력. BorderRadius의 정보를 담은 class.   
(BorderRadius.circular(double), BorderRadius.all(), BorderRadius.vertical(), ...)    
shape: BoxShape / 컨테이너의 모양 결정 BoxShape은 enum으로 circle, rectangle(default)등 가능.    
gradient: Gradient / background를 gradient있는 색상으로 지정. 

- *Gradient*    
gradient를 표현한 객체. 상속하는 여러 class존재.
LinearGradient( // linear한 gradient생성.    
colors: List<Color> // gradient하게 표현될 색상들 지정.    
begin: AlignmentGeometry // 시작 색상 위치 지정. 일반적으로, Alignment객체 사용.    
end: AlignmentGeomtery // 마지막 색상 위치 지정. Alignment는 여러 static값도 저장되있는 클래스로, Alignment.topLeft 등을 포함.    
 
)    

- *Border*   
BoxDecoration의 border 인자로 들어가는 Boarder에 대한 정보를 표현한 클래스.   
Border.all() / Border의 named생성자. color, width등 지정 가능. border의 모든 방향으로 같은 값 적용.   
 
- *BorderRadius*   
BoxDecoration의 bolderRadius 인자 등으로 들어가는 Border 꼭짓점부분의 곡면Radius에 관한 정보를 표현한 클래스.   
BorderRadius.circular(double) / BorderRadius의 named생성자, 반지름 값을 지정해 원형의 곡면 반지름 지정 가능.      
BorderRadius.all() //

- *showDatePicker() -> future<DateTime>*    
현재 화면에서 달력의 날짜를 선택할수 있는 overlay창(datePicker)을 띄워주는 flutter내의 함수. (다른 package?)       
입력을 받기위해 대기하며 입력을 받으면 저장하는 future객체를 반환. 즉, 입력을 저장할 객체(d)를 하나 선언해두고,    
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
(Navigator.of(ctx).)push(Route) // 현 스크린에서 다른 페이지를 생성해 페이지 스택에 추가. Route객체를 받음.     
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
fullScreenDialog: bool // 페이지를 디폴트값인 slide 애니메이션으로 불러올지, 전체화면에 바로 띄울지를 결정.     
ex)MaterialPageRoute(builder: (ctx) { return CategoryMealsScreen(id,title); }, ),
(불러올 새로운 페이지의 생성자를 통해 데이터도 전달 가능.)    

- *ShapeBorder*    
card의 shape인자로 들어가는 Border의 shape에 대한 정보를 표현한 클래스.    
RoundedRectangleBorder( // 모서리가 둥근 shape을 나타내는 일종의 shapeBorder클래스.     
borderRadius : BorderRadiusGeometry // 각 모서리의 반지름 지정.    
),     


-------------------------------------------------------------------------------------------------------------------------
## Packages   
- *intl*   
help internationalization and localization facilities, including message translation, plurals and genders, date/number formatting and parsing, and bidirectional text.    
DateFormat.yMMd().format(dateTime) / DateTime객체를 pattern에 맞게 formatting하여 String으로 반환.    
DateFormat은 class로서 생성자 인자로 패턴(ex)yyyy-MM-dd)을 받거나 여러 pre-configured pattern을 named생성자(ex).yMMD())로 지정.   
DateFormat.E().format(dateTime) / DateTime을 요일만 표시(Mon,Tue,...).      
format은 class내부 메소드. date를 해당 패턴을 가진 String으로 반환.    


--------------------------------------------------------------------------------------------------------------------------

