# working with web server+database    
직접 db와 연결하는것보다(코드 노출 가능), web server를 통해 db와 interact하는게 더 안전.    
device에 제한되지않는 data를 저장해야할때. internet필요.     
일반적으로, REST API(endPoint(URL)+verb(method))를 사용하는 server에 HTTP request를 보내서 server(+dataBase)와 interact.    

- *optimistic update.*     
delete, update 등 현 memory상의 처리와 서버 상의 처리 진행. 이때, 서버상의 처리의 오류 발생시(device 인터넷 끊김, 서버 오류 등.)    
memory상의 처리를 복구하는 작업을 optimisitic update라고함.   
예를 들어, 상품 삭제시, 앱 메모리 상의 상품 삭제 후, 서버 상의 상품 삭제. 이때 서버에 대한 요청 실패시, 메모리 상에 다시 상품을 로드하는 것.   
(물론, 서버 요청 처리 실패에 대한 error handling도(앱 내에 실패 메시지를 표시하는 등.) 같이 구현 가능.)    

- *push notification*      
앱의 push 알람은 push notifiaction 서버를 사용해 구현. (남용과 안전문제 방지)     


# FireBase      
google fully-managed backEnd API (database, file storage, authentication, server-side code(cloud-function) etc. )       

- *Firbase를 flutter app과 연동(Firebase sdk설치)*      
firbase 프로젝트에 앱 추가(OS별 app추가. 같은 작업을하는 안드로이드, IOS앱을(두 os를 지원하는 플러터앱의 경우 둘다 필요) 하나의 Firebase프로젝트에 연결)       
추가하는 법: https://firebase.google.com/docs/flutter/setup?hl=ko&platform=android    
(IOS의 runner에 GoogleService-info.plist추가시 파일을 직접 저장하는것 외에, xcode에서 앱의 ios프로젝트를 열어 Runner에 직접 add file해야함.)     

# DB: cloud_firestore     
앱과 연동된 firebase의 firestore를 접근 및 사용 등 db관리를 돕는 패키지.     
flutter 앱에 cloud_firestrore 패키지 설치 및 앱과 연동된 firebase프로젝트에 cloud_firestore생성. (개발시 test mode)       

- *DB 구성*    
collection(table) - documents(entries, Map) 로 구성. 각 document는 고유 id가 존재하며,       
각 document는 필드-값을 가질수있음. (flutter에선 key-value의 Map과 동일시. 저장 및 전송시 Map처럼 사용. ['key']로 접근.)   
또한, 각 document는 다시 sub collection들을 가질수있음. (documents는 flutter에선 List와 동일시 [i]로 document접근)  
ex) chat app: 최상위 chats collection이 chat room document들을 가지고 각 chat room이 messages collection을 가지고     
message collection내에 단위 String 메시지를 document로 구성.    

- *access to firestore*      
cloud_firestore 패키지에 포함된 Firestore.instance로 연동된 firebase의 firestore에 access. 항상 생성된 인스턴스(내 Firebase entry)를 통해 여러 메소드 사용.     
ex) Firestore.instance().collection...     

- *access to collection*
(Firestore.)collection(String) // store내 collection접근. 최종 collection 경로 명시. (/로 forwarding) ex)'chats/(document key)/messages' 
접근한 collection을 CollectionReference객체로 반환.     

