---
layout : single
title : "교육 에러 정리"
excerpt : "교육 중 겪었던 각종 에러 정리"
published : true
toc : true
toc_sticky : true

categories : 
    - Miraework

date : 2024-07-28
last modified : 2024-07-28
---
## 서론
내가 교육 받으며 겪었던 온갖 오류를 모아놨다. 대부분은 Flutter 관련이고 Spring Boot나 MySQL도 한두개 있다. 아마 프로젝트 중에도 겪게 될 오류들이라고 생각돼 기록한다.


## Flutter

### 파이썬 모듈 import 안 되는 경우 
에러 메시지 마지막 문장 : `line 3, in <module> from bs4 import BeautifulSoup ModuleNotFoundError: No module named 'bs4'`

`pip install beautifulsoup4`처럼 파이썬 모듈을 깔아준 뒤 코드에서 사용하려고 했을 때 나타난 오류. 설치환경과 컴파일 환경이 다르기 때문에 발생하는 오류다.

#### 해결 방법 
1. VSC 기준 `ctrl + shift + p`로 명령 커멘트를 열고
2. Python: Select Interpreter를 입력/선택
3. pip를 썼던 인터프리터와 같은 인터프리터 선택

<br>

### enable Developer Mode
![roqkfwk](https://github.com/user-attachments/assets/09a0fdc3-18ee-4118-b191-d1ef67ca72c5)  
개발자 모드를 키지 않아서 발생하는 오류다. 한 번만 키고 실행한 후 꺼도 작동하지만, 여기저기서 자주 막히니 계속 켜놓는 것도 고려할만 하다. 작업표시줄 검색에서 `개발자 기능 사용`을 검색한 후 켜주면 된다.
![mode](https://github.com/user-attachments/assets/4e4ebf23-0d6b-4176-9ef9-3f0cfac05cab)

<br>

### 환경이 변한 후 flutter라는 걸 인식하지 못할 때
![flutterpubget](https://github.com/user-attachments/assets/e95f428c-fecc-40b1-b683-cb8e427c6189)  
주로 깃허브에서 pull 했거나, 어디서 복붙한 경우 나타나는 일이다. flutter라는 걸 인식하지도 못한듯 여기저기서 빨간줄이 생기고, 해당 함수는 정의돼지 않았다거나 패키지를 찾을 수 없다고 말한다.  
dependencies를 다시 로드해주면 해결된다. 터미널에 `flutter pub get`이라고 치거나(안 된다면 프로젝트 루트로 올라가서), `pubspec.yaml`에 들어가서 아무거나 치고 지운 후 저장하면 다시 로드된다.

<br>

### 이름 변경 후 widget_test.dart에서 Target of URI doesn't exist
![test](https://github.com/user-attachments/assets/c6b85a6b-fef3-4812-8266-85ad3a872b64)  
프로젝트의 이름을 변경했을 경우 생기는 에러다. import하는 부분에서 Target of URI가 없다, 즉 유효하지 않은 링크라고 한다. 이는 `pubspec.yaml`에서 `name: ` 부분이 예전 이름으로 돼있기 때문이다. 이걸 현재 이름으로 바꿔주면 해결된다. `name: `은 해당 파일 최상단에 있다.

<br>

### null 관련
![null](https://github.com/user-attachments/assets/7c46b1e1-fb5f-4872-b2fc-cedf4e2b566b)  
Dart는 null과 관련된 걸 엄격하게 확인하기 때문에 null과 관련된 에러가 많이 뜬다. 사진과 같이 `snapshot.data.length`의 `length`에서 문제가 발견했다면, !나 ?를 대부분의 경우 더해서 해결할 수 있다.  
`snapshot.data!.length`로 바꾸거나, `snapshot.data.length?`로 바꾸거나.

<br>

### can't be assigned to the list type
![eorhkfgh](https://github.com/user-attachments/assets/1fc3c66c-01a5-44c1-a56e-45ccf61d58d6)  
리스트에 넣을 수 없는 걸 리스트에 넣어서 생기는 오류다. 빨간줄이 그어진 부분을 고치는 게 아니라 그 부분을 감싸는 대괄호 []를 삭제해주면 된다.

<br>

## Spring Boot

### Lombok을 인식하지 못하는 경우
1. `start.spring.io`의 spring intializr에서 `Dependencies`에서 lombok을 이미 추가하지 않았다면, 추가해준다.  
2. 해결되지 않은 경우 gradle sync를 한 번 해준다.
![gradlesync](https://github.com/user-attachments/assets/b5ccf5e3-b868-4d36-b37e-038acbae7a4a)  
인텔리제이 기준 우측 네이게이션 바에서 gradle 아이콘을 누르면 뜨는 탭에서 새로고침 버튼을 눌러주면 된다.
3. 아직도 안 된 경우 인텔리제이에 lombok을 설치하지 않아서 그런 걸수도 있다. `Settings > Plugins`로 들어가서 lombok을 설치해준다.

<br>

## MySQL

### 테이블 삭제가 잘 안 되는 경우
`edit> perferences` 로 들어가서 `safe updates` 끄기 
![delete](https://github.com/user-attachments/assets/1edd86ba-641b-44cf-9920-027a7826ee55)


이 외에 EER Diagram 만드는 법, 내가 쓴 SQL문 실행하는 법, 내가 만든 데이터베이스 접속하는 법 등이 있었다. 그러나 이건 내가 Oracle을 쓰다 MySQL을 처음 써봐서 생긴 문제기 때문에 제외한다. 