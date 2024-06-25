---
layout : single
title : "포스트 레이아웃 문제"
excerpt : "layout : post 미적용"
published: true

categories : 
    - 블로그
  
toc : true
toc_sticky : true

date : 2024-06-24
last modified : 2024-06-25
---

글에 layout : post를 넣으면 
![제목 없음213](https://github.com/unVictory2/unVictory2.github.io/assets/117062169/473dc841-256a-4ce3-84ba-eec5326bf462)
이렇게 흰 html 레이아웃이 되고,

해당 문구를 지우면
![화면 캡처 2024-06-24 233508](https://github.com/unVictory2/unVictory2.github.io/assets/117062169/42ddeb27-d8cd-492e-bf4f-b9214bc6d3d6)
이렇게 블로그 테마가 적용된 상태가 된다.

보통 이 반대여야 되는 거 아닌가...? 해결 필요.
뭐가 문젠지 알기 위해 layout : posts를 single 등 다른 걸로 바꿔 보자.

===

2024.6.25
문제 해결. 일단 깃허브에서 _posts와 같은 디렉토리에 있는 _layouts 폴더에 가보면 사용할 수 있는 모든 레이아웃 후보가 있다. 
![image](https://github.com/unVictory2/unVictory2.github.io/assets/117062169/52d632e1-f358-4348-8b4f-7754dfe291a8)
이 중 "post"는 없기 때문에 인식하지 못해서 흰 배경으로 나온 듯. 아예 입력 안 하면 기본값인 무언가로 나갔을 거고. 아마 single 같은 게 아니였을지?

근데 이 많은 레이아웃들이 다 뭔지 궁금해서 gpt한테 물어봤다. 예시 사진은 무슨 차인지 궁금해서 내가 하나하나 layout을 바꿔보며 찍었음.
<br>

GitHub 블로그의 Minimal Mistakes 테마에서 제공하는 다양한 레이아웃에 대해 설명해 드릴게요.

### Archive Layouts
1. **archive-taxonomy**:
   - 특정 분류나 태그에 따라 아카이브를 구성합니다. 예를 들어, 특정 카테고리나 태그에 속한 모든 글을 한 페이지에 나열할 때 사용됩니다.
![archive-taxonomy](https://github.com/unVictory2/unVictory2.github.io/assets/117062169/8741b095-9176-4b26-ab43-05d123feac54)
글 내용이 없다...

2. **archive**:
   - 전체 아카이브 페이지를 구성합니다. 연도별, 월별로 글을 분류해서 보여줄 때 주로 사용됩니다.
  ![archive](https://github.com/unVictory2/unVictory2.github.io/assets/117062169/eb61194b-9414-476b-9d73-cdd35f73efcc)
아무것도 없다...

### Category and Tag Layouts
3. **categories**:
   - 블로그의 모든 카테고리를 나열하고, 각 카테고리마다 포함된 글의 개수를 표시합니다. 보통 사이드바에 위치하며, 사용자가 특정 카테고리를 선택하면 해당 카테고리에 속한 글들을 보여줍니다.
![categories](https://github.com/unVictory2/unVictory2.github.io/assets/117062169/3c96dcfe-d463-4e13-9a42-63001ac31743)
사이드에 두면 좋겠다.

4. **category**:
   - 특정 카테고리에 속한 글들을 모아서 보여줍니다. 예를 들어, 'JavaScript' 카테고리 페이지라면 JavaScript 관련 글들만 나열됩니다.
![category](https://github.com/unVictory2/unVictory2.github.io/assets/117062169/59bb59c7-5f67-43c9-9124-a653f684197f)
특정 카테고리를 설정해주지 않아서 이런 듯.

5. **tag**:
   - 특정 태그에 속한 글들을 모아서 보여줍니다. 예를 들어, 'Web Development' 태그 페이지라면 해당 태그가 붙은 글들만 나열됩니다.
![tag](https://github.com/unVictory2/unVictory2.github.io/assets/117062169/3e8beb6e-d7bf-4301-afb1-59730bc73f16)
마찬가지로 특정 태그 미설정해서 그런듯.

6. **tags**:
   - 블로그의 모든 태그를 나열하고, 각 태그마다 포함된 글의 개수를 표시합니다. 사용자가 특정 태그를 클릭하면 해당 태그에 속한 글들을 보여줍니다.
![tags](https://github.com/unVictory2/unVictory2.github.io/assets/117062169/df02fa9c-b7b2-43ec-b403-b0bb4b26ac6e)
내가 태그를 달아서 작성한 글이 없어서 이런듯.

### Collection Layout
7. **collection**:
   - 블로그의 특정 컬렉션을 나열합니다. 예를 들어, 특정 주제나 유형의 글들을 모아서 한 컬렉션으로 구성할 수 있습니다.
![collection](https://github.com/unVictory2/unVictory2.github.io/assets/117062169/8611bc3b-9cd8-4d7e-bb19-e79544e2a6d7)
특정 주제 설정을 안 해서 모든 글을 보여주는 듯?

### Page Layouts
8. **default**:
   - 기본 페이지 레이아웃입니다. 다른 레이아웃의 기본이 되며, 페이지 내용이나 포스트 내용을 단순히 나열할 때 사용됩니다.
![default](https://github.com/unVictory2/unVictory2.github.io/assets/117062169/4358cac2-e8d5-438a-a511-888d3107fc3b)
그냥 글만 보여주는 레이아웃인데, 이걸 글 보여주는 다른 애들이 자기 내부에 포함시키는 걸로 보임. </br>
예를 들어 single 레이아웃은 default 레이아웃 + 댓글 더보기 등등 몇가지 기능을 추가한 레이아웃인 식으로.

9. **home**:
   - 블로그의 홈 페이지 레이아웃입니다. 보통 최신 포스트 목록이나 소개 글 등이 포함됩니다.
![home](https://github.com/unVictory2/unVictory2.github.io/assets/117062169/80b75757-5004-432b-a1dd-9a211d014ab0)
설명과 같은듯.

10. **posts**:
    - 블로그 포스트 목록을 나열합니다. 최신 포스트나 특정 조건에 맞는 포스트 목록을 보여줄 때 사용됩니다.
![posts](https://github.com/unVictory2/unVictory2.github.io/assets/117062169/15951efc-dd95-49a4-a4a0-16e588b05247)
어제 쓴 몇 개의 글들이 2023으로 돼있는 이유는 내가 실수로 날짜를 2023이라고 써서 그럼. 아무튼 이건 글 쓸때 쓰는 양식이 아니였다...

11. **single**:
    - 개별 포스트 페이지의 레이아웃입니다. 사용자가 블로그 포스트를 클릭하면 해당 포스트의 내용을 보여줍니다.
![single](https://github.com/unVictory2/unVictory2.github.io/assets/117062169/81b642c9-30c6-4d6d-b012-9b8141da2483)
글 쓸 때 사용하기 가장 좋은 레이아웃으로 보임. 이 글도 single 레이아웃으로 쓸 거임.

12. **splash**:
    - 스플래시 페이지 레이아웃입니다. 눈에 띄는 이미지와 함께 간단한 텍스트를 보여줄 때 사용됩니다. 보통 중요한 알림이나 소개 페이지에 사용됩니다.
![splash](https://github.com/unVictory2/unVictory2.github.io/assets/117062169/f9e8877e-90a3-4623-b086-4ab292014dcc)
이미지가 없어서 글만 심심하게 보이나보다.

13. **search**:
    - 검색 결과 페이지의 레이아웃입니다. 사용자가 검색한 키워드와 관련된 포스트 목록을 보여줍니다.
![search](https://github.com/unVictory2/unVictory2.github.io/assets/117062169/24a8b72f-ed4f-4f6e-a05e-b7fd84070ee5)
실제로 내가 뭘 검색하면 결과를 보여줬었음

### Utility Layouts
14. **compress**:
    - 페이지의 용량을 줄이는 압축 레이아웃입니다. 주로 이미지나 리소스 파일을 압축해서 보여줄 때 사용됩니다.
![compress](https://github.com/unVictory2/unVictory2.github.io/assets/117062169/b02fae01-1e00-4bb4-aa0b-7fa752291eeb)
기능을 하는 애지 보이기 위한 애가 아니라 아무 꾸밈이 없는듯.

각 레이아웃은 특정한 용도와 구조를 가지고 있어 블로그의 다양한 페이지를 구성하는 데 유용합니다. 블로그의 목적과 스타일에 맞게 적절한 레이아웃을 선택하여 사용할 수 있습니다.
