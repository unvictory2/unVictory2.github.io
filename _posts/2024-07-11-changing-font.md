---
layout : single
title : "폰트 바꾸기"
excerpt : 
published: true

categories : 
    - 블로그
  
date : 2024-07-11
last modified : 2024-07-11
---

폰트를 바꿔보자.  

![1 글씨체 찾기](https://github.com/unvictory2/unVictory2.github.io/assets/117062169/3a5607ee-b92e-4f18-bf88-0233981f0c60)

[눈누](https://noonnu.cc/index#google_vignette)에 가서 마음에 드는 폰트를 찾는다. 우측에 있는 "웹폰트로 사용"에 있는 코드를 복사한다.
<br/><br/>

![2 main scss에 추가](https://github.com/unvictory2/unVictory2.github.io/assets/117062169/7301e628-959e-44de-981d-fae44767b8a7)

블로그 디렉토리를 열어서, `assets/css/main.scss`로 이동한다. 복사한 텍스트를 맨 밑에 추가해준다. 이 때 font-family에 해당하는 폰트명은 복사해둔다.  

참고로 맨 위에 있는 주석같은 말들은 건드리면 안 된다. 주석처리도 안 돼있고 빨간 줄이 그어졌길래 주석처리 해줬더니 main.css가 없다고 블로그가 망가진다. 내가 건드린 건 main.scss인데 그게 영향이 있나보다...
<br/><br/>

![3 _variables에 추가](https://github.com/unvictory2/unVictory2.github.io/assets/117062169/e9115757-c23c-431d-995b-9e09f4f490f4)  

이제 `_sass/minimal_mistakes/_variables.scss`로 가서 아까 복붙한 폰트명을 `$sans-serif`제일 앞에 넣어주면 된다.