- *Collection*
(CollectionReference.)snapshots() // collection의 Stream객체를 반환. stream을 통해 collection 참조 및 변화가 있을때마다 자동으로 감지 가능.     
(반환된 Stream객체에서 listen 호출 가능. : .snapshots().listen((){}) // collection(정확힌, Stream) 변화시마다 호출 할 함수를 명시. 해당 함수에 가장 최근 Snapshot     
 (정확히는, firestore에서 제공되는 QuerySnapshot(collection의 snapshot)) 전달.  // (QuerySnapshot.)documents로 collection내 documents(list형태)접근 가능
Future<> (CollectionReference.)add(Map<String,dynamic>) // collection에 document추가. document id는 자동으로 생성. Map의 key-value가 필드-값으로 추가됨.     
(CollectionReference.)document([String path]) // Collection내 document에 접근. DocumentReference객체 반환. document id가 존재하지않던 id거나 혹은    
// document id(path)가 명시되지않은 경우(null), 명시한 새로운 id의 document를 참조 혹은 자동으로 id를 생성한 document를 참조하는데,     
// 단, 실제로 생성하는 것은 아님. documentRefence의 setData등으로 데이터를 생성해야, 해당 id로 document가 자동생성됨.     
(CollectionReference.)orderBy(dynamic field, {bool decending=false}) // 접근한 collection의 data들을 특정 필드의 값을 기준으로 정렬한 collection을 반환.     
// ex) 각 documents에 'createdAt'이란 필드가있고, TimeStamp가 값이라면, timeStamp기준으로 정렬하여 collection반환 (document[0]이 가장 빠른 timeStamp)      
// decending: true로 명시할 시 정렬한 것을 다시 역순으로 반환. (document[0]이 가장 느린 timeStamp)     
// Firestore에 정렬한 collection을 새로 생성하는 것이 아닌, store내의 collection에는 변화가 없고, 정렬하여 반환하기만함. 같은 collection을 의미하므로, snapshot()으로 stream생성 등 가능.    

- *document*    
Future<> (DocumentReference.)setData(Map) // 해당 document에 데이터 추가(Map). 이때, documentReference가 존재하지않던 id의 document면    
// firestore에 document를 새로 생성해 추가.    
Future<DocumentSnapshot> (DocumentReference.)get() // 해당 document의 data를 get. Future로서, DocumentSnapshot으로 resolve. 해당 snapshot은 Map처럼 [''] 참조가능.   
 
- *Rules*     
Firestore에 접근하는 요청에 대한 규칙들을 규칙(Rules)탭에서 정의 가능. db에 대한 read, create, write등이 가능한 조건들을 가능.    
자세한 사용법: https://firebase.google.com/docs/rules/rules-and-auth?authuser=0     
ex)    
match /users/{uid}{      
  // users collection의 모든 document(uid)에 대해 write은 동일한 user에 의해서만(요청에 user가 authenticated되어있으며, 해당 user의 uid가 document와 일치하는 경우만) 가능.     
  allow write: if request.auth != null && request.auth.uid == uid;    
}   
match /chats/{documents=**}{    
  // chats collection내에 존재하는 모든 (하위의)documents들에 대해 read,write(=create, update, delete포함)은 authenticated된 user만 가능.       
  allow read,write: if request.auth != null;    
}    

- *TimeStamp*     
firestore 패키지에 포함된 클래스로, datetime과 비슷한 timestamp를 나타내는 클래스. TimeStamp.now()로 현재 timeStamp생성     
데이터를 store에 추가할 때 시간이 필요한 데이터인 경우 사용. 
ex) chat의 각 message document의 필드에 'createdAt' : TimeStamp.now()를 추가해 명시. 해당 필드로 message들을 불러올때,    
orderBy로 시간순으로 정렬 가능.

# auth: Firebase_auth     
앱 및 연동된 Firebase에 auth를 관리할수있도록 돕는 auth관리 패키지. 별도의 auth state management(ex) provider)필요x        
여러 auth관련 sdk사용을 돕는 패키지인 firebase_auth 설치.

- *access to FirebaseAuth*      
FirebaseAuth.instance로 앱과 연동된 Firebase Auth에 access. 생성된 FirebaseAuth 인스턴스를 통해 여러 메소드 사용.

- *signUp,signIn,signOut*     
(FirebaseAuth.)signInWithEmailAndPassword(email: ,password: ),     
(FirebaseAuth.)createUserWithEmailAndPassword(email: password: ),     
(FirebaseAuth.)signOut()
등의 메소드로 signUp/In, signOut     
해당 메소드는 future로 내부적으로, firebase에 signIn,signUp요청. 결과로서, AuthResult객체로 resolve.

