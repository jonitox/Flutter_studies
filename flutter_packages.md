## Flutter  

- *pubspec.yaml*   
프로젝트의 set-up과 dependencies, 포함패키지 등 설정. 파일을 저장하면 자동으로 필요한 파일 및 패키지를 install & get.    
indent로 항목을 구분하기때문에 indent를 잘 지킬 것!    

- *pubspec.yaml: assets*     
프로젝트에 사용되는 font를 제외한 asset(이미지 등) 명시. 당연히, 모든 파일은 프로젝트 폴더 내에 저장되있어야함.     
assets: // 사용될 파일들 명시   
    -파일위치 및 이름    
  
- *pubspec.yaml: fonts*   
프로젝트에 사용할(다운받은) font를 명시. (당연히, 프로잭트 내에 assets/fonts/...등에 저장되있어야함.)    
pubspec.yaml에 font 영역 수정.   
fonts:   
   family: '사용할 폰트그룹명' // 한 폰트 그룹 (bold,regular..)의 이름(직접 입력)    
     fonts: // 그룹이 포함할 폰트 파일들을 아래 assets로 명시.   
        assets: // 폰트의 해당 파일(위치경로 포함)    
        (optional)... //(해당 파일 폰트에 대한 정보를 추가. 다운받은 폰트의 info확인!    
        ex) 특정폰트의 bold 버전은 weight: 700 으로 지정해야 fontWeight: FontWeight.bold로 해당 패밀리의 bold버전 접근(지정된게 없거나 600은?))        

- *프로젝트 폴더변경시 hot reload,restart에 적용안됨. pub get 후 restart필수*   

- *package:flutter/material.dart*    
flutter 여러 base위젯 및 함수 등이 포함된 flutter패키지   

------------------------------
## provider     

- *concept: provider & listner*.   
특정위젯에 attach된 provider(data container)를 지정하면, 해당 위젯의 자식위젯(deep한 자식 포함)들은 이 provider에 대한 listner를 선언 가능.      
그리고 이 위젯의 provider의 상태가 변하면(update), 모든 자식위젯을 다 빌드하는 것이 아닌, listner가 선언되이있는 자식위젯들만 build.      
사용하지 않는 data임에도, 해당 data를 자식위젯이 필요로 해서, data를 deep하게 받아 계속 전달해야하는 경우 사용.    
provider는 어느 위젯에도 선언될수 있고, 같은 위젯에 여러개의 provider를 선언할수도 있음.    

- *stateful vs provider* 
state이 특정 위젯만 관련이 있는 widget-wide state라면 단순 stateful로 구현하는게 간편. ex) 현재 스크린의 filter       
하지만, 여러곳에 영향이 있는 app-wide state이라면 provider가 더 적합. ex) 전체 product의 상태              

- *provider선언*     
provider는 data container로서 우선 provider로 사용할 class를 선언.(data를 원하는 형태로 담아둔 class)     
이때, 이 클래스(provider)를 listen하는 모든 자식 위젯들에게 해당 객체가 변경되었음을 알리기 위한 함수 notifyListner()를 사용하기 위해     
import 'package:flutter/foundation.dart' 후 ChangeNotifier를 이 클래스에 mixin.    
그리고, 해당 클래스내의 변수를 변경하는 함수를 선언하여, 변경 후 notifyListner()를 호출.    
ex)    
```Dart	      
import 'package:flutter/material.dart';
import '../models/product.dart';
class Products with ChangeNotifier{     
    List<Product> _items; // 일반적으로, provider내의 변수는 private으로 선언하고, 참조(get)와 변경(add등)을 따로 선언. 외부에서 직접변경하지않기위해.      
    // private변수를 참조하는 getter    
    List<Product> get items {    
        // 일반적으로, 리스트같은 변수의 레퍼런스를 직접 전달하지않고, 복사해서 전달해준다. 그이유는 직접 변경을 막고, 클래스내의 변경함수를 호출해서 변경하기위해     
        return [..._items];    
    }     
    // 변수의 변경. add함수 //  변경 함수를 호출해 변경해야, listner들에게 알리는 함수까지 호출할수 있음.     
    void addProduct(value) {    
        _items.add(value);    
        notifyListeners(); // 이 객체의 변수를 변경 후, listner들에게 알린다. // provider와 연결되어있는 listner를 re-build.    
    }    
}   
```	      

