---
layout : single
title : "글에 비디오 넣기"
excerpt : "리포에 첨부 후 html 태그 사용"
published: true

categories : 
    - Blog Writing


date : 2025-03-02
last modified : 2025-03-02
---
## 후보
1. 유튜브에 업로드 하고 유튜브 영상으로 삽입
2. 이미지처럼 github issues 사용해서 삽입 > 그냥 쌩링크고 링크 눌러야 새 탭에서 영상 재생
3. 내 리포지토리에 첨부하고 html 태그로 재생하기
   
2는 재생이 번거롭고 유튜브에 올리긴 귀찮아서 3번으로 하기로 했다.

<br>

## 방법
![Image](https://github.com/user-attachments/assets/de1510b6-ef21-484d-b286-0eac7d61097f)

루트에 있는 `asset` 폴더 내부에 `videos` 폴더 만들고 거기다 넣고싶은 영상을 집어넣는다. 본인이 기억만 하면 아무 루트나 상관 없음.

글 쓸 때 이런 식으로 사용한다.
```markdown
<video width="100%" height="auto" controls>
  <source src="{{ site.baseurl }}/assets/videos/카메라%20문제%20Gameview.mp4" type="video/mp4">
  Your browser does not support the video tag.
</video>
```
`{{ site.baseurl }}`은 변수로 쓰는 거라 바꿀 필요 없다. 뒤에 있는 경로는 파일 경로를 써주면 되는 거고. `%20`은 url에서 띄어쓰기 문제 생기지 말라고 넣어준 거다. 다음부턴 파일명에 띄어쓰기 안 넣는 것도 하나의 방법일듯.  

글에선 이렇게 보인다.  

![Image](https://github.com/user-attachments/assets/8bd90c82-e361-4a91-8c6e-a4a1774ffea5)  

이 방법의 문제점은 영상을 많이 넣을수록 블로그 리포 크기가 늘어날 거라는 점? 근데 위에 3개는 1MB라 아직 별 상관 없을 거 같다. 