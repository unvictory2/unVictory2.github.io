---
layout : single
title : "[BUG] 카메라가 캐릭터 따라가게 하기"
excerpt : "캐릭터를 따라 회전하는 카메라, 각도가 변하지 않는 카메라"
published: true

categories : 
    - Capstone

toc : true
toc_sticky : true

date : 2025-03-02
last modified : 2025-03-02
---

## 문제
### 설명
<video width="100%" height="auto" controls>
  <source src="{{ site.baseurl }}/assets/videos/카메라%20문제%20Gameview.mp4" type="video/mp4">
  Your browser does not support the video tag.
</video>

<video width="100%" height="auto" controls>
  <source src="{{ site.baseurl }}/assets/videos/카메라%20문제%20Sceneview.mp4" type="video/mp4">
  Your browser does not support the video tag.
</video>


영상을 넣는 게 처음이라 어떻게 올라가는지 모르겠다.  

캐릭터가 회전하면 카메라도 같이 회전한다. 그래서 캐릭터가 어느 방향으로 움직이든 직진하는 걸로 보인다.  

다른 3d 게임들을 보면 :  
1. 캐릭터 이동시 카메라는 뒤에 붙어있긴 하지만 돌진 않는다
2. 마우스 회전시 카메라도 돌고 캐릭터도 돈다. w 누르면서 카메라 회전시 움직이는 방향 바뀜.

1번에서부터 문제가 생긴 거다. 

### 현 코드
```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class FollowCam : MonoBehaviour
{
    // 따라가야 할 대상
    public Transform targetTr;
    private Transform camTr;

    // 따라가야 할 대상으로부터 얼마나 떨어져 있을지
    [Range(2.0f, 20.0f)] // 변수 입력 범위 제한, 인스펙터 뷰에 슬라이드바
    public float distance = 10.0f;

    // Y축으로 이동할 높이, 카메라 높이
    [Range(0.0f, 10.0f)]
    public float height = 2.0f;

    //카메라 LookAt의 offset 값
    public float targetOffset = 2.0f;

    // 카메라 반응 속도
    public float damping = 0.1f;

    private Vector3 velocity = Vector3.zero;

    // Start is called before the first frame update
    void Start()
    {
        // Main Camera 자신의 Transform 컴포넌트 추출
        camTr = GetComponent<Transform>();
    }

    // Update에서 이동 로직 한 후 실행하기 위해
    void LateUpdate()
    {
        // 추적해야 할 대상의 뒤쪽으로 distance만큼, 위로 height만큼 이동
        // 타깃의 위치 + (타겟의 뒤쪽 방향 * 떨어질 거리) + (y축 방향 * 높이)
        Vector3 pos = targetTr.position
            + (-targetTr.forward * distance)
            + (Vector3.up * height);

        // 구면 선형 보간 사용, 위치 부드럽게 바꾸기
        //camTr.position = Vector3.Slerp(camTr.position, pos, Time.deltaTime*damping);

        // SmoothDamp로 위치 보간
        camTr.position = Vector3.SmoothDamp(camTr.position, pos, ref velocity, damping);

        // 피벗 좌표 향해 회전
        camTr.LookAt(targetTr.position + (targetTr.up * targetOffset));
    }
}
```
<br>

## 해결
### 1. Vector 수정 
40행의 `-targetTr.forward`를 `Vector3.back`로 바꾼다. 모델의 로컬 좌표 기준으로 계산하던 걸 월드 좌표 기준으로 계산하게 바꾸는 거다. 이러면 더이상 카메라가 캐릭터와 함께 회전하지 않고, 일정 거리를 유지하되 계속 같은 방향에서 캐릭터를 관찰한다.  
(`Vector3.back`은 월드 좌표계에서 항상 -Z 방향을 가리키므로, 캐릭터의 회전과 무관하게 일정한 방향을 유지)
### 2. LookAt 삭제
50행의 `camTr.LookAt(targetTr.position + (targetTr.up * targetOffset));`를 삭제한다. 오류 해결을 위해서라기보단 더이상 필요하지 않기 때문이다. 이전엔 카메라가 계속 회전하기 때문에 회전 후 캐릭터를 바라봐야 해서 이 코드가 있었다. 그러나 이제 회전하지 않기 때문에 없어도 아무 지장이 없고, 있어도 아무 효과가 없다. 

