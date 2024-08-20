---
layout : single
title : "[미래일경험 프로젝트] 문제 해결 - Calendar View 패키지 수정하기"
excerpt : "onCellTap 함수가 좌표도 전달하게 수정"
published : true

categories : 
    - Miraework Project

toc : true
toc-sticky: true

date : 2024-08-20
last modified : 2024-08-20
---
그래서 결국 어떻게 했다는 건데? 라는 의문이 든다면 제일 밑의 최종 해결을 보면 된다.
## 문제사항과 고민 과정
내가 개발을 맡은 데스크톱 위젯은 이렇게 생겼다.   

![figma_calendar](https://github.com/user-attachments/assets/0058f358-126e-4de2-92cf-3be512c5dfbc)  

날짜를 터치하면 그 날의 일정이 메뉴처럼 뜨는 형식이다. 메뉴 자체는 `showMenu` 위젯으로 구현하기로 했다. 터치 인식은 내가 쓰게 된 [`calendar view` 패키지](https://pub.dev/packages/calendar_view)의 `onCellTap` 함수를 사용할 거다. 참고로 `onCellTap`의 설명은 다음과 같다.
```dart
{void Function(List<CalendarEventData<Object?>>, DateTime)? onCellTap}
Type: void Function(List<CalendarEventData<Object?>>, DateTime)?

// This function will be called when user taps on month view cell.
```
이걸 가지고 
```dart
 body: MonthView(
        useAvailableVerticalSpace: true, // Avoid clipping
        onCellTap: (events, date) {
          _showDailySchedule(context, events, date);
        },
      ),
```
이런 식으로 달력을 보여주고 날짜를 탭하면 `_showDailySchedule` 함수를 부르게 했다. 해당 함수는 
```dart
void _showDailySchedule(BuildContext context,
      List<CalendarEventData<Object?>> events, DateTime date) {
    final RenderBox overlay =
        Overlay.of(context).context.findRenderObject() as RenderBox;

    String weekdayDate = _formatDate(date);

    showMenu(
      context: context,
      position: RelativeRect.fromLTRB(300, 300, 300, 300),
      items: [
```
이런 식이다. 인자로 위젯 트리에서의 위치와 해당 날짜의 이벤트를 배열로 받고, 날짜 정보도 받는다. 함수의 첫 줄인 `overlay`관련된 내용은 내가 보여줄 메뉴를 화면 밖이나 다른 위젯에 가리지 않게 배치하기 위해 현재 오버레이 정보를 가져오는 내용이다. 그 다음엔 현재 날짜를 내가 원하는 형식으로 바꿔서 가져온다.  
지금 내 문제와 관련이 있는 건 그 밑의 `showMenu`인데 <u>여기서 `position`을 어떻게 줘야 될지 모르겠다는 거다.</u> 내가 터치한 좌표를 `onCellTap`에서 이 함수를 부를 때 인자로 줘야 될 거 같다. 근데 내가 터치한 좌표를 도대체 어떻게 알아내야 하는 걸까... `GestureDetector`를 쓰면 된다는 건 아는데, `GestureDetector`와 `onCellTap`을 같이 썼더니 둘 중 하나만 동작한다. 터치라는 이벤트를 뺏어가서 그런듯.  

이걸 한참 고민했었는데 희망적인 부분을 발견했다. `GestureDetector`와 유사한 동작을 하는 `onCellTap`이라 그런지 터치시 디버그 콘솔에 관련 내용을 출력했다.  

```
Handler: "onTap"
Recognizer: TapGestureRecognizer#3e971
    debugOwner: GestureDetector
    state: ready
    won arena
    finalPosition: Offset(341.0, 458.0)
    finalLocalPosition: Offset(89.0, 89.3)
    button: 1
    sent tap down
```

보면 얘도 `Offset` 관련 정보를 가지고 있다! 이걸 어떻게 빼올지만 고민하면 될듯. 남이 만든 패키지를 쓰는 것도 쉽진 않은 거 같다. 또다른 문제는 이 메시지가 첫 터치시에만 뜬다는 거다. `onCellTap`이 한 번만 실행되는 건 아니던데 왜일까... `Handler`라고 적혀있는 `onTap`은 한 번만 실행되는 건가...? 공부가 필요하다... 

<hr>

## 공부 후 수정
공부 후 수정한다. 위의 내용은 내가 `onCellTap`에게 처리를 맡긴 부분 외의 부분들 터치했을 때 한 번만 나타났고, 이후에는 범위 밖이라고 디버그 콘솔에 에러 메시지가 떴다. 또 `Handler`의 이름이나 `debugOwner`를 봤을 때 `GestureDetector`가 띄운 메시지다. 즉 `onCellTap`이랑은 무관하다는 뜻.  

결국 `calendar view` 패키지 소스 코드로 가서 `onCellTap`의 코드를 확인했다. 경로는 `flutter_calendar_view/lib/src/month_view
/month_view.dart`였다. 
```dart
return GestureDetector(
            onTap: () => onCellTap?.call(events, monthDays[index]),
```
같은 식으로 돼있었다. 조금만 바꾸면 터치 좌표도 주게 바꿀 수 있을 거 같다. 그런데 PR한다고 받아들여 질지 모르겠기 때문에 그냥 내 임의로 바꿔야 될 거 같은데, 그럼 어떻게 `import` 해야되는지와 다른 환경에서도 정상적으로 작동할지를 모르겠다. 

## 최종 해결
결국 패키지를 직접 수정하는 방식으로 해결했다.  
수정하고 적용하는 방식에 대해서는 [별도의 글](https://unvictory2.github.io/miraework%20project/customizing-package/)에 정리했다. 여기선 뭘 수정했는지 정리하겠다.

### 목표
`Month View`의 `onCellTap`이 원래는 좌표값을 전달하지 않는데 전달하게 변경.  
이를 달성하기 위해서는 
- 콜백함수들이 정의돼있는 `lib\src\typedefs.dart`와, 
- 달력 위젯 파일인 `lib\src\month_view\month_view.dart`  

를 수정해야 한다. 

### 사전 지식
콜백 함수란 "다른 함수가 실행을 끝난 뒤 실행되는 함수"라고만 알면 될 거 같다. 비동기 처리와 관련됐다는 건 이해했지만 비동기 처리 자체에 대한 이해가 낮기 때문에 지금 이해하고 가기는 어렵다.  
`tyedefs.dart`에서 온갖 콜백 함수가 정의되고, 각 위젯을 정의하는 개별 파일들에서 다양한 함수들이 이 콜백 함수들을 사용한다. 하나의 콜백 함수를 여러 개의 개별 함수들이 사용할 수 있다.  

### 해결 과정

1. `lib\src\typedefs.dart`
    ```dart
    typedef CellTapCallbackExtended<T extends Object?> = void Function(
        List<CalendarEventData<T>> events, DateTime date, Offset offset);
    ```
    `CellTapCallbackExtended`라는 새로운 콜백 추가. 기존의 `CellTapCallback`에 `Offset offset`이라는 인자 하나만 추가함. 굳이 새로운 콜백을 추가한 이유는 기존의 콜백을 쓰는 함수가 `onCellTap`이 유일하지 않기 때문에 다른 함수들에게 영향을 주지 않기 위해서다.

2. `lib\src\month_view\month_view.dart`  

    온갖 onCellTap 관련 부분에서 콜백 타입을 수정해줌.  

    `class MonthView<T extends Object?>`에서, 
    ```dart
    final CellTapCallbackExtended<T>? onCellTap;
    ```

    `class _MonthPageBuilder<T> `에서도.
    ```dart
    final CellTapCallbackExtended<T>? onCellTap;
    ```

    이후 하단의 `build`에서 실제 전달값 수정. `globalPosition`이 아닌 `localPosition`을 전달하면 해당 셀 내에서의 위치값을 전달하기 때문에 부적절하다.
    ```dart
    return GestureDetector(
                onTapDown: (details) => onCellTap?.call(
                    events, monthDays[index], details.globalPosition), 
    ```
3. 내 프로젝트의 calendar.dart에서 처리  
   이전에 좌표가 없어서 못 했던 작업을 처리하면 된다.
    ```dart
    body: MonthView(
            useAvailableVerticalSpace: true, // 잘리는 거 방지
            onCellTap: (events, date, offset) {
              _showDailySchedule(context, events, date, offset);
            },
          ),
    ```
    MonthView에서 특정 날짜를 선택하면 context, 해당 날짜, 일정, 터치 위치를 `_showDailySchedule`에 인자로 전달한다.
    ```dart
    void _showDailySchedule(BuildContext context,
        List<CalendarEventData<Object?>> events, DateTime date, Offset offset) {
      final RenderBox overlay =
          Overlay.of(context).context.findRenderObject() as RenderBox;

      final RelativeRect position = RelativeRect.fromLTRB(
        offset.dx,
        offset.dy,
        MediaQuery.of(context).size.width - offset.dx,
        MediaQuery.of(context).size.height - offset.dy,
      );
    ```
    해당 함수에서는 오버레이 세팅을 하고, 얻어온 좌표로 어디다가 메뉴를 띄울지 계산한다. 오버레이 세팅을 해줘야 테두리를 클릭해도 자동으로 메뉴가 잘리지 않고 테두리 내에 뜨기 때문에 중요하다.
    ```dart
    showMenu(
          context: context,
          position: position,
          items: [
    ```
    이후 `showMenu`에서는 위에서 설정한 `position`을 그냥 주면 되고 아이템을 나열하기 시작하면 된다. 끝!

![menu](https://github.com/user-attachments/assets/a7567374-e7dd-4a39-a34f-54a77bd80f82)

터치하면 터치한 위치에 메뉴가 뜬다!