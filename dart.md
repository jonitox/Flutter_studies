## Dart문법

- *object*    
dart의 모든 데이터(기본자료형/class)가 상속하는 클래스. toString() 메소드를 가짐.    

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

- *(List./Map.)length*    
List의 원소개수를 반환. Map의 key의 개수를 반환. (단, null이면 error, 빈 list면 0)     

- *??*     
var a = b ?? c;   // a 저장 시 b가 null이면 c를 사용. null이 아니면 b를 사용.           
     
- *(List.)map*       
List의 각 원소를 다른 원소로 mappingg하여 새로운 iterable반환. 리스트로 변환하려면 .toList() 추가     

- *(List,Map.)forEach*      
각 원소를 순회하며 실행할 함수를 받는 메소드., Map의 경우 entry(key,value)를 함수에 전달.(인자가 2개)     

- *getter/setter*   
getter // 클래스 내에서, 변수를 이용해 특정 값들을 도출해 반환할시. (ex) enum변수를 적절한 String으로 변환할때 등)     
setter // private변수를 외부에서 접근, 초기화할시.  

- *...*   
List b; List a = [1,2, ...b]; : b의 각원소를 나누어 a에 이어 붙임. (List a = [1,2,b] : b가 nested list로 선언됨)   
Map이라면 entries를 이어붙임. {...(Map)}와 같이 Map의 entry를 복사해 똑같은 새로운 Map생성 가능.    

- *..*      
객체내의 메소드 등을 참조하지만, 해당 메소드의 반환값을 사용하는 것이 아닌, ..앞쪽의 객체를 사용.   
ex) transform: Matrix4.rotationZ(-8 * pi / 180)..translate(-10.0),      
// transform은 Matrix4를 받음. Matrix4내의 translate은 해당 Matirx4내의 변수들을 변경하는 void형 메소드.     
즉, Matrix4를 생성 및 final myTrans = Matrix4.rotationZ(..)처럼 저장한 뒤, myTrans.translate(..) 후에 transform: myTrans로 전달할 필요없이,    
생성된 객체내에 메소드를 호출하고 해당 객체를 바로 사용하는 코드를 한줄로 작성 가능.    

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

- *(List.)insert(int, Object)*     
List의 특정 index에 원소를 추가하는 메소드. 기존에있던 뒤의 원소는 자동으로 1씩 밀려서 채워짐.    

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

- *double.parse(String)*   
String을 받아 double로 변환해주는 메소드. 입력String이 double로 변환이 불가능하면 error출력.   

- *double.tryParse(String)*    
parse()와 똑같지만, double로 변환이 불가능할시 error를 출력하는것이 아닌 null을 반환.      

- *(String.)startsWith(String)*     
String의 prefix로 다른 String이 존재하는지를 확인. true/false반환.    

- *(String.)endsWith(String)*       
String의 suffix로 다른 String이 존재하는지를 확인. true/false반환.    

- *(object.)isEmpty*   
해당 오브젝트에 값이 있는지(null) 확인.   

- *(String.)subString(i,j)*   
String [i..j)까지의 subString을 반환.   

- *(List).fold(<T>first, (<T>sum, item)-> <T>)*   
 list의 각 원소를 순회하여 특정값을 계산해서 반환하는 메소드.   
 첫번째 인자로, 계산할 값의 초기값 지정. (일반적으로, totalSum을 구하면, 0으로 입력)     
두번째 인자로, 각 원소의 아이템으로 계산할 값을 수정하는 함수 입력.   
계산할 값의 현재값(sum)과, 현재 사용할 List의 원소(item)을 전달받아, 함수body에서 새로 덮어씌울 계산할값을 return해야함.    
 
 - *Future&asynchronous(비동기식) code*    
