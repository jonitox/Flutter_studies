## Features & issues

- *OS != Theme*   
특정 Theme이 해당 os에서 자주쓰이는 style일 순 있어도, 반드시 IOS에서는 Cupertino 위젯을 써야한다는 것은 아님.    
flutter가 내부적으로 대응되는 native코드르 생성하는게 아닌 UI를 직접 rendering하기때문.    
즉,말그대로 style이기 때문에, theme은 optional. 예를들어, IOS사용자에게 cupertino가 익숙한 style인 것은 맞음.     

- *material/Cupertino theme 종속성*   
material 기반으로 작성된 위젯중 일부는 다른 material 위젯만을 조상 위젯으로 가질 수 있음.    
ex) IconButton은 mateiral기반 위젯으로, cupertino 위젯에서 쓰려면 custom Icon Button을 생성해야함.      
반대로, 강제성이 없는 일부 Cupertino위젯을 MaterialApp + Scaffold에서 사용 가능(단순, style차이).   

- *ternary expression으로 위젯을 변수에 저장시 type 감지오류*    
상황에 따라 다른 위젯을 렌더링하기 위해 변수에 위젯을 ternary expression으로 선언할 시, 같은 위젯이 아니라면    
변수 type을 Widget으로 인식. 즉, 두 위젯이 비슷한 위젯으로 공통된 property를 갖더라도 해당 property는 변수를 통해 참조 불가능.    
이런 경우, 변수의 공통된 type을 explicitly 명시해준다.   
ex) final PreferredSizeWidget appBar = Platform.isIOS? CupertinoNavigationBar() : AppBar(); / appBar.preferredSized 참조가능.    

- *rule: lifting state up*    
서로 다른 위젯에서 한 state를 변경,사용할떄, 그 state를 두 위젯의 부모 위젯에서 관리함. state는 가능한 higest level의 위젯에서 관리.   

- *rule: split the app into Widgets*   
위젯 trees 생성시 여러 logic이 포함된 complex한 큰 custom위젯을 분리하여 작은 sub-Widge(class) & file로 나누어 관리.      
main.dart을 lean하게 관리, 모든 위젯을 더 readable한 코드로 관리 가능. 한 코드에 나열하는것보다 실제 performance 상승 가능.(빌드할부분 분리)       

- *rule: use final for Stateless*   
Stateless내 (final이 아닌) 변수를 생성,변경가능. but. 객체 재생성이 아니면 UI 반영(rebuild)불가. 즉 관용적으로 모든 변수 final로 사용(stateless는 한번 생성해 보여주는위젯. EX)text )    

- *Widget List made by map method*    
리스트의 원소들로 한 종류의 위젯 리스트를 만들어 (Column같은 layout에) 이어붙이는 경우   
ex)  ...( qa['answers'] as List< String > ).map( ( answer ) { return Answer( _answerQuestion, answer );  } ).toList(),   
map method를 사용하여 질문의 선택지 목록(List< String >)로부터 각각의 String에 대한 버튼위젯 리스트(iterable)를 생성해 기존의 layout에 연결   
(qa: Map, 질문(key='question', val:String)과 선택지(key='answers', val:List)를 key로 가짐)   
(Answer: Custom 버튼 위젯, onpressed에 쓰일 함수포인터와 button에 표시할 string(answer)을 받음.) 

- *flutter internal*   
tree형태로 위젯을 관리. 내부적으로 3개의 tree구조를 유지.    
widget tree(configuration) - element tree(widget과 render tree를 참조할수있도록 연결, state attached) - render tree(Rendering)     

- *rebuilding Widget tree는 비효율적이지 않음. (How flutter is.)*          
Widget tree는 단순한 configuration객체의 tree. 실제UI에 표시하는 것은 render tree기반.     
render object를 전부 rebuild하는것은 inefficient하지만 Widet tree을 rebuild하는 것은 괜찮음.    
즉, re-rendering시 widget tree를 재구성 후, render 객체를 다시 위에서부터 check하며 widget의 바뀐 rendering부분만 수정.    
그래도, 큰 app에서는 최대한 바뀌는 부분의 위젯 트리만 rebulid하도록 위젯들을 적당히 split하는게 좋음.   

