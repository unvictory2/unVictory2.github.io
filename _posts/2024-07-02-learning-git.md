---
layout : single
title : "깃 배우기"
published : true

categories : 
    - git

date : 2024-07-02
last modified : 2024-07-02
---
"모두의 깃&깃허브"라는 책으로 학습.

## 깃 사용 방법 종류
+ 소스트리
+ IDE
+ 명령창

이중 소스트리는 학습했고, IDE는 책에서 다루지 않는다. IDE는 소스트리만 배운 지금 써봤는데 쓰는데 큰 문제는 없는듯하다.  
명령창 command 부분을 학습하는 김에 여기에 정리도 해야겠다. 소스트리 부분도 복습겸 정리할 건지는 추후 결정.

git init : 로컬 저장소 만들기
원하는 경로에서(폴더 안까지 들어가서) git bash로 git init. 즉 폴더 자체는 이미 존재해야 함.

git status : 현재 디렉터리 상태 알려줌. 브런치는 어딘지, 커밋한 게 있는지, 변경 사항이 있는지 등.

git add : 스테이지에 올리기. git add a.txt

git add . : 현재 작업 디렉터리에 있는 모든 변경 사항을 한 번에 스테이지로 추가.

git commit : git commit -m "커밋 메시지" OR git commit --message "커밋 메시지".

git commit -am "커밋 메시지" : add와 commit 동시에

git log : 저장소의 커밋 목록 출력