- *provide(object) to Widget*    
(provider를 위젯에 attach하기 전에, 우선 해당 provider를 attach할 위젯은, provider를 listen할 위젯들을 모두 포함하는 가능한 최상위 위젯이어야함.     
provider의 listner는 attach되어있는 위젯의 자식위젯에서만 선언될수 있으므로. 모든 listen하는 위젯들에 접근할수있는 상위레벨의 위젯에 attach.)     
provider를 attach하기위해선, attach할 위젯에 import 'package:provider/provider.dart';한 후, 위젯을 custom data provider(object)와 묶어    
하나의 provider로 관리하는 ChangeNotifierProvider으로 생성, 위젯의 data object를 create(3.xx버전이하는 builder)인자로 명시     
ex) 
```Dart
import 'package:provider/provider.dart';
@override
 Widget build(BuildContext context) {
    return ChangeNotifierProvider(  // 가장 많이쓰이는 provider위젯. Notifylistner로 변경됨을 알려줌.       
      create: (ctx) => Products(),// 해당 위젯에 provider를 생성해 attach    
      child: MaterialApp(...),
      }     
```      
provider는 non-object도 선언 가능. (String, int..) 단 위젯을 provider와 감쌀때, ChangeNotifier가 아니므로,     
Provider클래스로 위젯+provider를 생성하고, 이 provider는 일종의 global(const)변수처럼 사용 가능.     

- *provider의 Listner선언*     
한 provider의 listner를 특정 위젯에 attach하려면, 해당 위젯에서 package:provider/provider.dart를 import하고     
Provider.of<Products>(context)로 해당 provider 객체에 연결 및 data fetch. 이후, provider가 변할때마다(notifyLister호출시),연결이 선언된 이 위젯 re-build.     
(Provider클래스의 of(context) 메소드로, provider객체에 직접 접근. 이때, of메소드는 generic으로 <타입> 명시 가능. 앱내에 여러 provider가 있을수있으므로,              
현 위젯의 부모 위젯 중 Products객체 타입의 provider가 선언되어있는 가장 가까운 곳을 찾아 연결. 따라서, 반드시 provider는 상위 위젯에, listners는 자식위젯에 attach.    
provider를 선언한 위젯은 provider가 update되면, provider가 attach된 위젯의 자식위젯 중 listner가 선언되어있는 위젯만 re-build됨.        
ex)     
final productsData = Provider.of<Products>(context);    // provider(data class)에 연결 및 listen.
final products = productsData.items;  // provider내의 함수로 data fetching.     

- *첫 생성시에만 data fetching, provider update시 re-build하지 않는 경우*    
Provider.of()메소드는 listen: bool 인자 제공. provider의 리스너가 선언된 현 위젯이, provider의 notifyListners호출 시, re-build될것인지를 명시.    
디폴트값은 listen: true로 provider update(notifyListners)시 re-build. false는 provider가 바뀌더라도 변경될 것이 없는 위젯에 사용.    
ex) // Products중 선택된 product의 화면을 표시하는 스크린을 첫 생성시, // id로 선택된 product fetch. // 첫 생성시에만 provider에서 정보를 가져옴.            
final loadedProduct = Provider.of<Products>(context, listen: false).findById(productId); // 이후 Products가 update되도 re-build되지않음.     
// 즉, 다른 곳(다른 스크린)에서 Product를 추가하여 Products를 update해도, 해당 위젯은 살아있다면 변할게 없으므로 re-build되지않음.     
// 단, 이 product의 content를 직접변경하는일이 없는 경우           
    
- *provider의 소멸*    
ChangeNotifierProvider(및 create된 data객체)는 연결된 위젯(혹은 스크린)이 사라지는 경우 알아서 provider패키지에 의해 소멸 및 관리.     

