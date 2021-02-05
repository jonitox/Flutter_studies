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


------------------------
## extra packages   
- *intl*    
help internationalization and localization facilities, including message translation, plurals and genders, date/number formatting and parsing, and bidirectional text.    
DateFormat.yMMd().format(dateTime) / DateTime객체를 pattern에 맞게 formatting하여 String으로 반환.    
DateFormat은 class로서 생성자 인자로 패턴(ex)yyyy-MM-dd)을 받거나 여러 pre-configured pattern을 named생성자(ex).yMMD())로 지정.   
DateFormat.E().format(dateTime) / DateTime을 요일만 표시(Mon,Tue,...).      
format은 class내부 메소드. date를 해당 패턴을 가진 String으로 반환.    
