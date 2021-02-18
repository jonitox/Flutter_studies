# working with web server+database    
직접 db와 연결하는것보다(코드 노출 가능), web server를 통해 db와 interact하는게 더 안전.    
device에 제한되지않는 data를 저장해야할때. internet필요.     
일반적으로, REST API(endPoint(URL)+verb(method))를 사용하는 server에 HTTP request를 보내서 server(+dataBase)와 interact.    

# optimistic update.   
delete, update 등 현 memory상의 처리와 서버 상의 처리 진행. 이때, 서버상의 처리의 오류 발생시(device 인터넷 끊김, 서버 오류 등.)    
memory상의 처리를 복구하는 작업을 optimisitic update라고함.   
예를 들어, 상품 삭제시, 앱 메모리 상의 상품 삭제 후, 서버 상의 상품 삭제. 이때 서버에 대한 요청 실패시, 메모리 상에 다시 상품을 로드하는 것.   
(물론, 서버 요청 처리 실패에 대한 error handling도(앱 내에 실패 메시지를 표시하는 등.) 같이 구현 가능.)    

# FireBase     
간단한 서버+db생성 및 flutter에 연동.
Firebase -> 프로젝트 생성 -> realtime DB생성

- *Post*      
flutter에서 (http.)post(url, body:...,); 시, url에 firebase의 realtime DB(server+DB) 주소+ .json 입력.     
ex) const url = 'https://flutterudate-default-rtdb.firebaseio.com/products.json'; 와 같이, ( main주소 및, /products.json )      
endPoint(서버주소)뿐만 아니라, /로 접근or새로 생성할 db폴더 forwarding하고 .json으로 encoding 명시.(?)      


- *get*      
flutter의 http.get(url)으로 요청, response로 data 전달. response의 body에 해당 url이 나타내는 db내 key(ID)-value(data)를 Map형태의 jsonv포맷으로 전달.   
body로 받은 데이터를, decode하여 Map<String, dynamic>으로 저장가능.     
ex) final extractedData = json.decode(response.body) as Map<String, dynamic>; (Id가 나타내는 data가 Map인 경우(nestedMap)에, dynamic으로 받아야 error가 나지않음.)         

- *patch*       
flutter에서 http.patch(url, body:...)으로 요청. 해당 내용으로, url의 db를 update. 예를들어, products의 특정 한 item만 update시, (item의 정보가 Map<String, obejct>인 경우)     
url = 'https://.../products/$id.json'로 요청.(products폴더 내의 id에 접근(해당 item))       
이때, 메시지의 body로 새로운 title, description등을 입력하여(json(Map)) update요청. 입력된 정보로서,        
해당 item의 기존에 없던 key의 정보가 들어오면, 새롭게 추가, 기존의 key에 대한 값이 들어오면 해당 값으로 value update. 명시되있지 않은 기존에 있던 나머지 key에 대한 데이터는 유지.      

- *delete*    
flutter에서 http.delete(url)로 요청. 해당 url이 지칭하는 db 컨테이너를 삭제.     
