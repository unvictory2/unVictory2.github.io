---
layout : single
title : "[문제 해결] 화면 크기 미고려 문제"
excerpt : "블로그가 모든 화면에서 보기 적합하지 않다"
published: true

categories : 
    - Blog Decoration

toc : true
toc_sticky : true

date : 2024-07-30
last modified : 2024-08-08
---

## 문제점
언제나 집에 있는 큰 모니터로 작업하다보니 블로그 내부 기준으로 `x-large`인 경우만 고려하여 프로그래밍하고 말았다. 그 결과 모바일에서는 운좋게도 괜찮게 보이나, 노트북 화면에선 문제가 꽤 있다. 웹 화면을 80%로 축소하면 내가 원래 보던 화면이 잘 뜨지만 사이트 완성도에 문제가 있음은 분명하다. 시간이 나면 고쳐야 할 부분이다. 크기별로 화면을 비교해보자. 

## 화면 크기별 외형
내부에서 화면 크기를 `small`, `medium`, `medium-wide`, `large`, `x-large`로 나누는 걸로 아는데 화면의 종류는 3개다. <u>어느 크기에서 어떤 화면이 나오는 건지 파악하고 해당 크기에서 sidebar toc 본문너비를 조정하면 해결할 수 있을 거라고 보인다.</u>

### 큰 화면  
<img width="960" alt="large" src="https://github.com/user-attachments/assets/3967d055-e8d2-4157-9f4b-db576683111d" style="border: 2px solid black;">

### <u>중간 화면</u>
<img width="960" alt="broekn" src="https://github.com/user-attachments/assets/bdfa5405-6a33-4bdd-b7eb-e92371355b73" style="border: 2px solid black;">


### 작은 화면(모바일)
<img src="https://github.com/user-attachments/assets/969bc6e8-cce7-49f8-b610-a29f1f395c42" alt="before" width="300" style="border: 2px solid black;"/>


토글 메뉴 누를 경우 :   
<img src = "https://github.com/user-attachments/assets/2e1408b2-eb58-4b27-bea6-8bb199e4f0f3" alt = "sc" width = "300" style="border: 2px solid black;"/>  


<br>

## 해결  
### 해결 방법 선택
큰 화면에선 문제 없고, 휴대폰 화면에서도 별 문제 없으니 노트북 화면에서만 해결하면 된다. 그 외에 다른 화면이 있다면 그때 해결하면 된다.  
문제를 해결할 방법은 꽤 많아 보인다. 몇 가지 써보자면 : 
- 본문 너비를 줄인다
- 양 옆 마진을 줄인다
- 사이드바와 toc의 글자 크기를 줄인다
- 사이드바와 toc의 너비를 늘린다
등이 있다. 

나는 양 옆의 마진을 줄이고, 전체적인 폰트를 줄이고, 사이드바와 toc의 너비를 늘리겠다. 모든 작업은 `_sass\minimal_mistakes/`에 있는 파일들로 한다.

### 마진 줄이기
`_page.scss`로 간다. 여기서 맨 위에 있는 `#main`의 `margin-left`와 `margin-right`을 바꿔주면 된다. 

```scss
#main {
  @include clearfix;
  margin-left: 8%; // 좌우 여백(사이드바와 본문 사이가 아니라, 사이드바보다도 왼쪽)
  margin-right: 8%;

  @include breakpoint($x-large) {
    margin-left: 12%;
    margin-right: 12%;
  }
}
```
마진과 무관한 코드는 전부 지우고 관련된 코드만 가져왔다. 화면 크기가 `x-large`가 아닌 경우 양 옆의 마진은 8%고, `x-large`인 경우는 12%로 설정했다. ` @include breakpoint($x-large)`는 if문과 같은 역할을 한다.

### 전체적인 폰트 줄이기
`_reset.scss`로 가서 화면별 폰트 크기를 바꾼다.
```scss
html {
  /* apply a natural box layout model to all elements */
  box-sizing: border-box;
  background-color: $background-color;
  font-size: 16px;

  // 화면 크기에 따른 글자 크기, 각자 글자 크기 설정 있긴 하지만 대략적으로 바꿔준다고 보면 될듯
  @include breakpoint($medium) {
    font-size: 13px;
  }

  @include breakpoint($large) {
    font-size: 15px;
  }

  @include breakpoint($x-large) {
    font-size: 18px;
  }

  -webkit-text-size-adjust: 100%;
  -ms-text-size-adjust: 100%;
}
```
h1, h2, h3의 크기나, 개별 요소에서의 폰트 크기를 설정하는 곳도 있는데 이걸 수정하는 이유는 뭘까? 여기서 폰트 크기를 바꾸면 전체 폰트에 전부 적용되기 때문이다. 제일 위의 주석을 보면 "모든 요소에 박스 레이아웃 모델을 적용한다"라고 써있다. 어떤 식으로 적용되는 건지 확인하기 위해 배경색 부분을 빨간색으로 바꿨다. 그랬더니 블로그 화면 전체가 빨간색 배경을 가지게 됐다. 여기에 쓴 건 블로그의 모든 요소에 적용된다는 걸 확인한 거다.  
하지만 신기한 점은 폰트 크기를 16px로 한다고 모든 폰트가 16px이 되지는 않는다는 점이다. 그렇다고 아무 영향이 없는 것도 아니다. 16보다 크게 하면 전체적으로 커지고, 작게 하면 전체적으로 작아진다. 폰트 크기는 `_variables.scss`에 em단위로 정의돼있는데 여기서 정의된 폰트 크기랑 무슨 관계가 있는 건지 궁금하다.

