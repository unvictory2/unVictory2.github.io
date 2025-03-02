---
layout : single
title : "Rigged Humanoid 캐릭터에 외부 애니메이션 적용하기"
excerpt : "애니메이션 미포함 에셋을 받은 경우"
published: true

categories : 
    - Capstone

toc : true
toc_sticky : true

date : 2025-03-03
last modified : 2025-03-03
---

## 개요 
내가 구매한 캐릭터 에셋엔 애니메이션이 포함돼 있지 않다. 하지만 "Has Attached Skeletons"라고 써있었기 때문에 따로 애니메이션을 적용시킬 수 있을 거라고 생각했고 오늘 하려는 작업이 그거다.  

저렴한 에셋들은 애니메이션과 함께 오는 경우가 거의 없거나 같이 오는 종류수가 적다. 대신 대부분 Rigged(Attached Skeletons와 동일 의미라고 추정한다)인 상태긴 하다. 

## 과정
### 1. 구조 파악
일단 내가 쓰는 에셋의 모델을 찾는다. `Prefab`이 있고 `Model`이 따로 있기 때문에 주의해야 한다. 

![Image](https://github.com/user-attachments/assets/6ab95486-4f3c-459d-8984-3b673298ef18)  

내 경우 보면 왼쪽이 `Model`이고, 오른쪽이 `Prefab`이다.  
모델에 얼굴(이것도 따로 모델 폴더에 같이 있음)을 붙인 게 `Prefab`. `Hierachy`에 넣고 스크립트를 주며 쓰는 건 프리팹이지만, 오늘 할 초반부의 작업은 모델로 진행한다. 애초에 Inspector뷰에서 Rig 탭을 찾아야 되는데 이게 모델한테만 있다. 

### 2. Humanoid 적용
![Image](https://github.com/user-attachments/assets/96f371a0-6bfc-42c4-bedf-0596d408f138)  

내가 쓰는 프리팹의 모델을 찾은 후, 해당 모델을 클릭하고 Inspector뷰를 본다. `Rig` 탭에서 `Animation Type`이 `Humanoid`로 돼있는지 확인한다. 난 `Generic`으로 돼있어서 바꿨다. 이후 밑의 `Apply`를 누른다.  

### 3. Avatar 확인
![Image](https://github.com/user-attachments/assets/7f905969-92f2-4592-a278-65fd0c460de1)  

잘 됐다면 아까는 비활성화 돼있었던 `Configure`라는 버튼이 활성화된다. 이 버튼을 클릭해본다. 

![Image](https://github.com/user-attachments/assets/84823a3c-3bae-4e9f-8bbb-78fd3fca45de)  

Inspector뷰가 `Avatar Configuration`이라는 화면으로 바뀐다. Avatar 에셋은 3D 모델의 본 구조의 매핑 정보를 저장하고 있으며, 본 구조가 동일하다면 다른 모델에서 사용한 Avatar 에셋을 재사용할 수 있다.  
실선으로 된 동그라미들은 필수, 점선은 선택이라고 한다. `Body`, `Head`, `Hand` 등을 확인해보고, 위 탭 중 `Muscles&Settings`에서 여러 자세도 취해보자. Scene뷰에서 각종 자세에 따라 바뀌는 모습을 확인해볼 수 있다. 매핑이 잘 됐는지, 자세들은 이상하지 않은지 확인해보면 된다.  

![Image](https://github.com/user-attachments/assets/a9b7cb87-0315-4bbb-94c6-066d4d468c55)  

확인했으면 디렉토리에 있는 모델에 아까는 없던 하늘색 사람 모양의 `Avatar`가 생성된 것도 확인한다.

### 4. Animator Controller  
![Image](https://github.com/user-attachments/assets/927fba60-49ce-4f73-a0f2-b174cc22dbc8)  

이제 다음 작업을 해보자. `Animator Controller`를 하나 만들자. 나는 `Assets/Animation/Controllers/`의 경로에 만들었다.  

![Image](https://github.com/user-attachments/assets/db0f3210-4487-4c34-8bdb-78d945eb0c11)  

생성된 컨트롤러를 선택하고 Inspector뷰에서 `Open` 버튼을 누른다. Scene뷰 대신 Animator뷰가 열릴 거다. 여기서 상황에 따른 각종 애니메이션을 설계할 수 있다.  

![Image](https://github.com/user-attachments/assets/bd107572-4e27-405a-bc77-6b01484673f2)  

컨트롤러에 애니메이션을 드래그해서 넣어보자. 적용할 애니메이션은 미리 에셋 스토어에서 무료로 받아놨다. 컨트롤러에서 주황색으로 새 애니메이션이 추가된 걸 확인할 수 있다.  

### 5. Hierachy의 Prefab에 적용  

![Image](https://github.com/user-attachments/assets/b245f986-9090-4dea-b9a4-bc4e4a4083a5)  

지금까지 작업하던 모델의 프리팹이 이미 `Hierachy`에 없다면 넣고, 있다면 선택한다. Inspector뷰에서 `Add Component`를 누른 뒤 `Animator`를 추가한다.  

![Image](https://github.com/user-attachments/assets/4142ad01-47a7-4572-9cfa-a29bfc4d5c14)  

![Image](https://github.com/user-attachments/assets/41b00a98-174d-4dd6-8a25-e7810b978423)  

직전에 생성한 컨트롤러와 아까 생성된 걸 확인한 Avatar를 드래그해서 Animator에 적용한다.  

### 6. 성공!  

![Image](https://github.com/user-attachments/assets/4b08eeb5-4ef3-402d-a8a7-c717d97c87e3)  

Play 해보자. Play 전엔 T자로 서있던 캐릭터가 Idle 모션을 하고 있다!  
재사용 방법들이나 Avatar가 정확히 뭔지, Animation 종류엔 뭐가 있는지 같은 개념도 설명하면 좋겠으나 오늘은 피곤해서 다음에 생각해봐야겠다.
