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

------------------------
## extra packages   
- *intl*    
help internationalization and localization facilities, including message translation, plurals and genders, date/number formatting and parsing, and bidirectional text.    
DateFormat.yMMd().format(dateTime) / DateTime객체를 pattern에 맞게 formatting하여 String으로 반환.    
DateFormat은 class로서 생성자 인자로 패턴(ex)yyyy-MM-dd)을 받거나 여러 pre-configured pattern을 named생성자(ex).yMMD())로 지정.   
DateFormat.E().format(dateTime) / DateTime을 요일만 표시(Mon,Tue,...).      
format은 class내부 메소드. date를 해당 패턴을 가진 String으로 반환.    
