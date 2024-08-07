---
layout : single
title : "[핵심] 블로그 구성 파일들의 역할"
excerpt : "어떤 파일이 뭘 하는지 알아보자"
published: true
toc : true
toc_sticky : true

categories : 
    - Blog Decoration

date : 2024-07-18
last modified : 2024-08-07
---

## 구조 파악에 도움이 되는 도구들
1. chatGPT
2. 식빵맘님 블로그 및 깃허브의 소스
3. 브라우저의 개발자 도구 기능과 VSC의 `ctrl+shift+f` 찾기 기능
  
  놀랍게도 가장 도움이 된 건 초중반엔 식빵맘님이고, 후반에는 개발자 도구 기능이었다. 챗지피티는 다른 분야에선 엄청난 성능을 보여줄 때가 많지만 이번엔 크게 도움이 되진 않았다.

  <br> 

## 루트 디렉터리 구조

```
📦unVictory2.github.io
 ┣ 📂.git
 ┣ 📂.github
 ┣ 📂.jekyll-cache
 ┣ 📂assets
 ┣ 📂pages
 ┣ 📂_data
 ┣ 📂_includes
 ┣ 📂_layouts
 ┣ 📂_posts
 ┣ 📂_sass
 ┣ 📂_site
 ┣ 📜.editorconfig
 ┣ 📜.gitattributes
 ┣ 📜.gitignore
 ┣ 📜.travis.yml
 ┣ 📜android-chrome-192x192.png
 ┣ 📜android-chrome-512x512.png
 ┣ 📜apple-touch-icon.png
 ┣ 📜banner.js
 ┣ 📜favicon-16x16.png
 ┣ 📜favicon-32x32.png
 ┣ 📜favicon.ico
 ┣ 📜Gemfile
 ┣ 📜Gemfile.lock
 ┣ 📜index.html
 ┣ 📜LICENSE
 ┣ 📜minimal-mistakes-jekyll.gemspec
 ┣ 📜package.json
 ┣ 📜Rakefile
 ┣ 📜README.md
 ┣ 📜site.webmanifest
 ┣ 📜staticman.yml
 ┗ 📜_config.yml  

 (참고로 이 트리 구조는 VSC 플러그인 중 file-tree-generator를 사용하면 자동으로 생성해준다)
 ```

 모든 파일에 대해 서술할 건 아니고, 내가 블로그를 꾸미면서 자주 쓰고 무슨 기능을 하는지 공부한 파일들을 설명하겠다. 내가 사용하면서 직접 알아낸 사실들과 거기서 내린 결론들도 쓸 것이기 때문에 여기 적힌 내용이 정답이 아닐 수 있다. 

<br/>

## 루트

```
 📦unVictory2.github.io
 ┣ 📜android-chrome-192x192.png
 ┣ 📜android-chrome-512x512.png
 ┣ 📜favicon-16x16.png
 ┣ 📜favicon-32x32.png
 ┣ 📜favicon.ico
 ┣ 📜index.html
 ┗ 📜_config.yml 
```
이것보다 더 많은 파일이 있지만 기능을 몰라 제외했다.  

### 1. Favicon 관련 파일들  
`Favicon`을 설정하면 브라우저의 탭에 나타나는 아이콘을 설정할 수 있다. `android-chrome`과 `favicon`은 모두 이걸 설정하는 과정에서 내가 넣었거나 자동으로 생성된 파일들이다.

### 2. index.html
블로그에 처음 들어오면 사용자에게 보여줄 페이지, 즉 대문이다. 보통 YAML Front Matter만 쓰고 본문에는 아무 내용도 쓰지 않는다. 개발 지식이 부족해서 왜 그러는 건지 정확하게 이해는 못 했지만 객체지향언어들의 main 클래스에서 run 함수만 실행시키는 것과 비슷한 맥락 아닐까 싶다.  