- *nested Provider & ChangeNotifyProvider.value* (?)
provider를 list나 grid의 item으로 선언하는 경우, 해당 item이 화면 밖으로 가서 사라졌다가 다시 생성되는 경우,      
기존에 grid에서 item에 입력했던 변화를 반영하지 않는 문제 발생. 그 이유는,    
(추측,) list의 item 같은 위젯은 다시 rendering될때 이전에 썻던 위젯을 그대로 다시 사용한다. 즉, 해당 위젯의 data와 provider를 update하더라도,     
화면밖으로 나갔다 다시 오면, 기존값이 있는 새로운 provider를 다시 생성하고 변화 반영이 안됨.     
해결법: ChangeNotifyProvider.value로 item 생성.      
기존의 create으로 provder+위젯을 생성한 방식은, 위젯이 새로 생성될때마다 provider객체를 다시 초기화하는 방식으로, 일반적인 모든 경우에 권장.    
.value로 생성하는 방식은, 기존에 존재하던 객체를 다시 사용. 즉, grid의 item에 적합. 그리드 전체가 사라지기전까진 provider 객체 생존 및 재사용.      
ex) ChangeNotifierProvider.value(value: products[i], child: ...)     

- *consumer<T>*     
위젯을 listner로 선언하는 두번째 방법. Provider.of메소드로 연결을 선언하는것과 완전히 동일.    
 ex)
    ```Dart	
    Consumer<Product>( // 위젯이 provider를 listne하는 위젯임을 명시하는 위젯. generic으로 어떤 provider에 대한 연결인지 명시.      
            // builder로 (BuildContext, <T>, Widget) => Widget 메소드를 받음.     
            // <T> : 연결된 provider객체를 전달. child: 위젯이 re-build되어질때, re-build안될 위젯. 이 예제에선 사용안됨.  
            builder: (ctx,product, child) => IconButton(   // 위젯을 반환.
              icon: Icon(
                  product.isFavorite ? Icons.favorite : Icons.favorite_border),
              onPressed: () {
                product.toggleFavoriteStatus();
              },
            ), // IconButton
            child: Text('favorites'), // builder의 child로 전달될 위젯. 위젯 내에서 해당 child를 참조한 자식위젯이 있다면, 이 child는 re-build되지않음.
          ), // Consumer
    ```
다만, of메소드를 사용하는것에 비해 가지는 장점은, 위젯 트리상에서 특정 위젯의 build내에서 listen할 자식 위젯들이 존재한다면,      
현 위젯을 listen하게 선언하고 전체를 re-build하는 것보다 optimize하기위해 listen하는 자식위젯 부분들을 전부 따로 split해서 of메소드로 listen을 명시할수도 있지만,      
이 위젯의 build 코드 내에서 필요한 부분만 consumer로 감싸 선언 가능하다는 것. 예를들어, 현 위젯에서는 첫 생성시에만 provider에서 data를 받아 참조하여 생성되게끔,      
of메소드 내에서 listen:false로 선언하고, 현위젯 내의 자식위젯 중 provider업데이트시 다시 빌드될 위젯들만 consumer로 감싼다면,      
provider변화시 현 위젯은 re-build되지않고, 위젯 내의 필요한 부분만 update되고 다른 부분은 다시 빌드되지않음.         

- *MultiProvider*    
한 위젯에 provider를 여러개 attach하고싶을때, ChangeNotifierProvider(1개의 provider)가 아닌 MutliProvider클래스로 생성.      
ex)
```Dart	
MultiProvider(
       // 위젯에 attach할 provider를 명시
      providers: [    
        ChangeNotifierProvider(
          create: (ctx) => Products(),
        ),
        ChangeNotifierProvider(
          create: (ctx) => Cart(),
        ),
      ],
      child: Material(...), // 위젯 명시

```

- *initState& Provider.of(context)*      
일반적으로, initState에서 직접적으로 context를 사용하여 of메소드를 호출할수 없지만, provider의 경우, listen: false로 fetching만 하는 경우는 사용 가능.    
ex) initState(){ Provider.of<Products>(context, listen:false).fetchAndSetProducts(); super.initState();}      
단, listen: false가 없으면 다른 context 사용과 마찬가지로, 사용할수 없음. (-> fulture.delayed를 이용하거나 didChangeDependencies사용)          

