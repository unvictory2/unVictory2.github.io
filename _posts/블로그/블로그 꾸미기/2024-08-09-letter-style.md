---
layout : single
title : "글자 관련 설정"
excerpt : ""
published: true

categories : 
    - Blog Decoration

toc : true
toc_sticky : true

date : 2024-08-09
last modified : 2024-08-09
---
폰트 변경은 [예전 글](https://unvictory2.github.io/blog%20decoration/changing-font/)에서 다뤘다.  
이 글에서 다룰 내용들은 대부분 `_sass\minimal-mistakes\_base.scss`내에서 이뤄진다.

## `_base.scss` 내부
### 자간, 스페이스 너비, 줄 사이 너비 설정  
`body`를 찾아서 `line-height`를 조정한다. `letter-spacing`과 `word-spacing`은 없을텐데 추가한다.
```scss
body {
  margin: 0;
  padding: 0;
  color: $text-color;
  font-family: $global-font-family;
  line-height: 1.5;
  letter-spacing: 0.04em; // 자간
  word-spacing: 0.15em; // 공백 너비
```

### Recent Posts에서 항목 사이의 거리 조절하기
```scss
h6 {
  margin: 2em 0 0.5em;
  line-height: 1.2;
  font-family: $header-font-family;
  font-weight: bold;
}
```
이 부분에서 `margin`의 `2em`을 수정하면 된다. 그 외의 설정도 수정하며 뭐가 바뀌는지 보면 좋다.

### 밑줄 꾸미기
`<u> </u>`를 썼을 때 생기는 <u>이런 밑줄 관련 설정이다.</u> 기존 스타일은 글자와 너무 가까워서 별로라 색과 함께 바꿨다.
```scss
u {  // <u> </u> 밑줄 관련 설정
  text-decoration: none;
  border-bottom: 2px solid $anso-primary-color;
  padding-bottom: 2px;
}
```


### 하이퍼링크 밑줄 없애기
`a` 부분을 찾아서 밑의 코드를 더해준다.
```scss
a { // 하이퍼링크
  text-decoration: none; // 밑줄 없애기
```

### 방문한 하이퍼링크 색 안 바뀌게 하기
```scss
  &:visited {
    //color: $link-color-visited;
  }
```
이 부분이 원래는 주석처리 돼있지 않은데 주석처리 하면 된다.


### Backtick 내부 코드블록 설정
`이 부분`에 대한 설정이다.
```scss
// 코드 블럭 내부 처리. p a li figcap td(문단 링크 리스트 사진주석 테이블) 내부의 코드 블록을 어떻게 설정할 것인지. 
p > code,
a > code,
li > code,
figcaption > code,
td > code {
  padding-top: 0.1rem;
  padding-bottom: 0.1rem;
  font-size: 0.8em;
  background: $code-background-color; // _variables.scss에서 정의됨
  color: $anso-primary-color; // _air.scss(현재 사용중인 테마)에서 정의됨
  border-radius: $border-radius;
  // 코드블럭 앞뒤로 간격을 좀 둚
  &:before,
  &:after {
    letter-spacing: -0.2em;
    content: "\00a0"; /* non-breaking space*/
  }
}
```
### 수평선 꾸미기
```scss
hr {
  display: block;
  margin: 1em 0;
  border: 0;
  border-top: 1px solid $border-color;
}
```
이 부분을 건드리면 되지만 난 아무것도 바꾸지 않았다.  

<br>

## 그 외
이 밑은 `_base.scss` 밖에서 하는 설정이다.

### 최근 포스트 제목 스타일 설정
`_archive.scss`로 가서,
```scss
.archive__subtitle { // 최근 포스트
  margin: 1.414em 0 0.5em;
  padding-bottom: 0.5em;
  font-size: $type-size-3;
  color: $muted-text-color;
  border-bottom: 1px solid $border-color;
  width: 100%; // 최근 포스트 밑에 그어져있는 줄의 길이. 부모 요소의 n%

  + .list__item .archive__item-title {
    margin-top: 0.5em;
  }
}
```
이 부분을 수정하면 된다. 

### Categories 페이지에서 항목 간의 거리 조절
마찬가지로 `_archive.scss`에서
```scss
.archive__item-excerpt {
  margin-top: 0;
  margin-bottom: 3em; // categories에서 항목 간의 마진 조정하기 위해
  font-size: $type-size-6; //_variables.scss에 정의
```
이 부분 수정.

### 글자 크기 바꾸기
두 가지 방법이 있다. 
1. `_reset.scss`에서 제일 위에 있는 `html` 내에서 폰트 크기를 바꾸면 전체적인 폰트 크기가 다같이 커지거나 작아진다. 
2. `_variables.scss`에서 하나하나 설정한다.
    ```scss
    /* type scale */
    $type-size-1: 2.441em !default; // ~39.056px
    $type-size-2: 1.953em !default; // ~31.248px
    $type-size-3: 1.563em !default; // ~25.008px
    $type-size-4: 1.25em !default; // ~20px
    $type-size-5: 1em !default; // ~16px
    $type-size-6: 0.88em !default; // ~12px. 사이드바의 각종 작은 글씨들, body 부분의 excerpt들. 
    $type-size-7: 0.6875em !default; // ~11px
    $type-size-8: 0.625em !default; // ~10px

    /* headline scale */
    $h-size-1: 1.563em !default; // ~25.008px
    $h-size-2: 1.25em !default; // ~20px
    $h-size-3: 1.125em !default; // ~18px
    $h-size-4: 1.0625em !default; // ~17px
    $h-size-5: 1.03125em !default; // ~16.5px
    $h-size-6: 1em !default; // ~16px
    ```
    `type-size`는 여기저기서 쓰이는 폰트 크기를 정의해두는 곳이고, `h-size`는 아마 마크다운 문법 중 `#제목#`으로 제목을 꾸미는 것과 관련 있을 거다.

### 임의의 스타일 파일 추가
`assets\css\main.scss`에서 스타일 코드를 추가할 수도 있다. `main.scss`가 없다면 추가한다. 예를 들어 오늘 전에는 자간과 공백 너비를 어디서 조정하는지 못 찾겠어서 `main.scss`에 하단의 코드를 추가해서 썼었다. 
```scss
// 글자 설정
body {
    letter-spacing: 0.04em; // 자간
    word-spacing: 0.15em; // 공백 너비
}
```
지금은 _base.scss에서 `body`를 찾아 거기다가 옮겼다. 이걸 어디서 include하라는 얘기를 들은 것도 같은데 난 처음부터 `main.scss`가 존재했어서 별다른 설정 없이 됐던 것 같다.