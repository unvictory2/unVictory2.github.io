---
layout : single
title : "유니티 코드가 VS에서 하이라이팅 안 됨"
excerpt : "유니티 관련 단어 한정 하이라이팅, intelli sense도 안 된다"
published: true

categories : 
    - Capstone

toc : true
toc_sticky : true

date : 2025-03-27
last modified : 2025-03-27
---

## 개요 
일단 오늘 이미지 업로드 상태가 이상하다. 되는 것도 있고 안 되는 것도 있고... 켜둔 게 많아서 로딩이 오래 걸리는 거 같긴 하다.   

유니티 코드가 VS에서 하이라이팅 안 되고 intelli sense도 안 됐다. (마우스 올려두면 메서드 설명 보여주는 거, 자동완성 등 전부)

![Image](https://github.com/user-attachments/assets/e2b47f7c-1029-4d1a-b53d-0d5176f94802)  

이런 식으로 `public, class`같은 건 하이라이팅 되는데 `InvokeRepeating, Instantiate`같은 애들은 안 되고 마우스를 올려놔도 설명도 안 보여준다. 자동완성도... 작년부터 내내 이랬는데 아예 안 되는 게 아니라 그냥 냅뒀다가 이제서야 고친다. 사실 문제가 없는 건줄 알았음...  

내가 시도한 방법들은  
1. 유니티 설정의 External Tools에서 Visual Studio가 연결돼있는 거 확인  
2. 동일한 창에서 Regenerate Project Files 클릭  
3. Visual Studio Installer를 열고 Game Development with Unity 설치  
4. 유니티의 패키지 매니저에서 Visual Studio를 업데이트 해야 하는지 확인  
5. VS의 솔루션 탐색기에서 Assembly-CSharp 파일이 제대로 로드되었는지 확인  

이었고, 마지막 방법으로 해결됐다! 근데 마지막 방법으로 된 거라 1~5를 다 해서 해결된 건지, 아니면 5번 하나만 하면 됐던 건지 모르겠다. 그래서 전부 다 쓰는 거기도 하고.  

## 해결
### External Tools 관련 (1,2번)
1. 유니티에서 Edit > Preferences > External Tools로 이동.
   ![Image](https://github.com/user-attachments/assets/6d799d72-5726-4592-91b5-59ad040851ad)  
2. External Script Editor가 VS로 잘 돼있는지 확인
3. `Regenerate proejct files` 눌러보기 (눌러도 별 일이 일어나진 않았다)  
   
### Visual Studio Installer (3번)
1. 윈도우 검색 메뉴에서 `Visual Studio Installer`를 치면 놀랍게도 이미 깔려있다. 아마 VS 깔때 알아서 깔린듯?
2. 열어보면 내 VS 버전이 뜬다. `수정` 버튼을 누르고 `유니티를 이용한 게임 개발`이 깔려있지 않다면 깔아준다. 사실 이게 안 깔려 있고 설치 목록 중 Intellisense가 있길래 이거 하고 해결될줄...  
![Image](https://github.com/user-attachments/assets/c7aab6ca-3a38-4772-90ed-452c7c0a22df)  

### Package Manager (4번)
1.  Window > Package Manager로 이동하여 Visual Studio Editor 패키지가 최신 버전인지 확인하고 필요시 업데이트한다. 난 다른 업데이트 가능한 패키지들의 업데이트 버튼이 있는 위치에 `Unlock`이라는 버튼만 있길래 눌러봤는데 별 일은 안 일어났다. 다시 잠그고 싶었지만 어떻게 하는지 몰라서 실패.  
![Image](https://github.com/user-attachments/assets/f3aff7cc-50b4-4be9-b60f-868ec2ad9be7)  
   
### 솔루션 탐색기 (5번)
1. VS로 가서 우측의 솔루션 탐색기를 본다. 없으면 `Ctrl + Alt + L`로 연다.  
![Image](https://github.com/user-attachments/assets/e8d8d302-603c-41ae-92c9-7dc0b3972d4a)  
2. 위쪽에 "프로젝트 세팅에 따라 추가 설치가 필요할 수 있습니다" 같은 말이 있으면 그걸 하면 된다. 오른쪽 아래였나 위에 작은 글씨로 설치 혹은 그 비슷한 뜻의 버튼이 있었다.  
3. 안 뜬다면, `Assembly-CSharp`이 제대로 로드되었는지 확인한다. 옆에 (언로드됨)이라고 써있거나 하면 안 된 거다. 우클릭 해보면 뭘 설치하는 버튼이 있고 누르면 뭘 설치하라고 할텐데, 설치하면 해결된다!  
![Image](https://github.com/user-attachments/assets/fb4e2d8b-4fc9-48f4-98e5-e0d30016b7d6)  

## 참고
해결되긴 했는데, 유니티에서 연 C# 코드에만 적용된다! 교수님이 프로젝트 말고 그냥 스크립트만 올려주신 단독 `.cs` 파일을 열어보니 처음이랑 똑같은 상태다.  
단독으로 열었을 때 :   
![Image](https://github.com/user-attachments/assets/4ee94643-9b76-4435-b825-68c773c14815)


다른 유니티 프로젝트에 이 스크립트 넣고 유니티에서 열었을 때 :   
![Image](https://github.com/user-attachments/assets/b9e5499b-0bdd-4084-bd06-acd8d2d5b7b7)  