- *ProxyProvider*      
다른 provider에 의존적인 provider. 의존하는 provider가 변할때, 해당 provider를 다시 생성 혹은 갱신. ChangeNotifierProxyProvider<T, ChangeNotifier>로 생성.    
ChangeNotifierProxyProvider<T,ChangeNotifier>( // 첫번째 generic으로 의존할 provider객체 명시. 단, 해당 객체는 provider tree(multi Provider)에서 먼저 선언되어있어야함.  
(가장 가까운 해당 provider객체를 찾아서 연결(의존).)   
// 두번쨰 인자로, 현재 생성할 provider의 클래스 명시.    
create: (ctx) => ChangeNotifier // 의존하는 provider의 변경시, 현 provider를 처음부터 다시 생성하는 경우의 builder함수. 다시 생성하지않고 update할거면 null로 명시.   
update: (ctx, dynamic, ChangeNotifier) => ChangeNotifier // 현 provider를 update하는 경우의 builder함수. 이전의 provider를 참조하여 새로운 provider를 반환.       
// 해당 함수의 두번째 인자로 의존하는 provider(방금 변경된)객체 전달, 세번째 인자로, 현재까지 존재한 이전의 provider객체 전달. 처음 proxyProvider를 생성하는 경우라면, null전달.      
// 함수는 이값들을 참조하여, 새로운 provider를 생성해 반환. 일반적으로, 이전의 값들중 그대로 사용할 값들은 provider의 생성자로 받아 저장.          

------------------------
# Http    
Http request 및 response를 받을수있도록 돕는 flutter패키지.    
tip) import 'package:http/http.dart' as http; 하여 (http.)으로 접근해 패키지 사용.(너무많은 이름의 메소드,클래스 등이 있어서 crash방지)   
tip) 만약, app data를 provider로 관리하고 해당 데이터로 서버와 interact하고자한다면, provider class내에서 http메소드 선언.(ex) add함수에서 http.post ..)    

- *Post*     
(http.)post(String(url), body: dynamic(JSON), headers: Map<String,String>) // 서버에 post, url을 positional 인자로 전달.   
(post메소드의 반환값은 Future<Response>. 즉, 서버에 post를 보내고 response를 받는 함수를 비동기적으로 실행(요청을 보내고 나서, 응답을 받을때까지 코드를 block하지않음.)     
response를 받으면 Future객체의 완료상태, (post().)then()와 같이 응답을 받은 후 실행할 명령 선언 가능.)     
(FireBase의 경우: post 요청은 body에 명시된 data(Json(Map))를 서버에(정확히는, url에 명시된 주소 + db주소 내에) 생성,     
response는, body: (json.){'name':String(post로 생성된 해당 db container의 unique key)}) 로 구성.)        

- *Response*   
http를 사용하는 서버로부터의 응답을 표현하는 flutter http에 포함된 클래스. (Response.)body,header, statusCode 등 참조 가능.           

