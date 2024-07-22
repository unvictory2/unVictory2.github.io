---
layout : single
title : "블로그 구성 파일들의 역할"
excerpt : "어떤 파일이 뭘 하는지 알아보자"
published: true

categories : 
    - Blog

date : 2024-07-18
last modified : 2024-07-18
---

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

#### 1. Favicon 관련 파일들  
`Favicon`을 설정하면 브라우저의 탭에 나타나는 아이콘을 설정할 수 있다. `android-chrome`과 `favicon`은 모두 이걸 설정하는 과정에서 내가 넣었거나 자동으로 생성된 파일들이다.

#### 2. index.html
블로그에 처음 들어오면 사용자에게 보여줄 페이지, 즉 대문이다. 보통 YAML Front Matter만 쓰고 본문에는 아무 내용도 쓰지 않는다. 개발 지식이 부족해서 왜 그러는 건지 정확하게 이해는 못 했지만 객체지향언어들의 main 클래스에서 run 함수만 실행시키는 것과 비슷한 맥락 아닐까 싶다.  

이 파일의 `layout`은 보통 `home`으로 해둔다. 다른 걸로 바꾸는 것도 가능하긴 하지만, 좋은 생각은 아니다. 왜냐면 `home.html`부터는 이 파일과는 다르게 페이지에 대한 세부 설정이 들어가있기 때문이다. `home.html`은 `index.html`에서만 쓰기 때문에 내가 원하는 대문 설정을 마음껏 할 수 있다. 하지만 다른 레이아웃들은 `index.html` 이외의 곳에서도 사용되기 때문에, 마음껏 대문에 적용될 설정을 할 수 없다.  
또한 `home`의 레이아웃은 `archive`인 반면, 다른 레이아웃들 html파일들의 레이아웃은 제각각이기 때문에 `home`이 아닌 걸 쓰게 되면 뭔가가 꼬여서 예상하지 못한 동작이 일어날 수 있다. 실제로 블로그를 꾸미는 과정에서 이런 문제들을 겪었는데 자세한 건 해당 주제의 글에서 언급하고 링크를 달거나 하겠다.  

