---
layout : single
title : "블로그 공부 방법"
excerpt : "검사, ctrl shift f, 다른 블로그 뜯어보기, 글 읽기, 챗gpt"
published: true

categories : 
    - Blog Decoration

toc : true
toc_sticky : true

date : 2024-08-10
last modified : 2024-08-11
---

## 서론  
아마 문제 해결을 제외하고는 이게 블로그 꾸미기 카테고리의 마지막 글이 될 거다. 지금까지 내 블로그를 꾸민 과정을 정리했다. 이제 블로그를 추가로 꾸밀 때 어떻게 관련된 자료를 찾을지 써두겠다. 가장 주체적인 방법 순으로 검사 도구 사용, 다른 블로그 뜯어보기, 구글 검색, 챗gpt 순이다.

## 검사 도구 사용
블로그 초반엔 하는 방법도 몰랐고 어려워 보여서 시도하지 않은 방법이다. 사실 아직도 100퍼센트 활용하지는 못하지만 매우 유용한 방법인 건 확실하다. 블로그에 들어가서 크롬 기준 `f12`를 누르면 검사창이 뜬다. 여기서 여러가지를 할 수 있는데 우리는 <u>바꾸고 싶은 요소에 영향을 주는 html 코드나 css 코드를 찾는데 목적</u>을 둔다. 왜냐하면 블로그를 추가로 꾸밀 때 바꾸게 되는 건 주로 그 두가지이기 때문이다.  

![검사](https://github.com/user-attachments/assets/a9bf57af-c7ce-4f1f-9dce-4f339c2778bf)  

`ctrl + shift + c`를 누르거나 검사창 좌상단의 버튼을 눌러 `검사할 페이지 요소 선택`을 활성화한다. 이후 마우스를 페이지의 요소에 올리면 검사창에 그 요소와 관련있는 html 코드가 어디 있는지 뜬다. 

![findelement](https://github.com/user-attachments/assets/de1ebeb9-d929-462c-b9b4-8236b7d97b85)

위 이미지의 경우 블로그 프로필 사진에 마우스를 올렸더니, 관련된 코드인 
`<img src="/assets/images/profile_image.jpg" alt="unVictory2" itemprop="image" class="u-photo">`가 검사창에 나오는 모습이다. 밑의 파란색 박스는 해당 코드의 경로다. 근데 이게 들어있는 파일명은 못 찾았다... 경로를 잘 읽어보면 알 것도 같은데 그냥 VSC에서 `ctrl + shift + f`로 코드 내용을 검색했다. 경로를 읽고 검사창 내에서 파일명을 알아내는 게 똑똑한 방법인데 아쉽다.

![css](https://github.com/user-attachments/assets/84536167-4765-4fbf-997d-8b47e6c2604d)

아까 마우스를 올려두기만 했던 걸 아예 클릭하면 css 코드도 볼 수 있다. 이 경우는 우상단에 어느 파일에 있는 코드인지 뜬다. 체크박스로 적용을 풀어볼 수도 있고, 당연히 저장은 안 되지만 검사창 내에서 수정해 볼수도 있다.  

### VSC에서 코드 찾기
위에서 언급했던 VSC에서 검색하는 방법을 대해 알아보자. 위에서 우리가 알아냈던 코드 `<img src="/assets/images/profile_image.jpg" alt="unVictory2" itemprop="image" class="u-photo">`가 어디에 있는지 알아보자.  

VSC에서 `ctrl + shift + f`를 누른다. 

![search](https://github.com/user-attachments/assets/228d90b9-61a8-4e00-9481-5a0a725cd0aa)

그럼 우측에 이런 식으로 검색바가 뜬다. 그냥 `ctrl + f`와 다른 점은 한 파일 내에서만 찾는 게 아니라 전체 폴더에서 검색한다는 거다. 여기다가 아까 찾아낸 코드의 일부를 검색해보자.  

![no_result](https://github.com/user-attachments/assets/f71d5052-511b-41ed-909a-50bbb9ece890)  

지금 쓰고 있는 블로그 글밖에 안 뜬다! 이 이유를 깊게 생각해 본 적이 없는데 이번에 한 번 알아보자. 코드를 좀 줄여서 다음과 같이 검색해본다.

![route](https://github.com/user-attachments/assets/abe9b203-9b0a-4b92-8986-8265b4541ac3)

`_config.yml`이 뜬다. 한 번 어느 부분에서 나온 건지 볼까? 검색 결과를 클릭하면 볼 수 있다.  

![config](https://github.com/user-attachments/assets/0eb1067e-599d-428d-a968-72a4c4086da0) 

사이트 설정하는 부분 중 하나에서 나온 거였다. 그럼 원본 html 파일에는 `경로 = "파일경로"`라고 써있고, "파일경로"는 `_config.yml`에서 가져오는 형식이 아닐까라고 추측할 수 있다. 그래서 실행된 코드, 즉 검사창에 뜬 코드에서는 경로가 나오지만 실행 전에 검색해보면 경로까지 포함한 코드는 없는 거다.  
그럼 검색 문구를 바꿔보자. 기존의 `<img src="/assets/images/profile_image.jpg" alt="unVictory2" itemprop="image" class="u-photo">`에서 다른 파일에서 가져오지 못할 거 같은 부분인 `class="u-photo"` 부분을 검색해보자.  

![class](https://github.com/user-attachments/assets/b444cfbf-9568-41b0-bed2-130dbeea4abc) 

뭔가 찾았다! 눌러보자.  

![htmlfile](https://github.com/user-attachments/assets/e21a14ff-ac2f-4936-a14e-3910b4dfa478) 

진짜 `url`과 `alt`는 다른 데서 가져오고, `itemprop`과 `class`만 본 파일에서 정의하는 형식이었다! 문제를 해결하니 뿌듯하다.  

어쨌든 검사창에서 html 파일의 경로를 못 알아낸다면 이런 검색 과정을 통해 알아낼 수 있다.

## 나머지 방법들
나머지 방법들은 별로 설명할 게 없다. 
 - 대부분의 깃허브 블로그들은 깃허브에 소스코드가 공개돼있기 때문에 남의 코드와 내 코드, 혹은 파일 구조를 비교해보면 문제를 해결할 수도 있다. 
 - 남이 블로그를 꾸민 게 이뻐보여서 따라하고 싶어도 유용한 방법이다. 물론 남의 블로그에 검사창을 띄워도 된다.
 - 구글 검색과 챗gpt는 나오면 좋고, 아님 다른 방법을 쓰면 된다. 특히 챗gpt는 이상한 소리를 자주 해서 큰 도움은 안 됐다. css 수정 관련해서 물어보면 맨날 `main.scss` 만들어서 거기 넣으라는 소리만 한다.
 - 어디서나 그렇겠지만 편함과 배움은 반비례한다. 검색 도구를 쓰는 게 시간은 오래 걸리지만 블로그 구조에 대한 지식이나 뿌듯함 면에선 압도적이다.