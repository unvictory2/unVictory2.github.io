---
layout : single
title : "[BUG] GameView 화면이 너무 낮다"
excerpt : "Scale 1로 하고 카메라 가까이 붙이자"
published: true

categories : 
    - Capstone

date : 2025-03-02
last modified : 2025-03-02
---
## 문제 
이전 글 영상에서 볼 수 있는데, GameView 화면이 이상하게 저화질이다.  
SceneView에선 멀쩡한데 GameView만 픽셀이 보이는 수준이다!

![Image](https://github.com/user-attachments/assets/1ad6be7d-af45-41c3-870c-d4b2bddce835)

## 해결
위 사진에 있는 GameView의 `Scale`이 5배로 돼있어서 생기는 문제였다. 1배로 바꿔주면 정상적인 화질로 돌아온다. 대신 캐릭터가 너무 작아질 거다. 

그건 카메라 거리 설정도 Scale이 5배인 거 기준으로 해놨기 때문이다. 이것도 고쳐줘야 한다. `Distance`, `Height`, `Target Offset`을 손보면 된다.  

![Image](https://github.com/user-attachments/assets/6f7cdd56-10a4-4218-96c3-c8869dabe11d)

위처럼 엄청 높은 값으로 돼있는 `Distance`를 먼저 가깝게 바꾸고, 나머지는 화면 보면서 바꾸면 끝.

![Image](https://github.com/user-attachments/assets/453ba20b-0a86-4e72-8db3-f636adc14968)  

깨끗해졌다.  

![Image](https://github.com/user-attachments/assets/53fd8229-d01a-4f33-bed9-bfe0f825a0be)