- *patch, put, delete & catchError*
서버에 대한 요청시 받은 response의 statusCode가 400이상이면 오류가 발생한 것을 의미(요청 실패, 서버 오류 혹은 잘못된 url등.).     
http패키지의 post, get 메소드는 요청 후 응답에서 이 code값을 확인하여, 400이상이면 자동으로 error를 throw함. 그런데 delete 등은 statusCode가 400이상이어도, error를 throw하지않음.   
따라서 try{ await http.delete(url); } catch(error){ } 혹은 http.delete(url).catchError()와 같이 error catch불가능.     
해결법: throw custom exception & optimistic delete      
http.delete의 응답(res)을 await(혹은 then)을 통해 받아, res.statusCode를 확인하여, 400이상 시, error를 적절히 handling한 후, throw error.    
ex)
```Dart	
// 삭제할 product의 index와 객체를 참조해둠. //memory에서 삭제되지않게끔.
final existingProductIndex = _items.indexWhere((prod) => prod.id == id);
var existingProduct = _items[existingProductIndex];
_items.removeAt(existingProductIndex); // 현 List에서 삭제
notifyListeners();
// 서버에 삭제요청.
final res = await http.delete(url);
// 삭제 실패시
if (res.statusCode >= 400) {
  _items.insert(existingProductIndex, existingProduct); // List에 다시 추가. optimistic delete
  notifyListeners();
  throw HttpException('Could not delete product.'); // 실패시 error throw / 함수 종료
}
// 삭제 성공시, memory에서도 삭제.
existingProduct = null;
```
단, patch, delete뿐만 아니라 모든 http request메소드 실행시, 서버 요청오류가 아닌 인터넷 미연결 같은 오류는 자동으로 error를 throw.(위 코드에선 handling되지않음.)       
즉, 해당 오류를 catch하기위해선, try,catch로 handling해주면됨.(다시말해, 위 구문을 try로 감싸고 try내에선 statusCode를 확인해 복구, error발생시 try밖 catch구문 선언하여 복구.)        

- *Json*     
일반적인 http 통신 시 데이터에 사용되는 포맷으로, flutter http에선 body부분의 format. 따라서, Dart의 Map,List으로 data를 encode/decode가능.       
import 'dart:convert'; 후, (encode) json.encode(Map) // (decode) json.decode(Json)       

-----------
# Shared Preferences      
key-value형태로 data를 device내에 저장하도록 돕는 패키지. 일반적으로, auth의 token 등과 같은 복잡하지않은 data를 device에 저장할때 사용.     
패키지를 pubspec.yaml에서 get한 후 import 'package:shared_preferences/shared_preferences.dart'; 하여 사용.     

- *Future<SharedPreferences> SharedPreferences.getInstance()*      
SharedPreferences는 현 flutter앱과 SharedPreferences저장공간과의 연결을 관리하는 클래스. getInstance()메소드를 통해,      
연결을 생성.(SharedPreferences객체를 가져옴) 메소드는 Future를 반환하고, 가져온 SharedPreferences객체가 future에 resolve됨.    
즉, 해당 메소드를 async 내에서 await으로 호출 후 가져온 SharedPreferences객체를 저장 가능.     
ex) final prefs = await SharedPreferences.getInstance(); 로 저장해서 객체 내 메소드(저장, 로딩)를 호출.      

- *(SharedPreferences.)setString(String key,String value)*     
SharedPreferences객체는 디바이스에 데이터를 저장하는 함수인 여러 set함수를 제공. setDouble, setInt, setBool 등과     
data를 String인 key-val쌍으로 저장하는 메소드인 setString 제공.     
일반적으로, key는 사용자지정(해당 키로 나중에 데이터를 불러옴), value에는 저장하고자하는 데이터를 json으로 encode하여 저장.    

- *(SharedPreferences.)containsKey(String key)*      
저장된 key-value쌍 중 특정 key로 저장된 데이터의 유무 확인. bool반환.    

- *(SharedPreferences.)getString(String key)*      
SharedPreferences에 저장된 key에 해당하는 value값을 String으로 반환.(데이터 load)      

- *(SharedPreferences.)remove(String key)*     
해당 key의 데이터 삭제     

- *(SharedPreferences.)clear()*     
현app과 연결된 SharedPreferences저장 공간 전체 초기화.    
------------------
# sqflite       
SQLite를 flutter에서 사용할수 있도록 돕는 패키지. SQLite 명령을 사용하여 device내에 db를 생성 및 data insert, fetch 가능.         
tip) SQLite과 관련하여, data를 db와 interact하는 메소드를 별도의 class 및 파일에 선언 후, import하여 사용.   

