---
layout : single
title : "toc가 중간부터 깨질 때 해결 방법"
excerpt : "계층적인 # 사용!"
published: true
toc : true
toc_sticky : true

categories : 
    - Blog Writing

date : 2024-07-29
last modified : 2024-07-29
---

글을 쓰고 toc를 켰을 때, 초반은 괜찮은데 중반부터 제목이 이상한 형식으로 나오는 경우가 있다.  
![broken_toc](https://github.com/user-attachments/assets/a717c27c-d28c-42ed-8622-caf2af0006ed)  
이런 식으로 `assets`부터 구조를 제대로 표현하지 못한다.  

문제는 #의 사용이다. #을 계층적으로 써야 toc를 만드는 코드가 오류나지 않는다. 위 사진의 경우에는 #이 2개인 `##루트` 안에 갑자기 #이 4개인 `#### 1. Favicon 관련 파일들` 등을 써서 오류가 난 거다. # 하나를 늘려서 `### 1. Favicon 관련 파일들` 등으로 수정하면 문제가 해결된다.   
![fixed_toc](https://github.com/user-attachments/assets/fb2d6950-bab4-475b-bd52-e4b13817563e)

[참고 링크](https://github.com/mmistakes/minimal-mistakes/issues/2892)
