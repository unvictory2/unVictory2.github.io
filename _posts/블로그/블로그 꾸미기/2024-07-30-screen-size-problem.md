---
layout : single
title : "화면 크기 미고려 문제"
excerpt : "블로그가 모든 화면에서 보기 적합하지 않다"
published: true

categories : 
    - Blog Decoration

toc : true
toc_sticky : true

date : 2024-07-30
last modified : 2024-07-30
---

## 문제점
언제나 집에 있는 큰 모니터로 작업하다보니 블로그 내부 기준으로 `x-large`인 경우만 고려하여 프로그래밍하고 말았다. 그 결과 모바일에서는 운좋게도 괜찮게 보이나, 노트북 화면에선 문제가 꽤 있다. 웹 화면을 80%로 축소하면 내가 원래 보던 화면이 잘 뜨지만 사이트 완성도에 문제가 있음은 분명하다. 시간이 나면 고쳐야 할 부분이다. 크기별로 화면을 비교해보자. 

## 화면 크기별 외형
내부에서 화면 크기를 `small`, `medium`, `medium-wide`, `large`, `x-large`로 나누는 걸로 아는데 화면의 종류는 3개다. <u>어느 크기에서 어떤 화면이 나오는 건지 파악하고 해당 크기에서 sidebar toc 본문너비를 조정하면 해결할 수 있을 거라고 보인다.</u>

### 큰 화면  
<img width="960" alt="large" src="https://github.com/user-attachments/assets/3967d055-e8d2-4157-9f4b-db576683111d">

### <u>중간 화면</u>
<img width="960" alt="broekn" src="https://github.com/user-attachments/assets/bdfa5405-6a33-4bdd-b7eb-e92371355b73">


### 작은 화면(모바일)
![befoe](https://github.com/user-attachments/assets/969bc6e8-cce7-49f8-b610-a29f1f395c42) 

토글 메뉴 누를 경우 :   
![Screenshot_20240730_154430_Chrome](https://github.com/user-attachments/assets/2e1408b2-eb58-4b27-bea6-8bb199e4f0f3)