이 파일의 `layout`은 보통 `home`으로 해둔다. 다른 걸로 바꾸는 것도 가능하긴 하지만, 좋은 생각은 아니다. 왜냐면 `home.html`부터는 이 파일과는 다르게 페이지에 대한 세부 설정이 들어가있기 때문이다. `home.html`은 `index.html`에서만 쓰기 때문에 내가 원하는 대문 설정을 마음껏 할 수 있다. 하지만 다른 레이아웃들은 `index.html` 이외의 곳에서도 사용되기 때문에, 마음껏 대문에 적용될 설정을 할 수 없다.  
또한 `home`의 레이아웃은 `archive`인 반면, 다른 레이아웃들 html파일들의 레이아웃은 제각각이기 때문에 `home`이 아닌 걸 쓰게 되면 뭔가가 꼬여서 예상하지 못한 동작이 일어날 수 있다. 실제로 블로그를 꾸미는 과정에서 이런 문제들을 겪었는데 자세한 건 해당 주제의 글에서 언급하고 링크를 달거나 하겠다.  

참고로 index.html의 YAML Front Matter에서 `permalink : \`로 루트 주소를 주려고 하면 문제가 생긴다. 이 또한 관련된 글에서 쓰고 여기다 링크를 달던가 하겠다.

### 3. config.yml  
말 그대로 블로그 전체에 대한 온갖 설정을 하는 곳이다. 블로그 기본 언어, 이름, 글쓴이 이름, 댓글 방식, 검색, bio에 쓸 정보 등 다양한 정보를 쓴다.  
가장 밑에 있는 `default`에 가보면, 글을 쓸 때 YAML Front Matter에 미리 넣어둘 기본값을 설정할 수 있다. 예를 들어 `sidebar_main: true`를 써둘 경우 개별 글에서 따로 언급하지 않는다면 이 설정은 언제나 참이다.

<br/>

## assets  
```
📦assets
 ┣ 📂css
 ┃ ┗ 📜main.scss
 ┣ 📂fonts
 ┃ ┗ 📜Sagak-sagak.woff
 ┣ 📂images
 ┃ ┣ 📜20230830_191048.jpg
 ┃ ┣ 📜brownie.png
 ┃ ┣ 📜desktop.ini
 ┃ ┣ 📜profile_image.jpg
 ┃ ┣ 📜profile_image_old.jpg
 ┃ ┣ 📜random_image1.jpg
 ┃ ┣ 📜sky.png
 ┃ ┣ 📜thinking_face_192x192.png
 ┃ ┣ 📜title_image.jpg
 ┃ ┗ 📜title_image_og.jpg
 ┗ 📂js
 ```
 `js`폴더 내부에는 다른 파일들이 더 있지만 뭔지도 모르고 건드릴 일도 없었기에 제외했다.

### css, fonts, images
이 세 개의 폴더를 한 번에 설명할 수 있다. 내가 따로 추가한 데이터들이다.  

`main.scss`는 기존에 있는 scss들에 추가할 디자인 요소를 가지고 있다. 디자인 요소를 추가할 때 해당되는 scss 파일을 찾아서 거기에 추가해도 되고, 여기에 추가해도 되는 것 같다. 웬만한 건 해당 scss 파일에 추가하는 게 맞다고 생각돼서 그렇게 했다. 지금은 내가 추가한 폰트 경로와 설정, 자간, 단어간 공백 너비 설정만 넣어놨다. 

`fonts`에 있는 파일은 폰트 파일이다. 원래는 인터넷 다운로드 주소를 주는 식을 해서 내 리포지토리가 폰트 파일을 가지고 있을 필요는 없으나 알 수 없는 이유로 안 돼서 직접 다운받아놨다.

`images`에 있는 파일들은 블로그 여기저기서 지금 쓰고 있거나 임시로 넣어둔 이미지들이다.

<br/>

## pages
```
📦pages
 ┣ 📂categories
 ┃ ┣ 📜category-blog.md
 ┃ ┣ 📜category-git.md
 ┃ ┗ 📜category-miraework-education.md
 ┣ 📜about.md
 ┗ 📜category-archive.md