- *Widget state(lifecycle)*    
Stateful위젯은 생성자를 통해 첫 생성시, state객체를 생성 -> state객체에서 initState()를 호출 후, build()      
-> setState(), build()를 호출하며 위젯 유지. 이때, stateful위젯이 다시 생성될때마다 state에서 didUpdateWidget()호출 후, build()호출          
-> Stateful위젯이 변경이 아닌 소멸될때,state의 dispose() 호출.
(initState, didUpdateWidget, dispose()를 override하여 사용시, 함수내에서 super.initState()필요. 상속한 객체인 sate의 함수호출)     

- *app lifecycle*     
전체 app의 state에는 inactive, paused, resumed, suspending가 존재.      
app의 state를 확인하고자 할땐, stateful 위젯의(main app이 아니라도?) state객체에서 with WidgetsBindingObserver를 mixin한 후,    
클래스 내에서 listner인 didChangeAppLifecycleChange(AppLifeCycleState)를 override. 해당 함수는 app의 state가 변할때마다 호출되고,     
변화된 state에 대한 정보를 포함한 객체를 인자로 전달받음. 위 Listner를 stateful위젯에서 동작하게하기위해, stateful위젯의 생성과 소멸시,    
initState(), dispose()에서 각각 해당 위젯에 attach된 listner를 생성과 소멸하도록 선언필요. (memory leak방지)         
ex) 생성: WidgetsBinding.instance.addObserver(this); 소멸: WidgetsBinding.instance.removeObserver(this);     
(stateful위젯이 생존해있는 동안 app State가 변할때마다 이 위젯 내의 didChange..함수를 호출하도록 명시.)    

- *list & stateful item*     
list의 각 item이 stateful위젯인 경우, 한 item을 삭제시, 해당 item에 대응되는 state가 삭제되지않고, 다음 item에 attach되는 issue.     
그 이유는, item이 삭제되고 위젯트리가 변경된 후, element tree를 다시 확인하는 과정에서, element는 대응되는 위젯을 type으로 인식함.     
이때 List의 item이 삭제되면 다음 item이 자동으로 위부터 채워지므로, 다음 stateful item이 지워진 item의 (state attached)element로 인식.    
해결 방법: 위젯의 key이용. element는 key가있다면, 대응되는 위젯을 타입+key로 인식.
item인 stateful위젯의 생성자로 unique한 key전달. 이때, ListView의 builder로 item생성하지 말고(key명시 불가 버그?), map이용해 생성.    
ex) Abc({..., Key key}): super(key: key)      
item에 직접 해당하는 topmost의 stateful위젯의 생성자에 전달받아 상속한 부모위젯인 stateful위젯에 key값을 명시.      
(: super(..) 처럼 상속 부모클래스의 초기화에 특정인자를 명시하는 것을 "initializer list"라고 함. 주로, key쓸때만 사용)      
이때, 매 item생성시, UniqueKey()를 쓰면, item목록이 변하지않더라도 ListView가 갱신될때마다,(ListView의 조상위젯이 rebuild된다면,)      
각 item에 다시 새로운 key를 입력하므로, element들이 기존에 대응되는 위젯을 찾지못해, 새로운 위젯을 매번 다시 생성.       
따라서, UniqueKey대신 ValueKey 사용. ValueKey의 인자로는 unique한 key인 id등을 쓰면 됨.     
// 이 외에도, list가 유동적으로 변하거나, list item들이 일부가 변하지만, 전체 list가 다시 빌드되는 등의 경우(ex) chat), 
// 효율적으로 listview가 re-render되기 위해서, 유동적으로 변하는 listView에는 항상 key를 써주는게 좋다.     


- *column & conditional TextFormField*     
list & stateful과 마찬가지로, textFormField 여러개를 stateful내에서 Form내의 column에 선언시, 특정 textField를 특정 조건인 경우만 렌더링 시,      
버튼 클릭 등으로 조건을 변경해 해당 textFormField를 지우면, Form내에서 해당 textField내의 input이 지워지지않고, 순차적으로 다음 textField input으로 attach됨.      
마찬가지방법으로, key를 지정해 해결.        

 - *pages as a stack*    
 page들은 stack으로 관리됨. topmost Page가 화면에 표시. stack은 현 페이지에서 push(), pop() 등으로 관리.          
