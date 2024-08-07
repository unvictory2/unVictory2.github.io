---
layout : single
title : "카테고리 추가하기"
excerpt : "nav_list_main, pages"
published: true

categories : 
    - Blog Writing

date : 2024-08-07
last modified : 2024-08-07
---

카테고리를 추가하는 건 굉장히 간단한 작업이지만, 블로그를 안 쓰다가 쓰게 되면 헷갈릴 수 있으니 적어둔다.

## nav_list_main
- `_includes\nav_list_main`을 찾는다.
- 계층 구조에 주의하며 새 카테고리를 추가한다.
  
```html
<!--대분류-->
        <span class="nav__sub-title">미래일경험</span> 
            <!--소분류-->
            <ul>
                {% for category in site.categories %}
                    {% if category[0] == "Miraework" %}
                        <li><a href="/categories/miraework" class="">미래일경험 교육({{ category[1].size }})</a></li>
                    {% endif %}
                {% endfor %}
            </ul>
            <ul>
                {% for category in site.categories %}
                    {% if category[0] == "Miraework Project" %}
                        <li><a href="/categories/miraework_project" class="">미래일경험 프로젝트({{ category[1].size }})</a></li>
                    {% endif %}
                {% endfor %}
            </ul>
```
위 코드에서 `미래일경험 ` 카테고리 안에 `미래일경험 교육`과 `미래일경험 프로젝트` 카테고리가 들어가 있다. 각각의 내부 카테고리 코드는 `miraework`와 `miraework_project`다. `<span>` 태그나 `<li>` 태그 사이에 쓴 한국어는 표시될 이름이고, `if category[0] ==` 뒤에 쓰인 내용은 내부적으로 사용할 카테고리명이다. 새로 추가할 카테고리에 맞게 몇몇 부분을 추가해서 복사 붙여넣기 하면 된다.

<br>

## categories
`pages\categories`에서 `category-카테고리명.md` 파일을 만든다. 내용물은 다음과 같다.
```
---
title: "미래일경험 교육"
layout: archive
permalink: categories/miraework
author_profile: true
sidebar_main: true
---
{% assign posts = site.categories.Miraework %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}
```
주의할 점은, `site.categories.Miraework` 부분에서 내부 카테고리 코드가 띄어쓰기를 포함할 경우 `site.categories.['Miraework Project']`처럼 대괄호와 따음표를 넣어줘야 한다.

<br>

## 개별 포스트
이제 카테고리 생성은 완료됐고, 글을 쓰면 된다. YAML의 `categories`에 내부 카테고리 코드를 적어주면 된다. 