```
`pages`는 다른 사람들의 블로그엔 기본적으로 존재하는 폴더지만, 내 블로그 리포지토리에는 없었다. 이유는 아직도 잘 모르겠다... 처음 만들고 1년이 지나서야 본격적으로 설정하다보니 이런 면이 어렵다. 처음 만들 땐 있었는데 내가 삭제한 건지, 뭔가 잘못 만들었던 건지 기억이 안 난다.  

어쨌든 여기엔 블로그 곳곳의 메뉴에 쓸 마크다운 파일들이 모여있다. `about`은 블로그 우상단에서 해당 메뉴를 누르면 보여줄 자기소개 페이지다. `category-archive`는 그 옆 메뉴인 `Categories`를 누르면 보여줄 카테고리별 글을 모아둔 페이지다. 마지막으로 `categories` 폴더 내의 파일들은 각자 해당하는 카테고리의 글을 모아둔 페이지다.   
`category-archive.md`는 전체 카테고리를 모아서 보여주는 페이지고, `categories` 폴더 내의 페이지들은 각자 하나의 카테고리 글들을 세부적으로 보여주는 페이지라는 차이가 있다.

<br/>

## _data
```
📦_data
 ┣ 📜navigation.yml
 ┗ 📜ui-text.yml
```
블로그에 뜨는 텍스트에 대한 데이터들이 들어있다.  
`ui-text.yml`에는 각 언어별로 블로그의 UI를 어떻게 표시할지(영어면 Updated, 한국어면 업데이트 등)가 적혀있다. `navigation.yml`에는 블로그 우상단 메뉴에 띄울 메뉴와, 그걸 누르면 연결될 링크가 적혀있다. 지금은 About, Categories 2개 뿐이다.  
이 폴더 이름이 `_data`인 게 속성을 잘 표현해 주는지 모르겠다. 이 블로그에는 정말 많은 종류와 양의 데이터가 있던데 그 중 뭐가 들어있는지에 대한 정보를 전혀 제공하지 않는다. 

<br/>

## _includes
```
📦_includes
 ┣ 📂analytics-providers
 ┣ 📂comments-providers
 ┣ 📂footer
 ┣ 📂head
 ┣ 📂search
 ┣ 📜analytics.html
 ┣ 📜archive-single.html
 ┣ 📜author-profile-custom-links.html
 ┣ 📜author-profile.html
 ┣ 📜breadcrumbs.html
 ┣ 📜category-list.html
 ┣ 📜comment.html
 ┣ 📜comments.html
 ┣ 📜documents-collection.html
 ┣ 📜feature_row
 ┣ 📜figure
 ┣ 📜footer.html
 ┣ 📜gallery
 ┣ 📜group-by-array
 ┣ 📜head.html
 ┣ 📜masthead.html
 ┣ 📜nav_list
 ┣ 📜nav_list_main
 ┣ 📜page__date.html
 ┣ 📜page__hero.html
 ┣ 📜page__hero_video.html
 ┣ 📜page__meta.html
 ┣ 📜page__taxonomy.html
 ┣ 📜paginator.html
 ┣ 📜posts-category.html
 ┣ 📜posts-tag.html
 ┣ 📜post_pagination.html
 ┣ 📜scripts.html
 ┣ 📜seo.html
 ┣ 📜sidebar.html
 ┣ 📜skip-links.html
 ┣ 📜social-share.html
 ┣ 📜tag-list.html
 ┣ 📜toc
 ┣ 📜toc.html
 ┗ 📜video
