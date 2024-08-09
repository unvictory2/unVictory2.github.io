---
layout : single
title : "전체적인 레이아웃"
excerpt : "Splash 이미지 대문, Recent Posts, Sidebar, toc, 좌우 마진"
published: true

categories : 
    - Blog Decoration

toc : true
toc_sticky : true

date : 2024-08-06
last modified : 2024-08-06
---

![blog](https://github.com/user-attachments/assets/7c4199e4-5c68-432e-aa9e-c80bc3a33152)

블로그 화면에서 masthead를 제외한 나머지 부분을 꾸미는 방법을 알아보자. 꾸미기가 끝난지 시간이 꽤 흘러 커밋 기록을 보면서 글을 쓴다. 나말고 누가 보게될지 모르겠지만 뭔가 틀린 점이 있어도 양해 바란다.

<br>

## 좌우 여백
사이드바보다도 바깥쪽 여백을 바꾸려면 `_sass\minimal-mistakes\_page.scss`에서 바꿀 수 있다. 가장 위의 `#main`을 수정하면 된다.
```scss
#main {
  @include clearfix;
  margin-left: 12%; // 좌우 여백(사이드바와 본문 사이가 아니라, 사이드바보다도 왼쪽)
  margin-right: 12%;
```

<br>

## Splash 형식 이미지
이 부분은 중간에 시행착오가 꽤 있었기 때문에 사이드바와는 다르게 커밋 목록을 보고 작업 흐름을 쉽게 떠올리기가 어렵다. 
일단 `index.html`이라는 파일을 루트에 만들고,  
```
---
title : 
excerpt: unvictory2의 블로그
layout: home
permalink:
author_profile : true
sidebar_main: true
hidden: true
header:
  overlay_image: /assets/images/title_image.jpg
---
```
이런 식으로 작성해준다. 대문에 띄울 정보다.  

이제 `_includes\page_hero.html`로 간다.  
`<div class="page__hero">`의 스타일에 중앙 정렬을 넣어준다. 이걸 넣어야 제목이 중앙정렬된다.  
```html
<div class="page__hero{% if page.header.overlay_color or page.header.overlay_image %}--overlay{% endif %}"
  style= 
    " text-align: center; 
```
코드 블록으로 가져오면 오류가 나서 해당 태그 캡쳐다.  

![화면 캡처 2024-08-08 024543](https://github.com/user-attachments/assets/1b475bf8-808e-4ab0-8b73-580378e1e85a)  

그 밑의 코드들도 변경점이 조금 있다.  

![more](https://github.com/user-attachments/assets/f9d4b30e-1580-4b66-a94e-8357d1272421)  

주석으로 이미 설명을 다 해놨다.  
이미지에 뜰 제목은 원래 index.html의 제목이었는데, "밤새워 친 고스톱"으로 하드코딩했다. 왜냐면 브라우저 탭과 작업 표시줄에 index.html의 제목이 뜨기 때문이다. 이게 "밤새워 친 고스톱"이라고 뜨면 사람들이 이걸 블로그 제목으로 헷갈릴 수 있다. (블로그 제목은 "제발 돼라"다) 그래서 index.html의 제목을 비워서 탭과 작업 표시줄에 "제발 돼라"를 뜨게 하고, 이 부분은 하드코딩 해준 거다.
excerpt(unvictory2의 블로그)를 중앙정렬 하기가 매우 어려웠다. 어디다가 중앙정렬 설정을 해줘도 안 됐다. 결론은 마진이 있어서 중앙정렬된 상태여도 가운데에 떠있지 않는 거였다.

써놓고 나니 '진짜 이게 다인가?' 싶다. 커밋 목록을 최대한 뒤졌지만 뭔가 빼먹은 걸지도 모른다. 상단에 이미지와 글자가 가로로 길게 나오고, 그 밑에 최근 포스트와 bio가 뜨는 이 형식을 정말 하고 싶었다. 정말 정말 어려웠고 며칠에 거쳐서 했다. 하는 과정에서 블로그 구조를 이해해야 했고 생소한 코드들도 많이 읽었다. 블로그 이해도가 가장 많이 늘었던 작업이다. 그런데 이렇게 써놓으니 정말 간단해서 이상하다... 이 짧은 과정을 며칠에 거쳐서 한 걸까... 진짜 뭔가 빼먹은 걸지도. 

<br/>

## 최근 포스트(본문 영역)
### 너비 변경
이 설정은 대문에서 본문의 영역 너비뿐만 아니라 글 페이지에서의 너비도 바꾼다. `_sass\minimal-mistakes\_variables.scss`에서 바꾸면 된다. 
```scss
$small: 600px !default;
$medium: 768px !default;
$medium-wide: 900px !default;
$large: 1024px !default;
$x-large: 1450px !default;
```
사실 이건 화면 크기를 구분하는 기준이지만, `_page.scss`에서 이 값을 기준으로 포스트 영역을 지정하기 때문에 관련이 있다. 잘 모를 때 바꿔서 냅두게 됐다. 그러나 이 값을 직접 바꾸는 건 안 좋은 생각이다. 만약 지금 바꿔야 된다면 `_page.scss`에 가서 수정할듯 하다.   
개별 페이지나 레이아웃에서도 width가 100%로 돼있는 걸 바꿀 수 있긴 하다.
### 내용 설정
최근 포스트 내용을 만들어주는 코드는 전혀 바꾸지 않았다. `home.html`의 `{content}` 밑에 넣어줬을 뿐이다. 왜냐면 난 기존의 대문에 가로로 긴 이미지를 추가하고 싶었을 뿐이기 때문이다.  

![fdgfsd](https://github.com/user-attachments/assets/38a611ad-9b99-4394-92c8-fcdd13223ddd)

`home.html`의 코드다. 코드 블록으로 가져오면 이상해지길래 스샷으로 찍었다.

<br/>

## 사이드바
![sidebar](https://github.com/user-attachments/assets/1487c527-4c2d-466c-a537-1d68fb372722)

기본 형식에 카테고리만 추가된 사이드바다. 카테고리 추가는 [식빵맘님 블로그 글](https://ansohxxn.github.io/blog/category/)을 참고했다. 사이드바를 bio라고 부르기도 하고, toc를 right_sidebar로 부르기도 하다보니 좀 헷갈린다.  

### 너비 변경
`_sass\minimal-mistakes\_sidebar.scss`에서 `@include breakpoint($large) {` 혹은 `@include breakpoint($x-large) {`의 width를 바꿔주면 된다. 화면별로 너비가 다르기 때문에 저렇게 나뉘어져 있다.

### 내용 변경
사이드바의 내용은 루트의 `_config.yml`에서 변경할 수 있다. 
```yml
# Site Author
author:
  name             : "unVictory2"
  avatar           : "/assets/images/profile_image.jpg" # path of avatar image, e.g. "/assets/images/bio-photo.jpg"
  bio              : "집에 가고 싶다"
  location         : "Somewhere"
  email            : "seungeon7878@gmail.com"
```
이 부분을 수정하고, 추가로 링크를 넣고 싶으면 그 아래 부분까지 변경하면 된다.



### scss 변경
`_sass\minimal-mistakes\_sidebar.scss`로 가면 사이드바와 관련된 scss를 변경할 수 있다. 나의 경우에는 사이드바와 본문 사이의 거리, 사이드바의 투명도, 프로필 사진의 모양 이렇게 3가지를 바꿨다.

<br>

## toc

### 너비 변경
toc의 너비는 `_sass\minimal-mistakes\_variables.scss`에서, 
```scss
/*
   Grid
   ========================================================================== */
// toc 너비
$right-sidebar-width-narrow: 100px !default; // 200
$right-sidebar-width: 220px !default; // 300
$right-sidebar-width-wide: 450px !default; // 400
```
이 부분을 수정하면 된다. 

### 기타 설정
난 다른 부분은 건드리지 않았다. 하지만 scss를 바꾸고 싶다면 `_sass\minimal-mistakes\_navigation.scss`에 가서 `.toc` 관련 항목을 바꾸면 된다. 아니면 동일 디렉터리의 `_sidebar.scss`에서 `.sidebar_right`에 대한 설정을 바꿔야 할 수도 있다. 전자에는 스타일에 관한 설정이, 후자에는 패딩이나 마진에 대한 설정이 많은 느낌이다. 