push, pop은 navigator의 도움을 받아 사용. 스택이 쌓여있다면, topmost페이지에 뒤로가기버튼 자동생성.(버튼은 단순 Navigator.of(context).pop()실행)           

- *initState & context*    
staeful위젯의 state를 처음 생성시 필요한 변수의 초기화를 initState내에서 진행하는 경우.    
해당 초기화에 위젯의 context값을 사용한다면, error발생. 그 이유는, initState가 state위젯이 fully create되기 전(context가 생성되기 전)에 실행되기 떄문.     
(initState에서 다른 일반 property나 widget(대응stateful객체)들은 사용가능. context는 아직 미생성.)          
해결법1: didChangeDependencies에서 초기화    
state처음 생성시 필요하나 변수를 initState가 아닌 didChangeDependencies에서 초기화(모든 변수(context포함)가 초기화되었을때마다 호출되므로).      
단, didChangeDependencies는 매 state dependency가 변할때마다 호출되므로, state의 생존 동안 초기화를 매번 진행하지않기 위해,    
bool _loadedInitData =false; 와 같이 bool면수를 사용. didChangeDependencies내에서 _loadedInitData가 false일때만 초기화를 진행하고 bool값을 true로 바꾸고,    
그 이후에 다시 호출되더라도, bool값이 true이므로 초기화를 진행하지않도록 구현.       
해결법2: initState에서 선언하되, Future.delayed(Duration.zero).then()내에서 사용.        
물리적으로, 시간상의 차이(Duration.zero)는 없지만, Future로 선언하여, initState에서 모든 state위젯의 dependency가 생성되고 난 다음에 실행되도록 실행순서를 바꿔주는 역할.          

- *didChangeDependencies*    
sateful위젯의 state객체의 dependency(reference)가 변할때마다 호출되는 함수. state내 모든 위젯 및 변수가 처음 initialized되었을 때도 호출.    

- *state&state management*    
일반적으로, app내의 data를 state라고하고, 이 data들을 바탕을로 UI를 상황에 맞게 rendering함.    
state는 크게 app-wide state(app전체 혹은 많은 위젯에 공통적으로 영향이 있는 data. ex)authentication, 전체Loaded products..)와    
widget(local) state(현재 위젯에만 관여된 data. ex)loading spinner 표시여부, form(field) input등)으로 구분.    

- *multiple TextField in Column/Listview*       
Form등으로 여러개의 TextField를 포함한 column을 만드는 경우, render영역을 초과하는것을 대비해, singleChildScollView+Column 혹은 Listview를 사용하는데,      
textField가 매우 긴 경우, ListView를 사용하게되면, item이 화면밖으로 많이넘어갔다 다시 추가되는 경우, textField를 re-add하므로, 입력한정보가 사라지는 issue발생.     
즉, list의 정해진 길이가 짧으면서, portrait mode만 지원하는 경우면 ListView를 써도 괜찮지만, list가 많이 길어질 수있는 경우, 

- *http request & progress Indicator*     
http request과 같이 시간이 걸리는 명령을 처리할때, 화면에 progress Indicator를 rendering하는 방법.      
ex) product를 추가하는 버튼 클릭시, http request를 보내고 모든 요청을 처리한 후 페이지를 pop하기 전까지, 화면에 indicator를 표시하고자 하는 경우.    
statful인 페이지(Screen) 위젯 내에 bool 변수 _isLoading = false를 선언, 페이지의 body부분을 _isLoading에 따라 기존페이지(false),     
CircularProgressIndicator(true)을 렌더링.
product를 저장시 실행되는 함수의 처리 순서는(물론, 저장이 valid한 경우), setState(loading=true); 후에, http request를 보내고(post), 기다렸다 response를 받은 다음에 
(Post.then()혹은, Post.then()도 처리된(ex)app data에도 추가하는 과정) 후인 Post.then().then()에서), 다시 setState(loading=false) 및 Navigator.pop();     
(이때, http request를 보내는 부분이  Provider<Product>에 선언되어있다면, 해당 부분이 Future<void>를 반환하도록 하고(post메소드 자체나 post.then.then..then을 반환하면 됨),     
기존의 저장시 실행하는 함수에서 Provider가 return한 Future의 then에 setState과 pop()을 선언.)     
 
 - *future & context*      
