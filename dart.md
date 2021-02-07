## Dart문법

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
객체를 특정조건으로 생성하고자할떄, 일반 생성자가 아닌 이름이 지정된 생성자로 생성.       
ex) Abc.withNum(3,4);     

- *(List.)map*       
List의 각 원소를 다른 원소로 mappingg하여 새로운 iterable반환. 리스트로 변환하려면 .toList() 추가     

- *getter/setter*   
getter // 클래스 내에서, 변수를 이용해 특정 값들을 도출해 반환할시. (ex) enum변수를 적절한 String으로 변환할때 등)     
setter // private변수를 외부에서 접근, 초기화할시.  

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
years를 제외한 인자의 defautl값(1). 즉, DateTime(2021)처럼 생성시 해당 년도의 1월 1일 00시 .. 로 초기화.    
DateTime.now() : 현재 timestamp로 DateTime객체 생성하는 생성자.   
tip: now()를 debug시 간편하게 unique한 객체의 id로 사용가능.   
(DateTime.)(day,month,year) / 해당 dateTime이 나타내는 년,월,일 참조.   
(DateTime.)subract(Duration) / DateTime의 시간에서 Duration만큼의 시간을 빼주는 메소드.   
(DateTime).isAfter(Datetime2) / 다른 DateTime과 비교하여 그 이후인지를 bool로 반환해주는 메소드.   
Duration() / 시간의 duration값을 나타내는 클래스. 생성자에 named argument전달.(days: int, hours: int 등으로 duration의 길이 지정.)      

- *(object.)toString()*    
모든 object에 암묵적으로 포함된 메소드. object를 String으로 변환. double, DateTime등을 String으로 바꿀때 유용.    
ex) double a; a.toString(), Datetime b; b.toString()    
(num.)toStringAsFixed(int) (num을 decimal뒤 숫자갯수를 지정해서 반올림하여 String으로 변환.)     

- *Dart syntax를 print내에서 character로 명시할때*   
',$같은 charcter를 print할시 feature syntax로 인식하지 않으려면 \을 앞에 붙인다. ex) print(' \$ ');   

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
첫번째 인자로 List의 길이, 두번째 인자로 (index){}함수를 받음. index에는 List길이만큼 0부터 매 원소 생성시마다 1씩 증가해서 전달.   
함수body에서 List의 각 원소로 들어갈 object반환.   

- *(List).reversed*     
현 List의 원소 순서를 뒤집어 iterable로 반환. List로 변환하려면 reversed.toList() 사용.        

- *(Map.)containsKey(_key)*  
Map의 key 중 해당 값(_key)이 존재하는지 반환.    

- *(Map.)putIfAbsent(_key, ()=>_value)*     
Map의 존재하는 key 중 _key가 없다면 새로 들어갈 key와 value pair를 입력. 두번째 인자로 _key와 대응되는 입력할 value를 반환하는 함수 명시.    

- *(Map.)update(_key, (value)=>newValue, ()=>newValue)*    
Map의 _key가 존재한다면, 대응되는 value를 새로운 newValue로 바꿔 저장. 만약, _key가 존재하지않는다면, 생성하여 newValue를 입력.     
두번째 인자로, 기존의 value를 newValue로 바꾸는 함수 명시. 세번째 인자로, _key가 없다면, 저장할 newValue를 반환하는 함수 명시.    
_key가 반드시 있다면, 세번쨰 인자는 명시하지않아도됨. _key가 없는데 세번쨰 인자가 없다면 error.    

- *(double.)parse(String)*   
String을 받아 double로 변환해주는 메소드. 입력String이 double로 변환이 불가능하면 error출력.   

- *(object.)isEmpty*   
해당 오브젝트에 값이 있는지(null) 확인.   

- *(String.)subString(i,j)*   
String [i..j)까지의 subString을 반환.   

- *(List).fold(<T>first, (<T>sum, item)-> <T>)*   
 list의 각 원소를 순회하여 특정값을 계산해서 반환하는 메소드.   
 첫번째 인자로, 계산할 값의 초기값 지정. (일반적으로, totalSum을 구하면, 0으로 입력)     
두번째 인자로, 각 원소의 아이템으로 계산할 값을 수정하는 함수 입력.   
계산할 값의 현재값(sum)과, 현재 사용할 List의 원소(item)을 전달받아, 함수body에서 새로 덮어씌울 계산할값을 return해야함.    
 
 - *Future<T>*    
 <T>형 객체를 비동기적으로 저장하는 객체. 해당 변수가 저장되길 기다린다.
  (Future.)then((var) {}) / 변수가 저장될때, 실행하는 함수 선언. 실행 함수의 인자로 저장된 객체를 전달    
 then? wait? await?    
  
  - *if in List*   
  [if(a>1) B, C, ]처럼  if문이 성립할시 직후(괄호{}는 쓰지 않음.)의 원소를 포함하도록 선언 가능.    
  (flutter에서 children내의 특정 위젯을 포함시킬지 결정하는데 유용.)      
  
  - *mixin*    
  클래스가 직접 상속하지않고, 사용할수 있는 변수 및 메소드를 제공하는 feature. mixin Ghi{}와 같이 선언.    
  다른 클래스에서 해당 mixin을 가져와(with 키워드 사용.) mixin내의 메소드를 사용 가능. 한 클래스는 여러 Mixin을 포함 가능.          
  상속과 다른점: 논리적으로, 상속처럼 클래스가 다른 클래스와 밀접한 관계를 맺는 것을 나타내는 것이 아닌, mixin의 함수를 제공받아 사용하는 것.     
  mixin은 일종의 유틸리티 함수 제공 담당. 또한, 상속은 한 클래스만 가능하지만, mixin은 여러 개 사용 가능.     
ex) class Abc extends Def with Ghi와 같이 mixin명시.    
  
  - *Random*     
  random수를 생성할수있게 돕는 클래스. 특정 조건으로 난수를 생성. import 'dart:math' 필요.       
  Random().nextInt(k) / 0~k-1범위의 정수를 하나 생성해 반환하는 함수.    
  