- *Error handling*     
signIn,signUp 등 시 발생하는 에러를 처리. invalid한 email입력 등과 같은 에러는 요청 메소드에서 PlatformException 타입의 error를 throw. 즉, try,catch로 처리.       
// PlatformException에러는 Firbase_auth에 포함된 exception으로 err.message를 통해 error 메시지 확인 가능. (invalid Email등)    
이런 부류의 error를 catch하기위해 on PlatformException으로 catch하여 handling(ex) err.message가 null이 아니면 pop-up).     
그 외의 인터넷 끊김등으로 발생하는 에러도 catch하여 적절한 handling 필요.     

- *AuthResult*     
Auth요청에 대한 응답을 표현한 클래스.   
(AuthResult.)user // FirebaseUser반환. signUp/In한 user에 대한 정보 참조 가능.    
// (FirebaseUser.)uid 로 해당 user에 자동생성된 unique한 id를 참조 가능. 해당 id를 사용해, firestore에 해당 유저에 대한 extra정보를 저장하거나, 관리할수있음.    
// (ex) users콜렉션 내의 document id를 user의 uid로 하고 해당 document에 유저의 정보 저장.)    

- *onAuthStateChanged*     
FirebaseAuth.onAuthStateChanged는 현재 auth상태(정확히는, FirebaseUser)를 확인할수있는 Stream을 생성해 반환. auth상태 바뀔때마다 event발생.              
즉, 해당 Stream의 snapshot.hasData(FirebaseUser != null인지)를 확인하여, 로그인된 유저가 존재하는지 확인 가능.    
(내부적으로는, FirebaseUser 구성을 위해 현재 auth에 토큰이 있는지, 있다면 토큰이 유효한지 등을 모두 검사하는 과정 포함하기 때문.)   
ex) 스크린을 auth상태가 바뀔때마다 자동으로 렌더링 가능. (로그아웃 등으로 변할때마다 혹은 앱을 재시작했을 시 등)    
```Dart	  
 StreamBuilder(
        stream: FirebaseAuth.instance.onAuthStateChanged, // Stream<FirebaseUser>반환
        builder: (ctx, userSnapshot) { // 빌더함수, auth변할때마다 호출.
          if (userSnapshot.hasData) { // User가 null이 아닌지 확인. (authenticated user가 있는지)   
            return ChatScreen();
          }
          return AuthScreen();
        },
      ),
```

- *currentUser*      
단순히, 현재 유효한 로그인되어있는 유저의 정보를 알고싶을떈, (FirebaseAuth.instance.)currentUser() 호출. 해당 함수는 Future로     
await하면, 현재 로그인되어있는 FirebaseUser로 resolve됨. 해당 FirebaseUser객체를 통해 uid 등을 참조 가능.    

# storage: firebase_storage
Firebase의 storage(file 저장 및 관리) 사용을 돕는 패키지.     
cloud_firestore과 마찬가지로, 사용 권한에 관한 rule(규칙탭)지정 가능. 

- *bucket*     
bucket은 storage의 폴더 단위를 의미. bucket내에 file저장 가능. bucket은 sub-bucket을 가질수있음.     
FirebaseStorage.instance.ref()로 최상위 bucket(루트)에 access 가능.     

- *StorageReference*     
storage의 모든 폴더 및 파일은 StorageReference로 참조 가능. (ex FirebaseStorage.ref()는 루트 버킷)      
각 StorageReference는 다시 (StorageReference.)child(String path) 로 forwarding 가능. 함수 인자로 현재로부터 forwarding할 경로 명시.    
(StorageRefrence.)child()는 현 포인트의 child를 다시 StorageRefence로 반환. 즉, 파일 경로를 child로 순차적으로 chaining가능.    
이때, 해당 경로 도중 폴더(버킷) or 파일이 존재하지않는다면, 자동적으로 새로 생성. (단, ref에 실제로 putFile()을 통해 파일을 추가할때 생성됨.)     