of(context) 사용시, async의 await 후/future.then()내에서 사용하는 경우, 에러 발생. 그이유는, 현 위젯의 context가 future완료 후 변할수 있기 때문.   
해결방법: context로 참조한 객체를 변수에 저장해두어 사용.   
ex) final scaffold = Scaffold.of(context);     
async{await ..., scaffold.showSnackBar(..);}    

- *Scaffold.of(context)*     
Scaffold를 build하는 class내에서 Scaffold.of(context)로 Scaffold를 참조하면, 현재 Scaffold를 찾지못하는 에러 발생. (위젯 상위에 있는 scaffold를 찾으므로)    
해결법: Scaffold를 참조하고자하는, scaffold내의 세부위젯의 context를 of에 사용.     

- *FutureBuilder & build*      
FutureBuilder를 위젯의 자식위젯으로 쓰는 경우, 위젯이 build될때마다, FutureBuilder를 다시 생성, future객체를 다시 생성함.     
하지만, 첫 build에만 future를 생성 및 실행하고, 그 이후 build에는 같은 future를 다시 생성하고싶지않다면,     
(예를들어, 스크린을 렌더링하기위해 http request를 보내 화면에 쓰일 데이터를 가져오는 경우, FutureBuilder로 http request 완료 전까진 loadingSpinner를 표시하고,      
완료 후엔 데이터를 렌더링한다고 가정. 첫 렌더링 이후, 다른 사유로 (예를들어, 현 스크린에 dialog를 pop-up할때와 같이)스크린의 build를 다시 호출하지만, 데이터는 바뀔일이 없어      
FutureBuilder의 http request를 다시 실행하지 않아야한다면(새로운 future를 다시 생성/실행))     
위젯을 stateful로 바꾸고, 한번 실행될 future를 initState에서 변수에 저장해둔 후, build내의 FutureBuilder의 future: 에 사용.    
즉, future를 처음에만 생성한 후, 다시 build되더라도, FutureBuilder의 future는 새로 생성하지않고, 완료된 상태의 future를 사용.

- *Button의 외곽UI을 없고자할때(ex)화면 맨아래에 공간남김없이 버튼을 붙여서 렌더링하고자할때)*      
drop shadow를 없애기위해 elevation: 0 을 주고, (flatButton이 아니라면, default값이 2이므로, 버튼 밑에 검은 영역 존재.)              
materialTapTargetSize: MAterialTapTargetSize.shrinkWrap을 줌.    
버튼은 기본적으로, tap가능한 영역을 버튼 이미지 바깥에 좀더 주기위해, 버튼의 이미지보다 더큰 영역(탭 가능 영역)을 차지.   
해당 영역을 없애기 위해 shrinkWrap으로 선언.     

- *DexArchiveMergerException error*      
Firesore access시 발생하는 error.    
해결법: 앱수준 build.gradle내의 defaultConfig영역에 multiDexEnabled true 추가 + dependencies영역에 implementation 'com.android.support:multidex:1.0.3' 추가     

## tips    

- *adaptive한 위젯들은 custom으로 관리해 code 간결화*    
adaptive하게 비슷한 위젯을 여러번 사용할시 매번 ternary expression쓰는 것보다 adaptive custom 위젯 클래스를 만들어 사용.     
ex) theme에 맞는 flat Button 생성시 매번 os확인하여 생성하는 코드를 복사하는 것보단, adaptive_flat_button클래스를 만들어     
위젯 내에서 OS를 확인하고 필요한 정보를 받아 버튼을 렌더링하는 것이 코드관리에 효율적.   

- *adaptive by flutter*     
특정 위젯들은 OS에 맞는 style로 adaptive하게 생성될 수 있도록, (widget.)adaptive() 형태로 특별한 named 생성자 제공.    
해당 위젯이 build되는 os에 맞게끔 style이 설정되어 rendering된다.    
.adaptive로 전체적으로 os에 맞게끔 생성하되 특정 부분만 메인 색상으로 별도로 지정할 수도 있음.    
가능한 위젯 목록 : switch,      

- *디바이스 Platform 참조*    
dart가 제공하는 'dart:io'를 import하면, 해당 파일이 실행되는 os를 참조할수있는 객체인 Platform에 접근가능.   
(Platform.)isIOS/isAndroid 등으로 현재 os가 IOS인지 확인 가능. 해당 값들은 bool.     

