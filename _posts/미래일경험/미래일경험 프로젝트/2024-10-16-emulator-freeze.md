---
layout : single
title : " 문제 해결 - 에뮬레이터 종료시 컴퓨터 먹통"
excerpt : "close 버튼 누르지 말아라"
published : true

categories : 
    - Miraework Project

toc : true
toc_sticky: true

date : 2024-10-16
last modified : 2024-10-16
---
![화면 캡처 2024-10-16 021754](https://github.com/user-attachments/assets/2be6b033-0901-4079-b681-c549bfbce14a)  

안드로이드 에뮬레이터가 정신이 나가버렸다. 화면 끄기나 종료를 누르면 컴퓨터가 멈춘다. 마우스/키보드 입력이 안 되고 화면은 나오는 상태가 지속된다. 헤드폰에서 음악은 계속 나오다가 한참 기다리면 끊긴다. 결국 전원 버튼 눌러서 다시시작 해야된다. 오늘만 7번째인거 같다. 다시 시작한 이후에 해당 에뮬레이터는 먹통이 돼서 지웠다가 다시 만들어야 한다.  

해결 방법은 
- `flutter doctor` 해보기
- 에뮬레이터 종료를 작업관리자나 `adb emu kill` 커멘드로 하기
인 거 같다. 첫 번째 방법은 했더니 약관 동의 안 한 게 있다고 해서 그거만 했다. 

오 첫 번째 방법만 하고 지금 끄기 버튼으로 껐는데 컴퓨터가 멀쩡하다. 드디어 멀쩡해진 건가? 다음엔 안 되면 두 번째 방법으로 하는 게 좋겠다. 저건 매번 해야되는 거라 훨씬 귀찮을듯...

[참고링크1](https://www.reddit.com/r/androiddev/comments/8vy1vz/anyone_have_problems_with_android_studio_freezing/)  

[참고링크2](https://livelikesloth.tistory.com/entry/%EC%95%88%EB%93%9C%EB%A1%9C%EC%9D%B4%EB%93%9C-%EC%8A%A4%ED%8A%9C%EB%94%94%EC%98%A4-%EC%97%90%EB%AE%AC%EB%A0%88%EC%9D%B4%ED%84%B0-%EB%81%84%EB%A9%B4-%EC%BB%B4%ED%93%A8%ED%84%B0-%EB%A9%88%EC%B6%A4-%ED%98%84%EC%83%81)