- *파일 추가*    
(StorageReference.)putFile(File,[StorageMetaData]) // 해당 reference에 파일 추가. reference는 파일의 경로 및 최종 파일의 이름까지 명시되어있는 reference여야함.      
ex) Firebase.instance.child('user_image').child(authResult.user.uid+'.jpg').putFile(image);  // id.jpg의 이름으로 이미지 파일을 추가.     
// StorageMetaData객체는 optional로 파일의 메타데이터 지정 가능.(파일의 인코딩 언어 등)      
// putFile은 asynchronous하게 업로드하고, 함수는 StorageUploadTask를 반환. Future를 생성해 기다리고싶다면, 해당 작업을 완료하는것을 기다리는 (StorageUploadTask.)onCompleate 생성.    
// ex) await await ref.putFile(image).onComplete; // onComplete은 Future를 반환. uploadTask가 완료되면 done.       

- *경로 다운로드*     
Future<dynamic> (StorageReference.)getDownloadURL()() // 해당 reference(폴더 or 파일)의 url을 retreive. Future로 await을 통해 url(String)로 저장 가능.    
이미지 파일인 경우 storage에 저장하고, 직접 fetch하지않고, url을 통해, 이미지 참조가 가능. 즉 예를들어, user의 프로필 이미지를 가져오는 경우, 전체 stroge에서 해당 uid의 이미지를 탐색해 
 가져올 필요 없이, user 대한 document에 image_url 필드를 추가하여, 해당 필드의 값으로 이미지를 가져올수 있음.     

# Firebase Clouod Messaging(FCM) & Firebase_messaging    
push 알림을 돕는 서비스. 푸시알림은 서버에 메시지와함꼐 푸시알림을 요청하면, 내 앱을 가진 유저의 디바이스에 푸시알림을 서버가 별도로 보내줌.     
안드로이드의 서버를 활용한 푸시알림은 FCM을 반드시 사용해야하고, ios의 경우 ios push 서비스를 내부적으로 빌드해줌.     
메시지 구분: notification message(일반적인 푸시알림, 앱을 사용중이지않아도 알림이뜨고, 누르면 앱으로 이동) / data message (앱을 사용하고있는 동안 데이터 업데이트등을 알리는 메시지)       
안드로이드 에뮬레이터의 경우, 푸시 알람을 받기위해선, 구글 플레이가 설치되어있는 시스템이어야함.       
firebase_messaging은 flutter app에서 FCM 서비스를 접근 및 관리할수 있도록 돕는 패키지.      

- *Firebase_messaing setup*     
pub.dev의 패키지 README에 명시된 set up을 선행해야함. firebase와 앱을 연동해야하고 푸시알림을 탭하여 앱에서 handle할수있도록 setup 4번 과정 진행.(manifest파일 변경)      

- *cloud_messaging*     
firebase console의 cloud_messaging서비스를 통해 firebase console에서 직접, 모든 앱사용자에게 푸시알람을 발송 가능.      
단, 메시지가 안드로이드앱에 전송될때는, 메시지의 추가 옵션에 click_action: FLUTTER_NOTIFICATION_CLIC이 포함되어야      
앱내에서 푸시알림을 눌렀을때의 작업을 처리할수있음.(onResume,onLaunch등의 실행.)(푸시 handling작업이 없다면 물론 필요없고 탭 시 단순히 앱으로 이동됨.)     

- *Message receiving*    
IOS의 경우(안드로이드는 필요없음.), 앱내에서 디바이스에 push알림의 사용허가를 받아야함.       
푸시알림을 받기 허용할 클래스(위젯) 내에서 firebase_messaging을 inport 후,(main.dart 혹은 예를들어 로그인된 경우만 푸시를 받을 것이면, chatScreen의 initState 함수 내에서)     
FirebaseMessaging()으로 인스턴스를 생성하고, 인스턴스에서 (FirebaseMessaging.)requestNotificationPermissions()으로 허가 요청.     
(FirebaseMessaging.)configure()로 받는 푸시 메시지에 대한 처리 가능. (안드로이드의 경우도, 해당 함수로 메시지를 handling.)     