- *앱이 지원하는 모드 결정. 예)portrait만 지원하고싶은 경우*    
import 'package:flutter/services.dart',   
WidgetsFlutterBinding.ensureInitialized();    
SystemChrome.setPreferredOrientations([DeviceOrientation.portraitUp, DeviceOrientation.portraitDown,]);   
(WidgetsFlutterBinding.ensureInitialized()은 다음줄인 Orientation설정 함수가 정상적으로 작동하도록 설정.)   
(SystemCrhome은 system wide한 설정 가능하도록 하는 클래스, setPreferredOrientation()으로 앱의 지원 mode 설정.      
mode목록을 List로 전달.  DeviceOrientation은 enum.)    

- *fully controlled animation*     
staeful위젯의 state내에 AnimationController과 Animation을 직접 생성하는 방식. (<->) built-in widget사용(AnimationBuilder, AnimatedController..)     

- *Packages using device features*      
device feature를 사용하는 package를 사용하는 경우, 공식 문서의 README를 숙지할것.     
device feature를 접근해 처리하는 메소드를 별도의 helper 클래스(static 메소드로 구성)를 생성하여 사용.     

- *Google Cloud platform(Maps API, ..)*      
google Maps API : key를 발급받아 사용.           
google map의 이미지사용: (Maps Static API)  https://developers.google.com/maps/documentation/maps-static/overview    
위도, 경도 <-> 실제 주소 변환 : (Geocoding API) Google Maps Platform > documentations > Geocoding API // API(url)에 http request를 보내고 응답 사용.      

- *SQLite를 emulator에 사용시 db구성이 변화하는 경우*     
test하는 경우, db내에서 이름이 바뀌거나 구성이 바뀐다면, 해당 db를 emulator내에서 삭제 필요. 즉, 앱을 uninstall하고 다시 build해야 제대로 작동.     

-----------------

## memo  

- *flutter docs/ widget catalog*     
위젯의 정보 및 자세한 사용법에 관한 문서.         

- *pub.dev : packages info*    
플러터 패키지 문서. 각 패키지에서 API reference를 누르면 사용법, 기능 등이 적힌 문서 확인 가능.    

- *같은 폴더 내 파일 import시 import './aa.dart';*    

- *현폴더 상위폴더로 이동시 import '../aa.dart';*     

- import시 해당 파일의 이름을 따로 명시할때(ex)두개이상의 파일에 같은class가 있어서) import '..' as ci; // (ci.)으로 함수 및 class접근가능.      

- import시 파일 전체를 사용하기위해 불러오는게 아닌 몇개의 feature(class,함수)만 가져오고 싶을때 import '..' show Cart; // Cart클래스만 import      

- *함수, 변수 ctrl+click : 해당 함수or변수가 포함된 파일 열기*    

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
readable / split widgets    
readable / use builder method(긴 코드의 자식위젯을 생성하는 함수를 클래스내에 따로 선언해 build에서 호출)    (비슷한 위젯을 여러번 만들때도 유용)                   
performance / use const widget,    

- *split Widgets vs use Builder method* (정확히 예를들면?)        
한 위젯 내에서 긴 코드의 자식 위젯을 분리하거나, 클래스 내에 builder method를 쓰는 것은 큰 차이는 없음.    
다만, 만일, 해당 자식 위젯이, state나 theme,mediaQuery 등을 변경해, 다시 build된다면, 해당 위젯만 다시 build되는(부모 위젯 build는 호출하지않음.)    
split Widgets가 builder함수 사용보다 좀더 optimize가능함.    

- *const Widget*    
Widget tree의 모든 위젯은 property가 전부 final이라면 const 생성자를 붙일수 있음. (정확히는 dart의 featurue)    
(ex) class Abc{ final a; final b; const Abc(this.a,this.b);}) (위젯의 경우 생성이후 변하지 않는 immutable이라는 의미)    
또한, const 생성자를 가진 클래스는 const 객체를 생성할 수있음. (단, 생성자에 전달할 argument가 dynamic이 아닌 const인 경우.)     
즉, widget도 const로 생성할 수 있는데, 이런 경우 해당 위젯을 생성하는 부모 위젯가 re-build되더라도, 해당 위젯은 re-build되지 않음.    
(위젯트리에서 새로 instantiation되지 않음.) 즉, 큰 performance향상은 아니지만, 큰 app에서 사소한 차이는 보일 수 있기때문에,        
const로 생성할 수있는 위젯/객체는 const로 생성하는 습관 추천. app을 완성하고 추가하는 방식으로.       
  
