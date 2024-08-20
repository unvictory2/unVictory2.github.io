---
layout : single
title : "[미래일경험 프로젝트] 위젯 만들기 과정 문제"
excerpt : "피그마에서 플러터 코드 생성이 안 됨, 플러터로 위젯 개발이 불가함"
published : true

categories : 
    - Miraework Project

toc : true
toc_sticky: true
date : 2024-08-12
last modified : 2024-08-12
---
## 플러터로 위젯 개발
플러터로 모바일 위젯을 개발할 수 없다는 걸 알게 됐다. 이제서야 알게 됐다는 게 꽤 끔찍하다. 아, 위젯은 플러터에서 이미 사용하는 개념이기 때문에 구분을 위해 홈스크린 위젯이라고 부르겠다. 어쨌든 홈스크린 위젯을 만들고 싶다면 안드로이드와 iOS쪽 네이티브 언어로 개발해줘야 한다. 그걸 하지 않기 위해 플러터를 사용하는 거 아닌가 싶지만 안 된다니 어쩔 수 없다.

<br>

## 피그마에서 플러터 코드 생성하기
우리는 피그마 UI가 있기 때문에 이걸 플러터로 옮겨서 사용해야 한다. 하지만 `Figma to Code`, `DhiWise`, `Figma to Flutter` 같은 플러그인/사이트들을 사용해봤는데 결과물이 제대로 나오질 않았다. 결과물은 대부분의 경우 아예 아무것도 안 떴고, 요소들이 여기저기 흩뿌려진채 가로세로가 바뀌어져 있는 경우도 있었다. 플러터도 처음인데 피그마도 처음이니까 갈피도 못 잡겠고 참 어렵다. 아예 시작을 못 하고 있기 때문에 멘토님께 질문할 예정이다. 질문할 때 참고자료로 쓰려고 온갖 실패 사례들을 ppt로 제작했는데 첨부한다. 데스크톱 위젯을 만드는 상황이다.    

### 메모 부분 선택

![슬라이드1](https://github.com/user-attachments/assets/4ca6f0b4-d09e-41a5-a5cc-8de39c16ade1)  

![슬라이드2](https://github.com/user-attachments/assets/472dc549-91d3-475d-a4b2-e432acef5c9d)  

초반엔 개별 모듈을 어떻게 클릭하는지 몰라서 어딜 클릭했는지도 써놨다. 모듈이 선택되지 않고 세부 디자인이 선택되는 경우가 많았기 때문이다. 나중에 알게 된 거지만 왼쪽의 메뉴에서 모듈을 선택할 수 있다.  

### 전체 부분 선택

![슬라이드3](https://github.com/user-attachments/assets/2be4e08a-bda0-4d5a-8a9b-1f4341b5cae5)

![슬라이드4](https://github.com/user-attachments/assets/01fec82e-3d78-4b23-bc22-78de8194004f)

### 캘린더와 메모 선택
![슬라이드5](https://github.com/user-attachments/assets/76f7799b-d32b-4322-af62-77fd0f94e41a)

![슬라이드6](https://github.com/user-attachments/assets/12428985-8c19-4263-9009-f0af56fcbaab)

### 달력의 일부 선택
![슬라이드7](https://github.com/user-attachments/assets/38e451f2-0a7e-4a92-9e8d-fc15aebe4fe3)

![슬라이드8](https://github.com/user-attachments/assets/3ecd058f-e2a1-4110-90e4-53b8a55828a6)