ex) 
```Dart
inal fbm = FirebaseMessaging();
    fbm.requestNotificationPermissions();
    fbm.configure(onMessage: (msg) { // 각 메시지의 처리. 알림의 configuration이 msg로 전달됨. 해당 함수들은 future를 반환해야함.
      print(msg);
      //..
      return; // future로 처리하지않을거면 단순히 반환.
    }, onLaunch: (msg) {
      //..
      return;
    }, onResume: (msg) {
      //..
      return;
    });
    fbm.subscribeToTopic('chat');
```
onMessage : // push가 앱이 실행되고있는 도중 오는 경우 실행. 물론 push알림이 뜨진않음.    
onResume : // push가 앱이 중단상태일때 오는 경우, 탭하면 앱이 resume되면서 호출.     
onLaunch : // push가 앱이 종료상태일때 오는 경우, 탭하면 앱이 시작되면서 호출.        
(FirebaseMessaging.)subscribeToTopic(String) // 특정 topic을 subscribe. 해당 토픽으로 명시되어 전달된 알림을 현 디바이스에서 받음.        
(FirebaseMessaging.)getToken() // device의 token을 생성. 해당 정보를 firestore에 따로 저장하고, cloud function에서 token을 바탕으로 특정 device에만 push알림을 보낼수도 있음.    

- *push를 firebase프로젝트의 event에 따라 자동적으로 생성하는 방법. : Firebase의 Functions 사용*     
Firebase의 firestore에 데이터터를 저장할때마다 해당 데이터를 바탕으로 푸시알림을 하고자하는 경우, Firebase(server side)의  cloud function을 사용.     
firestore의 변화시마다, 자동으로 trigger되어 푸시알림을 생성하는 자바스크립트 코드를 앱 프로젝트내의 function내에 작성하여 firebase function에 게시.     
firebase Function을 게시하기위한 순서: (firebase tool 설치) -> (firebase login) -> 내 앱 프로젝트내에 명령어, firebase init -> 사용할 feature로 function선택     
-> 사용할 프로젝트 선택(Use an existing project)에서 내 앱 프로젝트 선택 -> 언어 javascript선택 -> bug catch yes -> dependencies 설치 yes(function코드에 사용할 패키지 포함)       
위 단계를 통해, 사용할 firebase functions 폴더가 내 프로젝트내에 생성됨. "index.js"에 function에 게시하고자 하는 함수의 코드를 작성한 후,    
firebase deploy명령어를 입력하면 함수가 firebase에 로드됨.    

- *Firebase Function(javascript) & firebase-admin*       
function에서 firebase의 event가 trigger하는 함수를 작성. 프로젝트에서 messaging을 보내기 위해 admin패키지 사용.(dependencies를 설치했다면 자동으로 설치됨.)     
FireStore로 function을 trigger : cloud function이 firestore에 의해 trigger되고자한다면, onCreate을 통해 해당 collection/document에 데이터가 입력될때마다 실행함 함수를 명시.        
onCreate등 사용의 자세한 코드는 문서 참조: https://firebase.google.com/docs/functions/firestore-events?hl=ko    
admin 패키지는. 파이어베이스에 대한 권한을 관리하는 sdk로, 해당 sdk를 통해 파이어베이스의 messaging(푸시 알림) 이용 가능.     
ex) firestore에 create할때마다, admin을 통해 firebase에서 알림 전송.     
```JavaScript    
// deploy할 함수 //
const functions = require("firebase-functions");
const admin = require("firebase-admin"); // 패키지 import    
admin.initializeApp(); // 초기화 필요.
exports.myFunction = functions.firestore
    .document("chat/{message}") // listen할 곳의 path명시 chat collection내의 모든 문서. {message}는 place-holder로 chat내의 모든 document의미.(message는 사용자 지정 이름)
    .onCreate((snap, context) => { // 데이터가 추가될때마다 호출.
      return admin.messaging().sendToTopic("chat", { // messaging()을 통해 'chat'이라는 이름의 topic에 push(message) 전송.
        notification: {   // payload(push알림의 데이터) 명시 // notifiacation 메시지(일반적인 푸시메시지)의 구성 명시.
          title: snap.data().username, // 저장된 데이터의 username필드값
          body: snap.data().text, // 저장된 데이터의 text필드값
          clickAction: "FLUTTER_NOTIFICATION_CLICK", // 안드로이드 탭을 위한 key-value
        },
      });
    });
```