```
길어서 폴더의 내용물들은 따로 뺐다.  
`_includes`와 `_layouts`에 모든 html 파일들이 모여 있다. `_layouts`에는 포스트 레이아웃에 대한 html 파일들이 모여있고, `_includes`에는 포스트 레이아웃 html 파일들이 재료로 쓰는 html 파일들을 포함해서 포스트 외 부분을 구성하는 html 파일들이 모여있다.

### 루트
포스트 레이아웃을 제외하고 이 블로그의 모든 것의 구조가 모여있다. 프로필, breadcrumbs(보고 있는 글의 디렉토리 경로 표시), 댓글, footer, masthead, 좌측의 카테고리 사이드바, toc, 글 끝의 공유하기 버튼, 태그 정렬 버튼 등 정말 모든 구조다. 내가 뭔지 아는 것만 이 정도고, 저 목록을 보면 내가 언급하지 않은 파일도 많다. 즉 블로그 구조를 바꾸고 싶을 때 웬만하면 보게 되는 곳이다.  

그 중 내가 직접 썼던 몇 가지를 써보자면,  
`nav_list_main`은 카테고리별 글을 사이드바에 추가로 보여주기 위해 만든 파일이다. 말단의 카테고리 자체는 블로그 글들의 YAML Front Matter에 쓴 카테고리와 맞춰줘야 하지만, 그 카테고리를 부를 명칭이나 상위 카테고리의 명칭은 마음대로 정할 수 있다. 이 때 겉으로 보이는 명칭은 한국어로 해도 상관 없지만, <u>내부적인 카테고리는 반드시 영어로</u> 해주는 게 좋다. 그렇지 않으면 breadcrumb에서 한글이 깨져서 나오고, 여기저기서 카테고리를 똑같이 써줘야 하는데 실수가 나올 확률이 높다.  
`sidebar.html` 파일 마지막을 보면 YAML Front Matter의 `sidebar_main`이 true라면 이 파일을 include하는 코드가 있다. 당연한 소리지만 마지막에 넣지 않으면 bio보다 위에 나타난다. 블로그 관련한 파일들에서 코드 위치가 중요하지 않은 경우가 좀 있다보니 아무 생각 없이 이상한 곳에 넣었던 적이 있다.

`paginator` 같은 경우는 글들을 하나의 목록으로 보여주는 역할을 한다. 한 페이지에 몇 개의 글을 보여줄지 정하고, 넘어갈 경우 새 페이지를 만들어서 넘겨보며 만들 수 있게 해준다. 내 블로그의 최신 포스트 부분을 담당한다.  

블로그 대문에 가로로 사진을 길게 나오게 하려고 파일들을 타고 돌아다니다 알게 된 건데, page__가 붙어있는 파일들은 페이지 레이아웃과 연관있는 파일들이다. 예를 들어 `splash.html`같은 몇몇 레이아웃은 `page__hero.html`나 `page__hero_video.html`을 부르는데 이 파일들은 블로그 대문에 이미지나 영상을 제목과 함께 출력해준다.

### analytics, 댓글, 검색
```
 ┣ 📂analytics-providers
 ┃ ┣ 📜custom.html
 ┃ ┣ 📜google-gtag.html
 ┃ ┣ 📜google-universal.html
 ┃ ┗ 📜google.html
 ┣ 📂comments-providers
 ┃ ┣ 📜custom.html
 ┃ ┣ 📜custom_scripts.html
 ┃ ┣ 📜discourse.html
 ┃ ┣ 📜disqus.html
 ┃ ┣ 📜facebook.html
 ┃ ┣ 📜giscus.html
 ┃ ┣ 📜scripts.html
 ┃ ┣ 📜staticman.html
 ┃ ┣ 📜staticman_v2.html
 ┃ ┗ 📜utterances.html
 ┗ 📂search
   ┣ 📜algolia-search-scripts.html
   ┣ 📜google-search-scripts.html
   ┣ 📜lunr-search-scripts.html
   ┗ 📜search_form.html
```
블로그 댓글은 `discourse`, `disqus`, `utterances` 다양한 플랫폼 중 하나를 골라서 사용하게 된다. 예를 들어, 내 블로그에는 `utterances` 플랫폼을 사용 중이다. 여기 있는 html 파일들은 각 플랫폼별로 다르게 생긴 댓글 달기 부분을 정의하는 것 같다. 파일 제목을 보니 `analytics-providers`나 `search` 쪽도 마찬가지인듯 하다.

### head, footer
```
 ┣ 📂footer
 ┃ ┗ 📜custom.html
 ┗ 📂head
   ┗ 📜custom.html
