---
layout : single
title : "글에 이미지 넣기"
excerpt : "Issues 이용"
published: true

categories : 
    - Blog

date : 2024-07-11
last modified : 2024-07-11

# header:
#   teaser: /assets/images/random_image1.jpg
---

글에 이미지 넣는 게 꽤 어려웠다. 로컬 환경에서 이미지를 넣어주면 커밋했을 때 이미지가 깨졌다. 사실 당연한 일이다. 내가 커밋한 건 md 파일이고 이미지는 아니니까... 그래서 직접 깃허브에 가서 이미지를 올리곤 했었다.  

그런데 찾아보니 쉽게 올리는 방법이 있는 거 같아서 실험겸 해봐야겠다.

깃허브의 아무 repository에나 가서 issues에서 documentation을 누른다.   
[내 블로그 repository의 issues 주소](https://github.com/unvictory2/unVictory2.github.io/issues/new/choose)

내 컴퓨터에 있는 이미지를 복사하고, 여기다가 붙여넣기 한다.
<br/><br/>

![alt text](image.png)
![코드](https://github.com/unvictory2/unVictory2.github.io/assets/117062169/cb5396ec-cde3-4b1b-b7a5-217d2778a7bf)

그럼 이런 식으로 업로드 중이라고 뜨더니, 텍스트 형식으로 바뀐다. 사진에서는 이미지를 총 4장 올린 거다. 각 이미지는 앞의 `!`로 구분할 수 있다.  

이걸 복사해와서 md 파일에다가 붙여넣기 하면 된다.

내가 궁금한 점은 이렇게 해서 한 번 커밋하면 로컬에 있는 이미지를 삭제해도 되는지다. 이 글에 있는 이미지로 먼저 실험해봐야겠다.

----
일단 지금 이 글의 이미지 주소는 `![alt text](image.png)`라고 떴었는데,  
`![제목 없음213](https://github.com/unVictory2/unVictory2.github.io/assets/117062169/473dc841-256a-4ce3-84ba-eec5326bf462)`같은 형식이 정상적인 형식이다. 안 됐다면 다시 issues로 가서 해보는 게 좋겠다. 참고로 issues는 저장할 필요 없다.  

깨져서 나온 이미지 밑에 새로 만든 이미지 주소를 추가했다. 그리고 아직 커밋 전이지만 이미지 원본은 휴지통으로 보냈다.  
결론은 성공이다! 로컬에서 삭제해도 상관없다.  

<hr>
보고 따라한 출처 :  
 
https://ansohxxn.github.io/blog/insert-image/