# FireBase REST API without SDK.    

프로젝트와 app의 연동(sdk 및 플러그인 설치) 필요없이, http만 사용해 db서비스 사용 가능.    
단순 rest api를 지원하는 realtime DB사용.     

- *realtime DB생성.*    
간단한 서버+db생성 및 flutter에서 사용.    
Firebase -> 프로젝트 생성 -> realtime DB생성

- *Post*      
flutter에서 (http.)post(url, body:...,); 시, url에 firebase의 realtime DB(server+DB) 주소+ .json 입력.     
ex) const url = 'https://flutterudate-default-rtdb.firebaseio.com/products.json'; 와 같이, ( main주소 및, /products.json )      
endPoint(서버주소)뿐만 아니라, /로 접근or새로 생성할 db폴더 forwarding하고 .json으로 encoding 명시.(?)      

- *get*      
flutter의 http.get(url)으로 요청, response로 data 전달. response의 body에 해당 url이 나타내는 db내 data를 (key(ID)-value(data)형태의 Map) json으로 전달.   
body로 받은 데이터를, decode하여 Map<String, dynamic>으로 저장가능. 해당 db경로에 data가 없다면, 응답 body는 null.    
ex) final extractedData = json.decode(response.body) as Map<String, dynamic>; (Id가 나타내는 data가 Map인 경우(nestedMap)에, dynamic으로 받아야 error가 나지않음.)         

- *patch*       
flutter에서 http.patch(url, body:...)으로 요청. 해당 내용으로, url의 db를 update. 예를들어, products의 특정 한 item update시, (item 정보가 Map<String, obejct>인 경우)      
url = 'https://.../products/$id.json'로 요청.(products폴더 내의 해당 item접근 (id로 접근))       
이때, 메시지의 body로 새로운 title, description등을 입력하여(Map)(json으로 encode) update요청. 입력된 정보로서,        
해당 item의 기존에 없던 key의 정보가 들어오면, 새롭게 추가, 기존의 key에 대한 값이 들어오면 해당 값으로 value update. 명시되있지 않은 기존에 있던 나머지 key에 대한 데이터는 유지.      

- *put*     
해당 url내의 db를 요청한 body로 update. patch와 다른 점은, patch는 body로 명시된 내용 중 다른 부분을 update하고 명시되지않은 부분은 유지하지만,    
put은 해당 경로의 data를 요청한 data로 전체 변경. 즉, key-value가 기존에 어땠는지 상관없이 새로운 data로 교체.     

- *delete*    
flutter에서 http.delete(url)로 요청. 해당 url이 지칭하는 db 컨테이너를 삭제.     

- *url+query*       
url뒤에 서버에 요청하는 query를 '?' 와 '(key)=(val)'의 형태로 전달. 명시할 query가 여러개인 경우 &로 구분.   
ex) url = 'https://flutterudate-default-rtdb.firebaseio.com/products.json?auth=$authToken&$orderBy="creatorId"&equalTo="$userId"';      

- *request with token*    
authentication(token)이 필요한 요청의 경우, fireBase는 url뒤에(.json뒤) '?auth=(String)'로 token을 명시.      
(다른 http api의 경우, 요청의 header에 token을 명시하는 경우도있음.) 해당 token이 유효한 경우(authenticated)만, 요청에 맞는 처리.    

- *filtering data from Server*     
data를 노드에서 fetch할때, 전체 data 중 특정 조건을 만족하는 일부만 fetch하고자하는 경우, fireBase server는 filtering지원.    
우선, realTime Db의 rules에, 특정 노드의 index화 추가. 
ex) "products": { // filtering할 노드
      ".indexOn": ["creatorId"]  // index기준인(?) 노드의 key 명시(filtering할 key)
    }