- *methods*     
Future<String> getDatabasesPath() // db를 새로 생성할수있는 디바이스 저장공간의 path(String)을 반환. 일반적으로, 안드로이드와 IOS의 경우 고정된 저장공간 경로 존재.       
Future<Database> openDatabase(String path, {Future<void> (DataBase, int) onCreate, int version})      
// path에 명시된 db를 (version을 명시했다면, 해당 version으로) 열어 반환하거나(Database를 resolve),     
// 해당 path(경로+이름)에 같은 이름의 db가 없는 경우, 해당 이름으로 db를 새로 생성 후, onCreate를 호출하고, 생성된 db를 반환.     
// onCreate은 db가 처음 생성될때 호출되는 함수로, sql이 새로 생성한 db와 int(생성한 db의 version)전달. 일반적으로, 해당 함수 내에서, return db.execute()로 db를 초기화.     

- *Database*     
SQLite에 포함된 db클래스.     

Future<void> (Database.)execute(String) // db에 sql 명령어 수행.(ex) CREATE TABLE .. : db에 테이블 생성.) 명령 완료시, future이 done.      
// 자세한 명렁어는 https://pub.dev/packages/sqflite 혹은 SQLite문서 참조.    
    
Future<int> (Database.)insert(String, Map<String, dynamic>, {conflictAlogrithm: ConflictAlgorithm}) // db에 데이터 추가. id(?)반환.        
// 첫번째 인자로, table명(String), 두번쨰 인자로, 추가할 data(Map) // Map의 key목록은 db table의 key와 동일해야함. key가 존재하지않거나,
// conflictAlgorithm은 데이터 테이블에 이미 존재하는 id의 데이터를 추가할시, 어떻게 처리할지를 명시하는 enum.     

Future<List<Map<String, dynamic>>> (Database.)query(String) // db내의 특정 table(String)의 모든 데이터를 추출.    
// 데이터를 List<Map<String, dynamic>>의 형태로 추출.     
// 데이터중 일부, 혹은 특정 조건을 만족하는 필터링된 데이터만 추출하고자하는 경우:  공식 문서 참조.    

- *SQL 문법*  
CREATE TABLE : table 생성.      
REAL : int          
TEXT : String      

------------------------
## extra packages   
- *intl*    
help internationalization and localization facilities, including message translation, plurals and genders, date/number formatting and parsing, and bidirectional text.    
DateFormat.yMMd().format(dateTime) / DateTime객체를 pattern에 맞게 formatting하여 String으로 반환.    
DateFormat은 class로서 생성자 인자로 패턴(ex)yyyy-MM-dd)을 받거나 여러 pre-configured pattern을 named생성자(ex).yMMD())로 지정.   
DateFormat.E().format(dateTime) / DateTime을 요일만 표시(Mon,Tue,...).      
format은 class내부 메소드. date를 해당 패턴을 가진 String으로 반환.    

- *Image_Picker*      
device의 갤러리, 카메라 등을 접근해 사용하고, 이미지 파일을 가져올수있도록 돕는 패키지. 
// 안드로이드는 별도의 패키지 외에 별도의 configuration이 필요없지만, IOS의 경우 device feature를 사용하기 위해 permission을 받아야하며,      
/ios/Runner/Info.plist에 NSPhotoLibraryUsageDescription(갤러리 접근) 등의 키를 선언해야함.     
(Info.plist에 key등록 방법: <key>...</key>로 등록, 다음줄에 <string>...</string>으로 IOS의 경우 해당 permission을 제공할때, prompt해 디스플레이할 텍스트를 지정.)        

// 갤러리, 카메라 등으로부터 이미지를 가져오는 방법.      
'package:image_picker/image_picker.dart'를 import한 후, final picker = ImagePicker()와 같이, ImagePicker 객체를 생성하고, 
객체 내의 getImage()메소드 호출. 해당 호출은 future로 pickedFile객체(imagePicker내에 포함된 일종의 File클래스)로 resolve되기 전까지 await필요.      
ex) final imageFile = (picker.)getImage(    
source: ImageSource // 이미지를 어디서 불러올지 소스 지정. ImageSource는 enum으로 gallery or camera 포함.(소스에 따라 갤러리/카메라 접근.)     
maxWidth(maxHeight): double // (?) 이미지의 최대 가로(세로)길이        
)    
이후, 해당 file을 dart의 File객체에 저장하려면, File(imageFile.path)로 변환해 저장.      

