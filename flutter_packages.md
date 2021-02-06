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
특정위젯에 attach된 provider(data container)를 지정하면, 해당 위젯의 자식위젯들은 이 provider에 대한 listner를 선언 가능.      
그리고 이 위젯의 provider의 상태가 변하면(update), 모든 자식위젯을 다 빌드하는 것이 아닌, listner가 선언되이있는 자식위젯들만 build.      
provider는 어느 위젯에도 선언될수 있고, 같은 위젯에 여러개의 provider를 선언할수도 있음.    

- *provider선언*     
provider는 data container로서 우선 provider가 될수있는 class를 선언.(data를 원하는 형태로 담아둔 class)     
이때, 이 클래스(provider)를 listen하는 모든 자식 위젯들에게 해당 객체가 변경되었음을 알리기 위한 함수 notifyListner()를 사용하기 위해     
flutter패키지에 포함된 클래스인 ChangeNotifier를 이 클래스에 mixin. 그리고, 해당 클래스내의 특정변수를 변경하는 함수를 따로 선언하여, 변경 후    
notifyListner()를 선언함.    
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
        notifyListeners(); // 이 객체의 변수를 변경 후, listner들에게 알린다.    
    }    
}   
```	      

- *attach provider*    
(provider를 위젯에 attach하기 전에, 우선 해당 provider를 attach할 위젯은, provider를 listen할 위젯들을 모두 포함하는 가능한 최상위 위젯이어야함.     
provider의 listner는 attach되어있는 위젯의 자식위젯에서만 선언될수 있으므로. 모든 listen하는 위젯들에 접근할수있는 상위레벨 위젯에 attach.)     
provider를 attach하기위해선, attach할 위젯에 import 'package:provider/provider.dart';한 후, 위젯을 provider와 묶일수있게하는 위젯인     
ChangeNotifierProvider으로 감싼 후, 위젯의 provider를 create(3.xx버전이하는 builder)인자로 명시     
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

- *provider의 Listner선언*     


------------------------
## extra packages   
- *intl*    
help internationalization and localization facilities, including message translation, plurals and genders, date/number formatting and parsing, and bidirectional text.    
DateFormat.yMMd().format(dateTime) / DateTime객체를 pattern에 맞게 formatting하여 String으로 반환.    
DateFormat은 class로서 생성자 인자로 패턴(ex)yyyy-MM-dd)을 받거나 여러 pre-configured pattern을 named생성자(ex).yMMD())로 지정.   
DateFormat.E().format(dateTime) / DateTime을 요일만 표시(Mon,Tue,...).      
format은 class내부 메소드. date를 해당 패턴을 가진 String으로 반환.    