### 사이드바와 toc의 너비 늘리기
_variables.scss로 가서,
```scss
/*
   Grid
   ========================================================================== */
// toc 너비
$right-sidebar-width-narrow: 180px !default; // 200 14인치 노트북 화면일때 
$right-sidebar-width: 220px !default; // 300
$right-sidebar-width-wide: 450px !default; // 400
```
이 부분을 수정하면 된다. 이건 toc 너비인데 이것만 고쳐도 사이드바 너비는 알아서 조정된다. 왜냐면 사이드바 너비는 toc 너비를 기준으로 책정되기 때문이다. `_sidebar.scss`에서 `x-large` 화면 기준 `width: calc(#{$right-sidebar-width} - 1em);`로 정의돼있다.

<br>

## 깨달은 사실
문제 해결은 끝났다. 이제 해결 과정에서 깨달은 사실들을 정리하겠다.  

### 화면 크기의 기준
아주 오래동안 `_variables.scss`의 `breakpoint` 부분이 본문의 최대 너비 설정이라고 생각해왔었다. 물론 그냥 보면, '$small medium medium-wide large x-large를 나누는 기준을 적어둔 거구나!'라고 생각할 수 있다. 실제로도 이게 맞다. 하지만 난 이걸 바꿨더니 본문의 너비가 바뀌는 걸 확인해버렸기에 본문 너비 설정이라고 생각하게 된 거다. 사실 이건 나누는 기준일 뿐이지만 다른 곳에서 이 값을 참고해 본문 너비를 정하기에 혼란이 있었던 거다.  

### 화면 해상도 확인하기
몇 가지 실험을 거친 결과, 내 노트북은 large 판정, 데스크탑은 x-large 판정이라는 걸 알게 됐다. 왜일까? 내 노트북은 14인치 1920x1080, 내 컴퓨터는 27인치 1920x1080이다. 그래서 당연히 모니터 크기랑 상관이 있나보다, 하고 넘어갔다.  
근데 오류가 있다. 화면 크기 구분 단위가 px이다. px이란 건 해상도란 건데, 해상도는 두 모니터 모두 같다. 크롬의 검사 도구를 뜯어보던 중 <u>브라우저 해상도가 별개로 존재한다는 걸 깨달았다.</u> 그리고 이 해상도는 모니터 크기에 비례하는듯 하다.  

다른 방법을 알아내지 못해서 좀 무식하게 해야하지만, 크롬 브라우저 해상도를 확인해보자 :  

1. 브라우저에서 f12를 눌러 검사창을 띄운다.  

2. 검사창 좌측 테두리를 잡고 드래그해서 검사창 크기를 늘렸다 줄였다 해본다.
![해상도확인](https://github.com/user-attachments/assets/84ec506f-ee3c-4bc8-a0ca-fedfdc6a570c)  
그럼 위 사진에서처럼 검사창 좌상단에 화면 크기가 바뀌는 게 보일 거다. 우리는 검사창의 가로 크기만 바꾸고 있으니까 세로값은 고정돼 있다. 이 세로값을 적어둔다.  

3. 지금 보이는 브라우저의 가로값은 실제 가로값에서 검사창의 너비를 뺀 값이다. 검사창을 아래에 뜨게 해서 실제 가로값을 알아내보자.  
![move f12](https://github.com/user-attachments/assets/3c3243db-c6c2-4ab0-9d5f-32caa2aa2dff)  
위 사진과 같이 검사창 우상단의 점 3개를 누르고, 밑에 뜨는 아이콘 중 하단 정렬 아이콘을 눌러서 검사창의 배치를 바꿔준다.

4. 이제 아까한 거처럼 검사창의 상단 테두리를 잡고 위아래로 늘였다 줄였다 해본다. 가로값은 고정이니 가로값을 적는다.
![check](https://github.com/user-attachments/assets/44a8e878-25fe-486c-9eb6-0ae58ad40af7)

### 다른 화면에서 어떻게 보일지 확인하기
이 검사창에서 다른 화면에서 어떻게 보일지 확인하는 방법도 있다.

![기기변환](https://github.com/user-attachments/assets/81e6ffa0-113a-400b-9369-8d0830e40eb3)

검사창 좌상단의 아이콘을 클릭하거나, `ctrl + shift + m`을 누르면 가능하다.

![기기](https://github.com/user-attachments/assets/ffb782ae-dbd2-4dc1-94f0-e638a014aa53)

이런 창이 뜨는데 지금은 휴대폰에서 어떻게 보이는지가 나오고 있다. 위의 '반응형'이라고 쓰인 부분을 누르면 특정 기종도 선택할 수 있다. 반응형의 경우엔 특정 픽셀값을 입력해서 화면 크기를 조정할 수 있다. 내 14인치 노트북의 크롬 해상도인 1280px x 559.33px나, 27인치 모니터의 크롬 해상도인 1920 x 919px를 입력하면 다른 기기에서도 해당 기기에서 보는 화면을 볼 수 있는 거다!  

이미지 배경이 블로그 배경과 같아서 위쪽은 테두리를 따로 넣어줬는데, 형식을 다 바꿔줘야 되다보니 좀 귀찮아서 아래쪽 이미지는 안 했다... 심지어 캡쳐할 때 일부러 테두리를 같이 캡쳐한 이미지도 있다. 다음 글부터는 깔끔하고 일관성있게 잘 해봐야겠다.