- *path_provider & path*     
path_provider: device내의 app이 사용할수있는 저장공간에 접근(path 탐색 및 반환)할 수있게 도와주는 패키지.     
path: 파일 경로와 관련하여 파일 이름 추출과 같은 동작을 도와주는 패키지.     
     
// path_provider 내의 메소드    
getApplicationDocumentsDirectory() // app이 사용할수있는 device 저장공간인 directory를 future를 통해 반환. directory resolve까지 await필요.    
getExternalStorageDirectory() // sd카드 등의 디바이스 외부 저장공간 반환. 안드로이드에서만 사용가능.      
getgetTemporaryDirectory() // 디바이스 내의 일시적으로 사용가능한 저장공간 반환.     
(directory는 디렉토리를 나타내는 클래스로, 해당 공간으로의 경로는 (Directory.)path로 접근 가능.     
// pah내의 메소드    
basename(String) // path에서 파일명+확장명을 추출. (마지막 / 이후의 문자열)         
join(String, [String, String, ..]) // path경로(String)를 여러 서브 경로의 합치기('/' 자동 추가)로 생성. ex) join('main/lib','example.txt');     

ex) 두 패키지를 사용하여, file을 device의 hard drive에 저장. (tip: device에는 app이 사용할수있는 공간이 별도로 있고, app이 삭제되면 자동으로 연결된 데이터가 삭제됨.)     
```Dart	  
// path_provider.dart를 syspaths로 import, path.dart를 path로 import함.    
final appDir = await syspaths.getApplicationDocumentsDirectory();  // device 저장공간 탐색    
final fileName = path.basename(_storedImage.path); // 현재 메모리에 저장된 이미지의 File의 경로로부터 해당 이미지의 (카메라로 찍었다면 카메라가 자동생성한)이름 추출.    
final savedImage = await _storedImage.copy('${appDir.path}/$fileName');   // 메모리의 이미지를 device저장공간에 같은 이름으로 복사.      
```

- *location*     
디바이스의 위치서비스를 이용하여 현재 위치를 앱에서 얻을 수 있도록 돕는 패키지. (?) 사용시 update settings.gradle. 문제    
IOS의 경우, info.plist에 NSLocationWhenInUseUsageDescription, NSLocationAlwaysUsageDescription 추가 필요.           

Location().getLocation(); // 위치 가져오기. Location 객체를 생성 후, getLocation()호출. Future를 반환, LocationData로 resolve됨    
// LocationData는 위치 정보를 표현한 클래스로, latitude(double), longitude(double)를 포함.     
ex) final locData = await Location().getLocation(); print(locData.latitude);     

- *google_maps_flutter*     
구글맵 지도를 띄워주는 패키지. 사용하기 위해선 os별 별도 configuration필요 (android: manifest, IOS: info.plist). README참조.          
맵을 표현하는 위젯 GoogleMap을 생성하여 사용.    
GoogleMap( // GoogleMap은 부모 위젯의 height, width가 있어야함.      
    initialCameraPositionL CameraPosition( // 맵을 생성할때 초기의 위치 지정. CameraPosition객체로 지정.         
        target: LatLng( 12.0, 34.91), // 위도 고도로 위치 지정. LatLng은 위도와 고도로 위치를 표현하는 클래스.          
        zoom: double, // 맵의 zoom크기 지정.      
        tilt: double, // 맵의 방향 지정.    
    onTap: (LatLng){} // 맵을 터치할때 호출할 함수 명시. 함수에 해당 지역의 LatLng전달. 함수 내에서 해당 터치 지역의 위치를 저장할수 있음.   
    markers: Set<Marker> // 맵에 표시할 마커의 목록(set)명시. onTap에서 setState을 호출하고, 마커의 위치를 tap마다 생성할 수 있음.    
    // Marker(markerId: MarkerId('m1'), position: LatLng) // Marker는 마커를 표현하는 클래스로, markerId는 unique하게 생성해야하며(Set), position에 위치지정하여 생성      
)