이제 카메라가 회전하지 않고 멀리서 관찰만 한다!  
<video width="100%" height="auto" controls>
  <source src="{{ site.baseurl }}/assets/videos/카메라%20문제%20해결.mp4" type="video/mp4">
  Your browser does not support the video tag.
</video>

<br>

## 개념
### Vector3.forward와 transform.forward
이 둘은 다르다.  
`Vector3.forward`는 (0,0,1)과 같은 뜻이다. 언제 어떻게 써도 (0,0,1)이다.  

`transform.forward`는 <u>월드 공간을 기준으로 한 현재 게임 오브젝트의 앞 방향 벡터</u>이다. 예를 들어, 내 캐릭터가 보고 있는 앞 방향이 월드 기준으론 x축이라고 하자. 그럼 `transform.forward`는 (1,0,0)이라는 거다. 내 캐릭터의 앞 방향을 월드 기준으로 해석하면 x축이니까 말이다.  

이 상황에서 `Translate(transform.forward)` 하면 `Translate(1,0,0)`이란 뜻이고, 앞(오브젝트가 쳐다보는 방향)이 아니라 옆으로 움직인다. `Translate`는 오브젝트 기준으로 움직이니까 오브젝트 기준 x축 방향, 즉 옆으로 움직이라고 명령한 게 돼버린 거다.   

<u>값은 월드 기준으로 `transform.forward`해서 가져오고, 해석은 오브젝트 기준으로 `Translate` 해서 문제가 발생하는 거다.</u>

해결 방법은  
1. `Translate`로는 언제나 `Vector3.forward`로만 이동, 방향은 회전으로만 바꾸기.
```cs
void Update()
    {
        Vector3 dir = Vector3.zero;
        if (Input.GetKey(KeyCode.UpArrow))
        {
            transform.Translate(Vector3.forward * 2.0f * Time.deltaTime);
            dir += Vector3.forward;
        }
        if (Input.GetKey(KeyCode.DownArrow))
        {
            transform.Translate(Vector3.forward * 2.0f * Time.deltaTime);
            dir += Vector3.back;
        }
        if (Input.GetKey(KeyCode.RightArrow))
        {
            transform.Translate(Vector3.forward * 2.0f * Time.deltaTime);
            dir += Vector3.right;
        }
        if (Input.GetKey(KeyCode.LeftArrow))
        {
            transform.Translate(Vector3.forward * 2.0f * Time.deltaTime);
            dir += Vector3.left;
        }
        dir = dir.normalized;
        if (dir.magnitude > 0.5f)
        {
            transform.LookAt(transform.position + dir);
        }
    }
```

2. `Space.World`를 `Translate` 함수의 인자로 넣어서 월드 기준으로 해석하게 하기.  
   이렇게 하면 Translate 함수가 월드 좌표계를 기준으로 이동을 계산하므로, 캐릭터의 로컬 좌표계와 무관하게 일관된 방향으로 이동한다.
```cs
void Update()
    {
        Vector3 dir = Vector3.zero;
        if (Input.GetKey(KeyCode.UpArrow))
        {
            transform.Translate(Vector3.forward * 2.0f * Time.deltaTime, Space.World);
            dir += Vector3.forward;
        }
        if (Input.GetKey(KeyCode.DownArrow))
        {
            transform.Translate(Vector3.back * 2.0f * Time.deltaTime, Space.World);
            dir += Vector3.back;
        }
        if (Input.GetKey(KeyCode.RightArrow))
        {
            transform.Translate(Vector3.right * 2.0f * Time.deltaTime, Space.World);
            dir += Vector3.right;
        }
        if (Input.GetKey(KeyCode.LeftArrow))
        {
            transform.Translate(Vector3.left * 2.0f * Time.deltaTime, Space.World);
            dir += Vector3.left;
        }
        dir = dir.normalized;
        if (dir.magnitude > 0.5f)
        {
            transform.LookAt(transform.position + dir);
        }
    }
```