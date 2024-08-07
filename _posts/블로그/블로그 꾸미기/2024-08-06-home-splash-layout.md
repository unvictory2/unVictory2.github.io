---
layout : single
title : "전체적인 레이아웃"
excerpt : "홈 화면에 splash 형식으로 이미지 넣기, Recent Posts, Sidebar, toc 너비"
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
개별 페이지 레이아웃에서도 width가 100%로 돼있는 걸 바꿀 수 있긴 하다. 위의 내용은 그 100%가 몇 픽셀인지 설정하는 거다.
### 내용 설정




## 사이드바
![sidebar](https://github.com/user-attachments/assets/1487c527-4c2d-466c-a537-1d68fb372722)

기본 형식에 카테고리만 추가된 사이드바다. 카테고리 추가는 [식빵맘님 블로그 글](https://ansohxxn.github.io/blog/category/)을 참고했다. 사이드바를 bio라고 부르기도 하고, toc를 right_sidebar로 부르기도 하다보니 좀 헷갈린다.  

### 너비 변경
`_sass\minimal-mistakes\_sidebar.scss`에서 `@include breakpoint($large) {` 혹은 ` @include breakpoint($x-large) {`의 width를 바꿔주면 된다. 화면별로 너비가 다르기 때문에 저렇게 나뉘어져 있다.

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