```
둘 다 들어가보면 `head`나 `footer`에 대한 구조는 없다. 만약 구조를 자기가 바꾸고 싶다면 여기에서 하는 게 아닌가 싶다. 나는 바꾸지 않았기 때문에 별 게 없는 거고. `head`쪽에는 favicon을 불러오는 코드가 있긴 하다. 하지만 뭔가를 하지는 않는 걸로 추정된다.

<br/>

## _layouts
```
📦_layouts
 ┣ 📜archive-taxonomy.html
 ┣ 📜archive.html
 ┣ 📜categories.html
 ┣ 📜category.html
 ┣ 📜collection.html
 ┣ 📜compress.html
 ┣ 📜default.html
 ┣ 📜home.html
 ┣ 📜posts.html
 ┣ 📜search.html
 ┣ 📜single.html
 ┣ 📜splash.html
 ┣ 📜tag.html
 ┗ 📜tags.html
```
여기는 블로그 개별 글들의 레이아웃에 대한 html 파일들이 있다. 재밌는 건 이 레이아웃 파일들이 서로 연관돼있는 레이아웃도 있다는 점이다. 예를 들어 `categories.html`의 레이아웃은 archive기 때문에, `archive.html`을 기반으로 한다. `archive.html`의 레이아웃은 `default.html`이기 때문에 결국 `categories.html`은 2개의 다른 레이아웃 파일을 참고한다.  
이 레이아웃 파일들은 `_includes` 폴더의 루트에 있는 여러 html 파일들을 가져와서 사용한다. 예를 들어 `default.html`은 `head.html, masthead.html, search_form.html, scripts.html,footer.html` 등 다양한 html 파일들을 가져온다. <u>사실상 레이아웃은 `_includes` 폴더에 있는 개별 위젯들을 어떻게 배치할지 정해둔 파일이라고 판단된다.</u> 아마 그래서 폴더 이름도 includes인 게 아닐까? 단독으로 사용되기 보다는 여기저기서 include 되니까.

<br/>

## _posts
```
📦_posts
 ┣ 📂깃
 ┃ ┗ 📜2024-07-02-learning-git.md
 ┣ 📂미래일경험
 ┃ ┣ 📜2024-06-24-mirae-work-day-1.md
 ┃ ┣ 📜2024-06-25-mirae-work-day-2.md
 ┃ ┣ 📜2024-06-26-mirae-work-day-3.md
 ┃ ┣ 📜2024-06-28-mirae-work-day-4.md
 ┃ ┗ 📜2024-07-16-final-application.md
 ┗ 📂블로그
 ┃ ┣ 📜2023-04-05-first-post.md
 ┃ ┣ 📜2023-04-05-remember.md
 ┃ ┣ 📜2024-06-24-post-layout-problems.md
 ┃ ┣ 📜2024-06-25-blog-todo.md
 ┃ ┣ 📜2024-07-11-changing-font.md
 ┃ ┣ 📜2024-07-11-inserting_image.md
 ┃ ┣ 📜2024-07-18-blog-structure.md
 ┃ ┣ 📜2024-07-18-changing-theme.md
 ┃ ┗ 📜2024-07-18-masthead.md
```
지금까지 내가 쓴 글이 모여있는 곳이자, 새 글을 쓸 때 파일을 추가하면 되는 곳이다.  
제목은 날짜와 제목으로 하며, 뒤에 `.md`를 붙이고, 제목은 웬만하면 영어로 한다. 글 내부에서 YAML Front Matter만 써주면 새 글이 된다. VSC나 다른 IDE를 사용해서 작성하는 게 좋으며 깃허브 마크다운 미리보기 플러그인으로 미리 보면서 쓰면 편하다.  
내부의 폴더들은 만들어도 되고 안 만들어도 된다. 난 관리할 때의 편리함을 위해 만들었다.

<br/>

## _sass
```
📦_sass
 ┣ 📂minimal-mistakes
 ┃ ┣ 📂skins
 ┃ ┃ ┣ 📜_air.scss
 ┃ ┃ ┣ 📜_aqua.scss
 ┃ ┃ ┣ 📜_contrast.scss
 ┃ ┃ ┣ 📜_dark.scss
 ┃ ┃ ┣ 📜_default.scss
 ┃ ┃ ┣ 📜_dirt.scss
 ┃ ┃ ┣ 📜_mint.scss
 ┃ ┃ ┣ 📜_neon.scss
 ┃ ┃ ┣ 📜_plum.scss
 ┃ ┃ ┗ 📜_sunrise.scss
 ┃ ┣ 📂vendor
 ┃ ┃ ┣ 📂magnific-popup
 ┃ ┃ ┗ 📂susy
 ┃ ┣ 📜_animations.scss
 ┃ ┣ 📜_archive.scss
 ┃ ┣ 📜_base.scss
 ┃ ┣ 📜_buttons.scss
 ┃ ┣ 📜_footer.scss
 ┃ ┣ 📜_forms.scss
 ┃ ┣ 📜_masthead.scss
 ┃ ┣ 📜_mixins.scss
 ┃ ┣ 📜_navigation.scss
 ┃ ┣ 📜_notices.scss
 ┃ ┣ 📜_page.scss
 ┃ ┣ 📜_print.scss
 ┃ ┣ 📜_reset.scss
 ┃ ┣ 📜_search.scss
 ┃ ┣ 📜_sidebar.scss
 ┃ ┣ 📜_syntax.scss
 ┃ ┣ 📜_tables.scss
 ┃ ┣ 📜_utilities.scss
 ┃ ┗ 📜_variables.scss
 ┗ 📜minimal-mistakes.scss
