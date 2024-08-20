---
layout : single
title : "[미래일경험 프로젝트] 패키지 직접 수정하기"
excerpt : "프로젝트 폴더 내부에 넣는 방법"
published : true

categories : 
    - Miraework Project

date : 2024-08-20
last modified : 2024-08-20
---
![question](https://github.com/user-attachments/assets/109e680f-6cb7-4bc9-85db-60faed944403)  

이 방법은 흔치 않은 방법이라고 하셔서 PR이나 Issues를 사용한 방법을 사용하려 했는데 전혀 받아들여질 기미가 보이지 않았다. 다른 패키지를 찾기에는 이미 시간도 에너지도 부족했고...  
어쨌든 수정 후 적용하는 방식은 
 - 패키지 fork > 수정 > 해당 리포지토리 주소로 import
 - 소스코드 다운 > 수정 > 프로젝트 디렉토리 내부에 넣어서 그 주소를 import  

이렇게 2가지로 보이는데 나는 후자로 선택했다. 직접 내 프로젝트에 포함시키는 방법인만큼 큰 패키지라면 하기 부담스러운 방법일듯 하다. 진행 방법은 생각보다 굉장히 간단하다.

1. 원하는 패키지의 깃허브로 간다. 주로 소개글이 있는 곳에 깃허브 주소도 같이 있다.
2. 초록색 Code 버튼을 누르고 Download Zip으로 받아서, 압축을 푼다.
3. 원하는 수정을 한다.
4. 패키지를 내 프로젝트 디렉토리 안 아무데나 넣는다. 나는 루트 디렉토리에 `local_packages`라는 폴더를 만들고 그 안에다 넣었다. 구조는 다음과 같다.

```
📦desktop_widget
 ┣ 📂📜내 프로젝트의 파일들
 ┣ 📜pubspec.yaml
 ┗ 📂local_packages
   ┗ 📂flutter_calendar_view-master
     ┣ 📂📜해당 패키지 파일들
     ┗ 📜pubspec.yaml
 ```
5. 루트의 `pubspec.yaml`로 가서 다음과 같이 calendar_view의 경로를 수정한다. 이 파일은 인덴트에 민감하니 잘 맞춰준다. 이 코드로 인해 해당 경로에서 `name: calendar_view`라는 내용을 가지고 있는 `pubspect.yaml` 파일을 찾게 된다. 이게 끝이다!
```yaml
dependencies:
  flutter:
    sdk: flutter
  calendar_view:
    path: ./local_packages/flutter_calendar_view-master/
```
