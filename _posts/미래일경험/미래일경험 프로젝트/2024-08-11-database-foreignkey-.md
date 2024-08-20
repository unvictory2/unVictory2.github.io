---
layout : single
title : "[미래일경험 프로젝트] 데이터베이스 테이블 생성 순서 문제"
excerpt : "유저 테이블과 그룹 테이블이 일정 테이블을 외례키로 참조하고 있어서 먼저 생성될 수가 없다."
published : true

categories : 
    - Miraework Project

toc : true
toc_sticky: true
date : 2024-08-11
last modified : 2024-08-11
---

## 문제점

![KakaoTalk_20240811_170151030](https://github.com/user-attachments/assets/17978171-c648-42c5-9535-120c4623f84f)

이게 기존의 데이터베이스 구조다. 보면 개인 캘린더와 그룹 캘린더에서 일정 테이블을 외례키로 참조하고 있다. 이러면 일정이 먼저 생성돼야 개인/그룹 캘린더가 생성될 수 있는데 이는 말이 안 된다.

<br>

## 수정 과정

<img src="https://github.com/user-attachments/assets/2ba9c756-d811-438b-9c04-2671e4372806">

수정된 데이터베이스 구조다. 캘린더는 프론트에서 구현하고, 일정 데이터를 서버에 요청하는 방식으로 바꾼다. 

![fix1](https://github.com/user-attachments/assets/e63624bd-0406-43e4-b3c8-1e5a47c975f4)

이렇게 추가로 수정해봤다. 지금 상황과 무관한 테이블이나 속성은 생략했다. 달력은 front(client)에서 구현된다. 데이터베이스에는 사용자, 그룹, 일정, UserGroup 4개의 테이블이 있다.
 - 사용자 테이블에는 `사용자 ID`와 정보가 있다.
 - 그룹 테이블엔 `그룹 ID`와 정보가 있다.
 - UserGroup 테이블은 사용자와 그룹의 ID를 가져와서 둘을 연결시켜준다.
 - 일정 테이블엔 `userID`, `groupID` 2개의 외례키가 있다. 그룹 일정인지 개인 일정인지를 `isGroupEvent`로 구분한다. 
   + 일정 추가시 개인 일정일 경우 `userID` 값을 참조해오고, `groupID` 값은 null로 바꾼다. 그룹 일정일 경우에는 반대. 
   + 마지막엔 무결성 체크를 위해 `isGroupEvent = 1`인데 `groupID`가 `null`이거나, 그 반대의 경우가 발생했는지 확인한다. 발생했다면 오류다.
   + 일정정보가 외례키로 `userID` `groupID` 둘 다 가지고 있는 형태로 가면 둘 다 nullable로 만들어야 되기 때문에 무결성 체크를 위해 `isGroupEvent` 속성이 추가됐다.

<br>

## Client - Server - DB 흐름 시뮬레이션
위 그림을 바탕으로 달력에서 데이터를 요청하고 가져오는 과정을 생각해봤다.  

![step1](https://github.com/user-attachments/assets/0a0f6505-0a95-4811-ac50-8b2324bdf655) 

사용자가 클라이언트(프론트와 혼용해서 사용)에서 일정 화면을 켰다. 클라이언트는 서버에게 `userID`를 전달하고 해당 유저의 개인/그룹 일정을 요구한다.

![step2](https://github.com/user-attachments/assets/21c84c0a-cd9e-40ab-873b-fd9c74fc5f26)  

서버는 데이터베이스에 접근해서 개인 일정을 찾는다. 전달받은 `userID`를 가지고 일정 테이블에서 `isGroupEvent = false`고  `userID = :userID`인 항목의 일정상세내용을 가져간다. 

![step3](https://github.com/user-attachments/assets/da1a0d63-e6a3-49c8-be29-549aecd38187)

이후 서버는 그룹 일정 내용을 찾는다. 일단 `UserGroup` 테이블에서 `userID = :userID`인 항목을 찾아 유저가 속한 모든 그룹을 알아낸다. 그 그룹 아이디를 가지고 일정에서 일정 정보와 `groupID`를 추출한다. 

![morestepinbtwn](https://github.com/user-attachments/assets/1f7d5ee2-a514-4036-8a8b-d8963fa0d0ec)

이제 서버에 필요한 데이터가 전부 모였다. DB에는 객체나 배열로 데이터를 저장할 수 없는데, API에서는 주로 객체나 배열 형태로 데이터를 전달한다. 그렇기에 서버에서 데이터를 받기 쉽게 가공해준다. 

![step4](https://github.com/user-attachments/assets/248cfd85-5f78-4d5a-8835-612cf48bdf83)

서버가 가공한 데이터를 클라이언트에 전달하고, 클라이언트는 그 데이터를 사용자에게 보여준다.

## 첨언
이 데이터베이스 형태는 최종이 아니다. 백쪽 분들이 개인도 일종의 그룹으로 판정하는 방법을 고려중이라 하시니 그쪽 결과를 기다리면 될듯. 우리 프론트 쪽은 위젯쪽 구현을 우선 해보기로 했다.