```
`vendor` 폴더 내부는 깊게 설명하지 않을 거라 생략했다.  
`_sass` 폴더는 온갖 scss 파일들이 모여있는 곳이다. scss를 기반으로 css가 만들어진다고 하고, 실제로 scss 파일에 문제가 생기니 css 파일이 없다는 에러가 뜨는 걸로 봐서 둘이 확실히 연결돼있다. 즉 색이나 기존 css 파일의 기능을 하는 파일들이 여기 있는데, 모든 부분이 뭘 하는지 파악했다기 보다는 필요할 때 열어보고 수정하는 식으로 진행했다. 그렇기 때문에 각 파일에서 다루는 기능을 전부 다루기 보다는 내가 봤던 기능을 한 두개만 언급하고 넘어갈 거다.  

참고로 이런 꾸미기 요소는 생각보다 여기저기서 정의돼있어서, override돼서 적용이 안 된다거나 하는 식으로 예상하지 못한 동작이 있을 수 있다. 웬만하면 <u>무작정 바꾸기 말고 개발자 모드를 사용해서 뭘 바꿔야되는지 알아내고 바꾸자.</u>

### 루트
루트 디렉토리에는 이름을 보면 어느 부분의 sass 파일인지 파악할 수 있는 `_footer.scss, masthead.scss, search.scss, sidebarr.scss, sidebar.scss` 같은 파일들이 있다. 그 외 몇 개 파일에 대해서 알아보자.  

`_base.scss`에서는 온갖 잡다한 설정을 할 수 있다. 나는 여기서 하이퍼링크 밑줄 빼기, 밑줄 스타일 바꾸기, backtick 내부 (`여기 안을 뜻한다`) 꾸미기 등을 했다. 특히 backtick 내부 꾸미기는 어디서 하는 건지 몰라서 꽤 헤맸었는데 여기서 하는 거였다! `td > code` 뒤에 중괄호를 열고 원하는 스타일을 넣으면 된다. 그 외에도 문단, 링크, 리스트, 주석, 테이블에 관련된 꾸미기 설정을 바꿀 수 있다. 글자 크기나 본문의 마진도 설정할 수 있으나 변수로 설정돼있고 변수의 정의는 다른 곳에서 돼있기 때문에 굳이 여기서 하드코딩 할 이유는 없다.  
`_forms.scss`는 모든 기능을 파악하진 못했지만 체크박스, 라디오버튼이나 이미지와 관련된 설정을 할 수 있다. 체크박스가 체크됐을 때 디자인을 바꾸기 위해 수정한 적이 있다.  
`_navigation.scss`에도 여러 코드가 있지만 난 그 중 breadcrumbs와 관련된 설정을 수정했었다. breadcrumbs의 마진, 패딩이나 글씨체를 바꿀 수 있다.  

`page.scss`는 예전엔 뭔지 몰라서 그냥 냅뒀으나 오늘 새로운 점을 꽤 찾아내 별도의 문단으로 쓴다. 이 파일에서는 <u>글의 본문 영역과 관련된 온갖 설정</u>을 할 수 있다. 
- 너비 관련 : 잘 몰랐던 땐 `_variables.scss`에서 breakpoint를 수정했으나, 잘못된 방법이다. 그럼에도 불구하고 본문의 너비를 넓힌다는 목적을 달성할 수 있었던 건 둘이 관련이 있는 건 사실이기 때문이다. 바람직한 방법은 `page.scss`의
  ```scss
  .page__inner-wrap {
    float: left;
    margin-top: 1em;
    margin-left: 0;
    margin-right: 0;
    width: 100%; // 본문 너비
    clear: both;
    ```
    이 부분을 수정하는 거다. 바로 밑의 `.page_share`를 수정해도 바뀌긴 하지만 바람직한 방법은 아닌듯하다. 이러면 우측으로만 본문 영역이 늘어나는데, 좌측으로 늘리고 싶다면 본문 영역 전체(본문+사이드바+toc)의 좌측 마진을 줄이면 된다.
- 영역 전체 마진 관련 : 
  ```scss
  #main {
  @include clearfix;
  margin-left: 8%; // 좌우 여백(사이드바와 본문 사이가 아니라, 사이드바보다도 왼쪽)
  margin-right: 8%;
  padding-left: 1em;
  padding-right: 1em;
  -webkit-animation: $intro-transition;
  animation: $intro-transition;
  max-width: 100%;
  -webkit-animation-delay: 0.15s;
  animation-delay: 0.15s;

  @include breakpoint($x-large) {
    max-width: $max-width;
    margin-left: 12%;
    margin-right: 12%;
    }
  }
  ```
  이 부분을 수정하면 된다. margin 관련된 부분으로 본문 영역을 제외한 빈 공간의 너비를 조정할 수 있다. 밑의 `@include breakpoint($x-large)`는 화면 크기가 클 경우 별도의 마진값을 주기 위해 작성됐다. 참고로 `max-width` 같은 경우는 바꿔도 다른 곳에서 override 되는지 외형에 영향을 주지 못한다.
- 본문 너비를 정하는 공식 : 
  ```scss
    padding-left: 1em;
  @include breakpoint($large) {
    float: right;
    width: calc(100% - #{$right-sidebar-width-narrow});
    padding-right: $right-sidebar-width-narrow;
  }

  @include breakpoint($x-large) {
    width: calc(100% - #{$right-sidebar-width});
    padding-right: $right-sidebar-width;
    padding-left: 0em;
  }
  ```
  이런 식으로 본문 너비가 대체 어떻게 정해진 건지, 어떤 식으로 떠있는 건지를 정하는 코드도 여기에 있다. 
- 대문에 이미지를 넣는 부분 : 
  ```scss
    &--overlay {
    position: relative;
    margin-bottom: 2em;
    padding: 3em 0;
    @include clearfix;
    background-size: cover;
    background-repeat: no-repeat;
    background-position: center;
    -webkit-animation: $intro-transition;
    animation: $intro-transition;
    -webkit-animation-delay: 0.25s;
    animation-delay: 0.25s;
    ```
  이렇게 내 대문에 이미지를 어떤 형식으로 넣는지 설정하는 부분도 있는 거 같다!  
  이 외에도 정말 본문 영역을 구성하는 여러가지에 대한 코드가 있는 걸로 보이는데, 아직 바꿀 일이 없어서 나머지는 확인하지 않았다.

`reset.scss`에서는 화면 크기에 따른 글자 크기를 정의할 수 있다. x-large일 땐 몇 포인트, large일 땐 몇 포인트, medium일 땐 몇 포인트 등.  
`sidebar.scss`에서는 사이드바와 관련된 설정들을 할 수 있다. 마진을 이용해 사이드바와 본문 사이의 거리를 설정하고, 사이드바 투명도를 조절하고, 프로필 사진의 모양을 바꿨다.  
`_syntax.scss`에서는 backtick 3개로 둘러쌓인 코드블록,
```
즉 이거
```
의 외형에 대한 설정을 할 수 있다. 뭔가를 수정하진 않았고 backtick 하나인 코드블록 스타일을 여기서 바꾸는 건가 싶어 살펴봤었다.  
`_variables.scss`에선 다른 여러 파일에서 쓰는 변수들이 등장한다. 예를 들어 글자 크기인 `$type-size-1~8`, 곳곳에서 쓰이는 `$gray`의 정의, 화면 크기별 toc의 너비 등이 정의된다. 결국 override 되지만 그렇지 않을 경우 사용됐을 값들도 있다. 예를 들어 테마가 적용되지 않았을 경우 쓸 색들, 링크색, syntax 색이 있다.  
또 주석으로 `Breakpoints`라고 돼있는 부분이 있다. 화면 크기에 따라 다른 픽셀값이 적혀있는데 이 값은 본문이 몇 픽셀까지 공간을 차지할 수 있는지, 즉 <u>본문의 너비</u>다. 예전에는 본문의 너비를 바꾸기 의해 `_archive.scss`나 `_page.scss`에서 `width`를 바꿨었는데, 그러면 하나하나 바꿔줘야 하기 때문에 안 좋은 방법이다. `width`는 100%로 냅두고 이걸 바꾸면 다같이 바꿀 수 있다. 'width = 100%면 그 100%는 몇 픽셀인데?'라는 궁금증이 있으면 볼 부분.

이 부분은 최근 틀렸다는 걸 깨달았다. `breakpoint`는 화면 크기에 따라 화면을 `$large`같은 여러 변수로 나눈다. 이 변수들은 다른 코드들에서 화면 크기에 따라 다른 구성 요소들의 크기를 바꾸는 데 사용된다. 사이트 외형을 바꾸려고 파일들을 열어보다 보면 `$large`나 `$x-large`같은 게 자주 튀어나오는데 그걸 정의하는 곳이 여기다. 참고로 여기서 나오는 픽셀은 인터넷 브라우저의 해상도지 모니터의 해상도나 크기와는 연관이 없다.

### skins와 vendor
`skins` 폴더에는 선택할 수 있는 블로그 테마들이 모여있고, 각 테마의 색을 들어가서 직접 변경할 수 있다. `vendor` 폴더는 내부의 파일들을 표시하지 않았는데, 안에 많은 파일들이 있으나 정확한 기능을 알아보지 않았기 때문이다.  


<br/>

## _site
왜 있는지도 모르겠고, 남의 블로그 리포지토리엔 없다. 삭제해보고 별 일이 생기나 볼 예정이다.

<br>

## 마치며
사실 직접 블로그를 꾸며보기 전까지 이런 구조를 알기는 어렵다. 파일들을 살펴볼 동기가 부족하기 때문이다. 하지만 블로그를 꾸미고 자기가 원하는 기능을 구현하기 위해서 검색하고 하다 보면 자연스럽게 파일들의 기능을 깨닫게 된다. 난 처음엔 정말 뭐가 뭔지 하나도 몰랐는데, 하다보니 마지막엔 뭘 수정하려면 대충 어느 파일에 가야되는지 감이 잡혔다.  
정보를 얻는 난이도는
1. gpt가 대답해주는 문제
2. 블로그에 나오는 문제
3. 직접 개발자 도구를 키거나 파일을 보면서 해결해야 되는 문제  

순으로 어렵지만 어려운 만큼 뿌듯함이 함께 오기 때문에 블로그를 꾸미는 과정은 대부분 즐거웠다. 내 블로그의 변화 과정을 올려둔다. 참고로 1은 대문이 아니라 포스트 화면이다. 대문 사진은 안 찍어놓은듯...

### 1. 첫 모습
![태초의 모습](https://github.com/user-attachments/assets/1e129fce-710a-49a7-a8cc-f5032c17b3f2)

### 2. 바뀌는 모습
![블로그 진화2호](https://github.com/user-attachments/assets/07c64e5c-36ec-43a9-a9b4-78a805efc85c)

### 3. 최종
![3](https://github.com/user-attachments/assets/50ee3071-96fb-4386-af46-ff05a7297f6d)