참고로 index.html의 YAML Front Matter에서 `permalink : \`로 루트 주소를 주려고 하면 문제가 생긴다. 이 또한 관련된 글에서 쓰고 여기다 링크를 달던가 하겠다.

#### 3. config.yml  
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

#### css, fonts, images
이 세 개의 폴더를 한 번에 설명할 수 있다. 내가 따로 추가한 데이터들이다.  

`main.css`는 기존에 있는 css들에 추가할 디자인 요소를 가지고 있다. 디자인 요소를 추가할 때 해당되는 css 파일을 찾아서 거기에 추가해도 되고, 여기에 추가해도 되는 것 같다. 웬만한 건 해당 css 파일에 추가하는 게 맞다고 생각돼서 그렇게 했다. 지금은 내가 추가한 폰트 경로와 설정, 자간, 단어간 공백 너비 설정만 넣어놨다. 

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

#### 루트
포스트 레이아웃을 제외하고 이 블로그의 모든 것의 구조가 모여있다. 프로필, breadcrumbs(보고 있는 글의 디렉토리 경로 표시), 댓글, footer, masthead, 좌측의 카테고리 사이드바, toc, 글 끝의 공유하기 버튼, 태그 정렬 버튼 등 정말 모든 구조다. 내가 뭔지 아는 것만 이 정도고, 저 목록을 보면 내가 언급하지 않은 파일도 많다. 즉 블로그 구조를 바꾸고 싶을 때 웬만하면 보게 되는 곳이다.  

그 중 내가 직접 썼던 몇 가지를 써보자면,  
`nav_list_main`은 카테고리별 글을 사이드바에 추가로 보여주기 위해 만든 파일이다. 말단의 카테고리 자체는 블로그 글들의 YAML Front Matter에 쓴 카테고리와 맞춰줘야 하지만, 그 카테고리를 부를 명칭이나 상위 카테고리의 명칭은 마음대로 정할 수 있다. 이 때 겉으로 보이는 명칭은 한국어로 해도 상관 없지만, <u>내부적인 카테고리는 반드시 영어로</u> 해주는 게 좋다. 그렇지 않으면 breadcrumb에서 한글이 깨져서 나오고, 여기저기서 카테고리를 똑같이 써줘야 하는데 실수가 나올 확률이 높다.  
`sidebar.html` 파일 마지막을 보면 YAML Front Matter의 `sidebar_main`이 true라면 이 파일을 include하는 코드가 있다. 당연한 소리지만 마지막에 넣지 않으면 bio보다 위에 나타난다. 블로그 관련한 파일들에서 코드 위치가 중요하지 않은 경우가 좀 있다보니 아무 생각 없이 이상한 곳에 넣었던 적이 있다.

`paginator` 같은 경우는 글들을 하나의 목록으로 보여주는 역할을 한다. 한 페이지에 몇 개의 글을 보여줄지 정하고, 넘어갈 경우 새 페이지를 만들어서 넘겨보며 만들 수 있게 해준다. 내 블로그의 최신 포스트 부분을 담당한다.  

블로그 대문에 가로로 사진을 길게 나오게 하려고 파일들을 타고 돌아다니다 알게 된 건데, page__가 붙어있는 파일들은 페이지 레이아웃과 연관있는 파일들이다. 예를 들어 `splash.html`같은 몇몇 레이아웃은 `page__hero.html`나 `page__hero_video.html`을 불러서 대문에 이미지나 영상을 제목과 함께 출력한다.

#### analytics, 댓글, 검색
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

#### head, footer
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
여기는 블로그 개별 글들의 레이아웃에 대한 html 파일들이 있다. 재밌는 건 이 레이아웃 파일들이 다 독립적이지 않다는 점이다. 예를 들어 `categories.html`의 레이아웃은 
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
 ┃ ┃ ┣ 📂breakpoint
 ┃ ┃ ┃ ┣ 📂parsers
 ┃ ┃ ┃ ┃ ┣ 📂double
 ┃ ┃ ┃ ┃ ┃ ┣ 📜_default-pair.scss
 ┃ ┃ ┃ ┃ ┃ ┣ 📜_default.scss
 ┃ ┃ ┃ ┃ ┃ ┗ 📜_double-string.scss
 ┃ ┃ ┃ ┃ ┣ 📂resolution
 ┃ ┃ ┃ ┃ ┃ ┗ 📜_resolution.scss
 ┃ ┃ ┃ ┃ ┣ 📂single
 ┃ ┃ ┃ ┃ ┃ ┗ 📜_default.scss
 ┃ ┃ ┃ ┃ ┣ 📂triple
 ┃ ┃ ┃ ┃ ┃ ┗ 📜_default.scss
 ┃ ┃ ┃ ┃ ┣ 📜_double.scss
 ┃ ┃ ┃ ┃ ┣ 📜_query.scss
 ┃ ┃ ┃ ┃ ┣ 📜_resolution.scss
 ┃ ┃ ┃ ┃ ┣ 📜_single.scss
 ┃ ┃ ┃ ┃ ┗ 📜_triple.scss
 ┃ ┃ ┃ ┣ 📜_breakpoint.scss
 ┃ ┃ ┃ ┣ 📜_context.scss
 ┃ ┃ ┃ ┣ 📜_helpers.scss
 ┃ ┃ ┃ ┣ 📜_legacy-settings.scss
 ┃ ┃ ┃ ┣ 📜_no-query.scss
 ┃ ┃ ┃ ┣ 📜_parsers.scss
 ┃ ┃ ┃ ┣ 📜_respond-to.scss
 ┃ ┃ ┃ ┗ 📜_settings.scss
 ┃ ┃ ┣ 📂magnific-popup
 ┃ ┃ ┃ ┣ 📜_magnific-popup.scss
 ┃ ┃ ┃ ┗ 📜_settings.scss
 ┃ ┃ ┗ 📂susy
 ┃ ┃ ┃ ┣ 📂plugins
 ┃ ┃ ┃ ┃ ┣ 📂svg-grid
 ┃ ┃ ┃ ┃ ┃ ┣ 📜_prefix.scss
 ┃ ┃ ┃ ┃ ┃ ┣ 📜_svg-api.scss
 ┃ ┃ ┃ ┃ ┃ ┣ 📜_svg-grid-math.scss
 ┃ ┃ ┃ ┃ ┃ ┣ 📜_svg-settings.scss
 ┃ ┃ ┃ ┃ ┃ ┣ 📜_svg-unprefix.scss
 ┃ ┃ ┃ ┃ ┃ ┗ 📜_svg-utilities.scss
 ┃ ┃ ┃ ┃ ┗ 📜_svg-grid.scss
 ┃ ┃ ┃ ┣ 📂susy
 ┃ ┃ ┃ ┃ ┣ 📜_api.scss
 ┃ ┃ ┃ ┃ ┣ 📜_normalize.scss
 ┃ ┃ ┃ ┃ ┣ 📜_parse.scss
 ┃ ┃ ┃ ┃ ┣ 📜_settings.scss
 ┃ ┃ ┃ ┃ ┣ 📜_su-math.scss
 ┃ ┃ ┃ ┃ ┣ 📜_su-validate.scss
 ┃ ┃ ┃ ┃ ┣ 📜_syntax-helpers.scss
 ┃ ┃ ┃ ┃ ┣ 📜_unprefix.scss
 ┃ ┃ ┃ ┃ ┗ 📜_utilities.scss
 ┃ ┃ ┃ ┣ 📜_su.scss
 ┃ ┃ ┃ ┣ 📜_susy-prefix.scss
 ┃ ┃ ┃ ┗ 📜_susy.scss
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
## _site
왜 있는지도 모르겠고, 남의 블로그 리포지토리엔 없다. 삭제해보고 별 일이 생기나 볼 예정이다.


