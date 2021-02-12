# working with web server to connect database    
직접 db와 연결하는것보다(코드 노출 가능), web server를 통해 db와 interact하는게 더 안전.    
device에 제한되지않는 data를 저장해야할때. internet필요.     
일반적으로, REST API(endPoint(URL)+verb(method))를 사용하는 server에 HTTP request를 보내서 server(+dataBase)와 interact.


# FireBase     
간단한 서버+db생성 및 flutter에 연동.
Firebase -> 프로젝트 생성 -> realtime DB생성

- *Post*      
flutter에서 (http.)post(url, body:...,); 시, url에 firebase의 realtime DB(server+DB) 주소+ .json 입력.     
ex) const url = 'https://flutterudate-default-rtdb.firebaseio.com/products.json'; 와 같이, ( main주소 및, /products.json )      
endPoint(서버주소)뿐만 아니라, /로 접근or새로 생성할 db폴더 forwarding하고 .json으로 encoding 명시.(?)      