자바스크립트의 promise와 같이 future는 생성시 생성자로 전달한 함수를 실행하고 반환값인 <T>를 기다림. 함수가 리턴하여 종료되면 완료상태이거나 혹은 중간에 error발생시 throw error    
 Future내의 함수는 바로실행되지만 함수의 실행 종료를 기다리지않고, 나머지 다른 모든 synchronous코드를 실행. 그 후 다시, future의 함수가 완료되었는지 확인.       
 (Future.)then((dynamic){}) // 확인했을때 Future객체의 완료 상태시 실행할 "비동기" 명령을 명시. future로 실행한 함수의 리턴값을 then의 함수에 전달.(null이어도 전달.)      
 then은 다시, future객체를 생성. 즉, then내의 코드도, 비동기로 실행. (Future.)then(..).then(..)같이 연쇄적으로, future에 then 실행 가능.       
(Future.)catchError((error){trow error}) // Future로 실행한 함수에서 error발생시 catch. (then().then().catchcError().then()...와 같은 경우, 앞쪽then에서의 error catch.)   
 (throw error: catchError가 생성하는 future객체에서 다시 error를 throw. 해당 객체를 이어받는 then혹은 catchError로 전달됨.)    
(error발생시, 코드에 catchError가 없다면(handling안되면), 그 뒤의 then 명령 실행안됨. catchError를통해 발생된 error를 처리해주어야(제거) 다음 명령들 제대로 실행.)     
 
 - *async/await*     
future.then.then..을 대체하기 위한 키워드. async, await은 한쌍으로 사용되며 async는 함수 body전체를 future로 감싸고(자동으로 async로 선언한 body(함수)는 Future반환),    
async내에서만 await 사용 가능. await으로 선언한 구문은 반드시 future여야함, await 선언시 해당 future가 done이기 전까지 다음 구문의 실행을 기다림. future가 완료되야 다음 구문 실행.     
즉, 내부적으로, future-then과 동일한 코드. async내의 코드를 future내 함수처럼 비동기적으로 실행하고, await부분의 뒷부분은 then()처럼 실행.)     
ex) Future<void> addProduct(Product product) async{    
 final response = await http.post(...);     
 final newProduct = Product(...,id: json.decode(response.body)['name'],);

- *try/catch/finally*     
error-handling을 위한 구문. 특히, async, await을 사용할때 error를 catch하려면 try-catch로 error catch. (future.catchError와 달리, 직접 catch)       
try문을 일반코드처럼 실행. try내에서 error가 발생하면 catch문으로 errorCatch. error가 나는 여부와 상관없이 try,catch문 실행 후 finally문 실행.       
ex) try{     
...     
} catch(error){     
...    
} finally{     
...    
}

- *remove&memory*     
List(map)의 item(object)을 remove하거나, 변수(포인터)에 저장된 object를 다른 object로 갱신하는 경우,     
dart는 해당 object를 memory에서 자동으로 지워줌.  (memory관리 / memory leak 방지)   
단, 해당 object를 참조하는 다른 어떤 변수도 없을시에만 지움. (즉, 해당 object가 더이상 사용할 일이 없다고 판단되는 경우)   
따라서, list의 item을 지울때, 서버에서도 지우는 요청을 하고 실패하는 경우에 다시 해당 item을 추가하려면       
해당 item의 index와 obejct를 다른 변수에서 참조해두고, 지운다음. 서버 요청 실패시 다시 추가하는 logic을 구상할수 있음.    

- *throw error *    
함수 내에서 오류 발생시 throw키워드를 사용하여, error를 함수 밖으로 내보내고, 뒤의 구문을 더이상 진행하지않고 함수 종료.    
내보낼 error는 객체로서 일반적으로 Exception객체.     
 
- *Exception*      
error를 나타내는 dart클래스(추상클래스). custom error를 생성할때 직접 사용하기보다 implement하여 사용. implement시 toString을 반드시 override    
ex) class HttpException implements Exception{      
final String message;
HttpException(this.message);     
@override    
String toString(){    
   return message;     
   }        
}

- *abstract class & implements*     
abstract : 직접 사용할 목적이 아닌 반드시 오버라이드해서 사용하도록 강제할 유틸리티 메소드들을 포함하는 클래스.     
implements : abstract class를 상속. 반드시 모든 메소드를 override해야함.    


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

- *min,max*    
min(a,b),max(a,b) // import 'dart:math'     