그 후, data 요청 시, url의 query에 parameter로 orderBy="creatorId"&equalTo="$userId" 추가.    
해당 요청을 받으면 서버단에서, 데이터를 creatorId 키의 value가 userId와 일치하는 항목만 fetching.       


# FireBase Auth REST API without sdk    
FireBase에서 제공하는 authentication API. 이 서버를 사용해 내 앱(내 firebase 서버,db)에 대한 auth관리 가능. (token사용)     
https://firebase.google.com/docs/reference/rest/auth#section-create-email-password 참조.    

- *endPoint에 내 서버의 키를 명시.*    
내 api키: Firebase 프로젝트 -> 설정 -> 내 웹 API키     

- *post로 auth요청*       
내 서버의 키를 포함한 url로 post 요청, signUp, signIn 등인지도 urlSegment로 구분.      
email,password등으로 로 sing up/in 가능, email로 요청시 body에 email, password, returnSecureToken(bool) 명시.      
response에는 body에 error여부, 성공시 idToken 등 포함. 해당 정보를 앱에서 처리. (ex)로그인 성공시, 토큰 저장 및 화면 전환 등)      

- *authentication error*     
authentication에 관한 error 발생시 (sign up시 이미 존재하는 email, sign in시 잘못된 비밀번호, 존재하지않는 email 등 )    
응답메시지의 statusCode가 400이 아니라, body에 'error'(key) 와 error에 대한 내용을 (value)로 응답.(statusCode는 200)    
즉, flutter의 http 패키지내의 post메소드로 요청을 보내더라도, post메소드에서 자동으로 error를 throw하는 statusCode가 400이상인 경우가 아니므로,
따로 response body의 'error'키 유무와, 해당 키의 value(Map형태)를 확인하여 error를 확인 및 handling해야함.(['error']가 null이면 정상적으로, sign in/up)    
'error'(key)에 대응되는 value는 Map(nested)형태로 이 Map내의 'message'키에 String으로 에러메시지 포함.       
에러메시지는 'EMAIL_EXISTS', 'INVALID_PASSWORD' 등 존재. firebase auth rest api문서 참조.     
일반적으로, authentication error를 따로 handling하기 위해, 해당 에러 확인 후, 에러 내의 message를 포함하는 custom Exception을 throw.   
요청을 호출한 외부 함수에서 해당 custom Exception을 catch하여 (on 키워드로) exception내용에 따라 적절한 처리. (error를 dialog로 표시 등.)    


# Authentication with token in Flutter    
session / token base (?)     
- *token방식*     
서버로부터 token을 받아 flutter app내에 저장. (일반적으로, app이 꺼져도 소실되지않기위해 device storage에)      
token을 사용해, 서버에 valid한 유저가 logged in됨을 인증.   

- *working with provider*    
앱 전체 혹은 여러 많은 부분에 걸쳐 auth에 따른 서로 다른 작업을 위해, provider로 현 auth 상태를 관리.      
auth 객채 내에, sign up, in메소드 선언, 현재 token 및 expiryDate 저장.     
일반적으로, 서버에 대한 응답으로부터, token, expiryDate, _userId(로컬id) 저장.

 -*sign up/in*     
서버로 auth 요청(일반적으로, http). 해당 요청에 대한 처리(Future의 결과)를 바탕으로, auth정보(token,expiryDate등) 저장,    
이후 메소드를 호출한 위젯에서 상황에 맞게 각종 페이지 전환 및 error handling.   
즉, 메소드를 async로 선언하여 요청을 await후, 응답에 대한 처리.    

- *현재 logged-in 여부 결정*     
token이 존재하고, 만료되지않았다면, 서버로부터 auth를 인증받은 상태 즉, 로그인된 상태.   
저장한 token이 null이 아니고, 저장한 만료까지의 기간이 지나지 않았다면, 로그인되어있음을 결정하는 로직 구현.  
ex) auth provider내에 
```Dart	
bool get isAuth {
    return token != null;
  }
  String get token {
    if (_expiryDate != null &&
        _expiryDate.isAfter(DateTime.now()) && // 현재 만료되었는지 확인.
        _token != null) {
      return _token;
    }
    return null;
  }
```
