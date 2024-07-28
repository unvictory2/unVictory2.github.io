---
layout : single
title : "Masthead, Favicon 꾸미기"
excerpt : "Favicon, 제목, 부제목, About, Categories, 검색"
published: true

categories : 
    - Blog Decoration

date : 2024-07-18
last modified : 2024-07-27
---

## 구조
내 블로그의 Masthead는 꽤 많은 블로그에서 사용하는 형식을 그대로 쓰고 있다. 
![d](https://github.com/user-attachments/assets/629cb6c7-5a7c-47ca-82ff-2e3a3c9c2af7)
이런 식으로 왼쪽엔 아이콘과 블로그명/부제목, 오른쪽엔 메뉴를 보여준다. 이 구조는 Minimal Mistakes에서 기본으로 제공해주는 구조 그대로다. 각 버튼에 해당하는 메뉴는 어떻게 만들었는지 알아보자.

#### Favicon
Masthead 좌상단과 브라우저의 탭에 띄울 이미지를 직접 정할 수 있는데, 이를 Favicon이라고 부른다. 설정 방법은 [식빵맘님 블로그](https://ansohxxn.github.io/blog/favicon/)를 그대로 했기 때문에 내가 알아낸 척 똑같은 걸 또 쓰진 않겠다.

#### 블로그명, 부제목
블로그 디렉터리 루트의 `_config.yml`의 `title`과 `subtitle`을 바꿔주면 된다.
![e](https://github.com/user-attachments/assets/0b42cd57-0ec7-4df7-b89f-a042bedbefc4)

#### 우측 메뉴
우측 메뉴는 자유롭게 설정할 수 있다. 검색 기능은 별개라 밑에 설명하겠다. 나는 평범하게 About과 Categories만 만들었다. `_data\navigation.yml`에서 원하는 제목(About)과 누르면 연결할 링크(/about/)를 주는 식으로 간단히 설정할 수 있다. 연결될 페이지는 `pages\about.md`로 구현하면 된다. YAML Front Matter에 `permalink: /about/` 속성을 주는 걸 빼면 평소와 같게 글을 쓴다.   
우리가 그냥 링크와 이름만 치면 그걸 보여주는 동작은 `_includes\masthead.html`에서 한다. 뭔가 바꾸고 싶다면 살펴보자.

#### 검색
`_config.yml`에서 `search: true`로 바꿔준다.
![search](https://github.com/user-attachments/assets/0525efa5-66f0-4ae0-a755-aabcb398736b)  
이렇게만 해도 검색 버튼이 뜨고 누르면 검색 화면으로 이어진다. 어딘가에 `search`가 true면 Minimal Mistakes가 미리 만들어준 검색 버튼을 보여주고, 버튼을 누르면 마찬가지로 미리 만들어진 검색 페이지를 보여주라고 써있기 때문이다.  
밑의 `search_full_content`는 글의 내용까지 검색할지 여부이다. 바로 밑에 `search_provider`라는 항목도 있는데 설정 안 해줘도 작동한다. 난 설정 안 했다. 