- *ios device에 앱 실행*     
xcode 혹은 IDE에서 실행. bulde identifier가 unique해야함.     
빌드 에러시 앱 프로젝트 내에서 flutter clean, flutter build ios 실행. (초기화 및 ios dependencies설치)    

- flutter build ios 실패시
https://stackoverflow.com/questions/63742891/encountered-error-while-building-for-device-flutter 참조.     

-----------------
  
## app configuration before publishing      

- app drawer에 표시될 앱의 라벨 지정. (안드로이드, ios)     
안드로이드: app/src/main/res/AndroidManifest.xml에서 android:label에 명시.    
ios: xcode프로젝트의 runner.xcworkspace폴더를 열어 runner탭에서 display name에 명시.    

- internet permission (안드로이드)     
user permission 중 internet permission은 debug시에 debug/AndroidManifest내에 포함되어있어 자동으로 원 androidManifest와 병합되어     
사용되지만, publish되는 경우 앱의 internet을 필요로한다면, 원 manifest(src/main/res/AndroidManifest.xml)에 internet permission을 명시해야함.    
(debug내의 manifest파일에 있는 user permission을 main mainfest파일로 복붙하면됨.)    

- package identifier(안드로이드, ios)     
domain/이름 형식으로 구성된 identifier는 unique해야함.(android와 ios에 명시한 identifier가 같을 필요는 x)     
안드로이드(모두 동일한 값으로 설정해야함.) :     
main manifest, debug폴더의 manifest, main/java내의 mainActivity(->현재는 kotlin내로 변경됨.), build.gradle내의 applicationId        
ios (identifer가_를 포함할수없음.):      
xcode에서 프로젝트의 runner.xcworkspace폴더를 열어 runner의 bundle identifier에 명시.     

- 지원하는 version check     
안드로이드의 경우, build.gradle에서 minSdkVersion, targetSdkVersion등 지정 가능. https://developer.android.com/studio/publish/versioning     

- third party service 사용시 필요한 부분 check     
firebase의 security rules, google api의 API key, API key restrition(google cloud console의 credential탭에서 설정 가능.) 등등..     

- app icon     
flutter_launcher_icons패키지 사용. pubspec.yaml의 icon의 path와 background color(for 안드로이드 adaptive_icon)등과 함께       
dev_dependecy에 명시후 run.(패키지 README참조)        

- splash screen (android) 
main/java(kotlin)/res/drawable/launch_background.xml내에 명시. image의 경우 이미지 src(소스)를 drawble폴더내에 저장하고, 경로를 명시하여,        
bitmap item으로 선언. 이미지의 background color를 지정하려면, values폴더내의 color에 color값을 이름과함꼐 저장하고, 다시 background.xml에 item으로 android:color로    
해당 컬러를 사용할것을 명시.    
ex)    
```Dart	     
<item>    
   <color adnroid:color="@color/splash"  />      // value폴더 내의 color파일에 splash이름의 색깔값을 선언해야함.     
</item>   
   
<item> 
   <bitmap     
         android:gravity="center"      
         android:src="@drawble/splash_icon"  />    // 각 drawble폴더내에 같은 이름의 이미지 파일이 포함되야함.  
 </item>     
 ```	     
 
 - splash scren(ios)    
 xcode에서 ios 프로젝트 열어, assets.xcassets에 launchImage에 이미지 파일을 갖다붙임.    
 launchScreen.storyboard의 view에서 확인 가능. 또한, view의 attribue inspector에서 이미지 외곽의 background color설정 가능.     

# publish       
- 안드로이드 : https://flutter-ko.dev/docs/deployment/android    
key-store 생성 - 앱에서 keystore 참조(key property파일이 git등의 오픈소스로 공개되지않게 git.ignore에 명시) - 터미널로 프로젝트 폴더에서 앱번들 빌드 - play console에 엡 정보와 함께 번들 등록     
