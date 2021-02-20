# working with web server+database    
직접 db와 연결하는것보다(코드 노출 가능), web server를 통해 db와 interact하는게 더 안전.    
device에 제한되지않는 data를 저장해야할때. internet필요.     
일반적으로, REST API(endPoint(URL)+verb(method))를 사용하는 server에 HTTP request를 보내서 server(+dataBase)와 interact.    

- *optimistic update.*     
delete, update 등 현 memory상의 처리와 서버 상의 처리 진행. 이때, 서버상의 처리의 오류 발생시(device 인터넷 끊김, 서버 오류 등.)    
memory상의 처리를 복구하는 작업을 optimisitic update라고함.   
예를 들어, 상품 삭제시, 앱 메모리 상의 상품 삭제 후, 서버 상의 상품 삭제. 이때 서버에 대한 요청 실패시, 메모리 상에 다시 상품을 로드하는 것.   
(물론, 서버 요청 처리 실패에 대한 error handling도(앱 내에 실패 메시지를 표시하는 등.) 같이 구현 가능.)    

# Authentication     
session / token base (?)     
- *token방식*     
서버로부터 token을 받아 flutter app내에 저장. (일반적으로, app이 꺼져도 소실되지않기위해 device storage에)      
token을 사용해, 서버에 valid한 유저가 logged in됨을 인증.   

- *working with provider*    
앱 전체 혹은 여러 많은 부분에 걸쳐 auth에 따른 다른 작업을 위해, provider로 현 auth 상태를 관리.      
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

# FireBase     
간단한 서버+db생성 및 flutter에 연동.
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

- *request with token*    
authentication이 필요한 요청의 경우, fireBase는 url뒤에(.json뒤) '?auth=(String)'로 token을 명시하여 요청.      
(다른 http api의 경우, 요청의 header에 token을 명시하는 경우도있음.) 해당 token이 유효한 경우(authenticated)만, 요청에 맞는 처리.    

# FireBase Auth REST API      
FireBase에서 제공하는 authentication API. 이 서버로부터 내 서버에 대한 auth관리 가능.      
https://firebase.google.com/docs/reference/rest/auth#section-create-email-password 참조.    

- *API의 endPoint에 내 서버의 키를 명시.*    
내 api키: Firebase 프로젝트 -> 설정 -> 내 웹 API키     

- *post*
post메소드로 요청, email,password등으로 로 sing up/in 가능, sign up시 body에 email, password, returnSecureToken(bool) 명시.      
response에는 body에 idToken 등 포함.    

- *authentication error*     
authentication에 관한 error 발생시 (sign up시 이미 존재하는 email, sign in시 잘못된 비밀번호, 존재하지않는 email 등 )    
응답메시지의 statusCode가 400이 아니라, body에 'error'(key) 와 error에 대한 내용을 (value)로 응답.(statusCode는 200)    
즉, flutter의 http 패키지내의 post메소드로 요청을 보내더라도, post메소드에서 자동으로 error를 throw하는 statusCode가 400이상인 경우가 아니므로,
따로 response body의 'error'키 유무와, 해당 키의 value(Map형태)를 확인하여 error를 확인 및 handling해야함.(['error']가 null이면 정상적으로, sign in/up)    
'error'(key)에 대응되는 value는 Map(nested)형태로 이 Map내의 'message'키에 String으로 에러메시지 포함.       
에러메시지는 'EMAIL_EXISTS', 'INVALID_PASSWORD' 등 존재. firebase auth rest api문서 참조.     
일반적으로, authentication error를 따로 handling하기 위해, 해당 에러 확인 후, 에러 내의 message를 포함하는 custom Exception을 throw.   
요청을 호출한 외부 함수에서 해당 custom Exception을 catch하여 (on 키워드로) exception내용에 따라 적절한 처리. (error를 dialog로 표시 등.)    
