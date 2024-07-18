---
layout : single
title : "테마 바꾸기"
excerpt : "가장 쉽게 색을 바꾸는 방법"
published: true

categories : 
    - Blog

date : 2024-07-18
last modified : 2024-07-18
---

## 1. 테마 고르기
블로그의 색을 바꾸는 가장 쉬운 방법은 테마를 바꾸는 거다. Minimal Mistakes에서 제공해주는 테마와 그 외형은 [공식 사이트](https://mmistakes.github.io/minimal-mistakes/docs/configuration/)에서 확인할 수 있다.  
전체적인 색이 마음에 든다면 세부적으로 색 한두개가 마음에 안 들어도 상관없다. 어차피 모든 색은 자기 마음대로 바꿀 수 있다.

## 2. 테마 적용하기
루트 디렉토리의 `_config.yml`을 연다. 여기서 블로그명, 테마, 언어, 주소, 글쓴이 정보 등 블로그에 대한 전반적인 정보를 수정할 수 있다.  
![theme ](https://github.com/user-attachments/assets/2c8c2e2f-f88e-4cbf-a07f-9f83ccc1d0fb)
`theme` 부분을 찾아서 `minimal_mistakes_skin : ` 뒤를 자기가 고른 테마명으로 바꾸면 된다. 나는 `air`를 사용중이다.   
참고로 theme과 remote_theme 중 어느 걸 주석처리 하는지는 블로그를 처음 만들 때 결정났었다. 이후에는 신경쓰지 않아도 된다.  

## 3. 테마 색 변경하기
아까 말했듯이 테마의 색은 변경 가능하다. `_sass\minimal-mistakes\skins\`에서 자기가 고른 테마 파일을 연다. 내 경우에는 `_air.scss`다. 여기서 각 색을 변경하면 된다.   
`$background-color`처럼 직관적인 이름도 있지만, `$muted-text-color`처럼 조금 덜 직관적인 이름도 있다. 이런 경우에는 VSC 기준 `ctrl + shift +  f`를 누르고 검색해서 블로그 내 어디에서 이 색들을 쓰는지 볼 수 있다. 



1. 밑줄 없애기 base.scss
2. 본문 너비 바꾸기 _page.scss의 margin과 width (x)
3. 글자 크기 바꾸기 _reset.scss
4. 사이드바 너비 바꾸기 _variables.scss
5. 글자 자간, 스페이스 너비 바꾸기 main.scss
6. favicon 추가
7. yaml front matter에 넣을 수 있는 거 모음
8. 블로그 공부 방법 : ctrl shift f, 개발자도구, 남의꺼 뜯어보기
9. backtick 내부 색 코드블록 안 설정
10. 날짜표현방식, subtitle, yaml 기본값 설정 등 config.yml 내부 세팅
11. 사이드바 추가 (카테고리)
12. 대문 바꾸기
13. 검색 기능 만들기
14. 가운데 정렬
15. 우상단 메뉴
16. excerpt 가운데 정렬
17. 링크 색 방문해도 안 바뀌게
18. 최근 포스트 글자 크기
19. 밑줄 설정 