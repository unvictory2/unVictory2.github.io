---
layout : single
title : "미래일경험 2일차 - flutter(dart) 설치 및 자료형"
published : true

categories : 
    - 미래일경험
  
toc : true
toc_sticky : true

date : 2024-06-25
last modified : 2024-06-25
---

## 전체적인 순서
1. 앞으로 있을 순서에 대한 개요
2. Flutter 설치 과정
3. Dart의 자료형

## 앞으로 있을 순서에 대한 개요

- 7.24 ~ 8.21 프로젝트
- 8.19 ~ 8.20 제주도 워크숍, 참여 시 8.20 발표
- 8.20 수강신청
- 9.2 개강

### 수업 계획
- **1주차**: Dart
- **2주차**: Flutter
- **3주차**: 복잡적인 기술들 (백엔드 연결, java 등)
- 1주차 끝나고 노트북 대여 여부 결정

## Flutter 설치 과정

1. VSC에서 Flutter Extention을 추가 (Flutter를 깐 것은 아님, Extention 추가했을 뿐)
   - `Shift + Ctrl + P` > `flutter new`까지 치고 `flutter new projects` 엔터
   - Locate SDK, 설치 디렉토리 찾아서 넣어주기</br>
     ![image](https://github.com/unVictory2/unVictory2.github.io/assets/117062169/341f37c4-2424-4ed1-ba08-991a2a2cfc91)
   - 없다 하면 폴더 위치를 내부의 `bin`으로
   - 예시: `C:\Users\seung\Desktop\공부\flutter_windows_3.22.2-stable\flutter\bin`
  
1-1. 다른 방법: 안드로이드 스튜디오에서 플러그인 설치
   - Flutter 플러그인 설치
   - 환경변수 설정. `Path`에 Flutter 경로 추가
   - CMD에서 `flutter` 쳐서 설치 확인

2. 안드로이드 스튜디오에서
   - `Tools` > `SDK Manager` > `SDK Tools` > `Android SDK Command Line Tool` 다운로드 (파일 > 설정에서도 찾을 수 있다)
   - Android SDK Builds도 다운로드
     ![image](https://github.com/unVictory2/unVictory2.github.io/assets/117062169/bb4b78c8-a29f-43f3-bc61-066e503af724)
     ![image](https://github.com/unVictory2/unVictory2.github.io/assets/117062169/ddc46972-2c59-41d5-a978-b8e1e1dfe525)

3. VSC에서 로그 열고 Dart 제대로 뜨나 확인

4. CMD에서 Flutter 프로젝트 할 경로까지 가서 `dart create exam01`
   - PowerShell에선 안 될 수도 있다

5. 안 되면 조교님께 연락 후 교수님께 문의
   - 임시방편: 크롬에서 [dartpad.dev](https://dartpad.dev)로 임시로 사용

6. `flutter doctor`를 CMD에서 쳐서 설치 확인 가능

7. VSC에서 프로젝트 경로 설정

## Dart의 자료형

기본적인 데이터 타입:
```dart
int num;
String str;
double d;
bool ok;
이들은 모두 변수가 아니라 객체다.

