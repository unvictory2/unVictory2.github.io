---
layout : single
title : "[미래일경험 프로젝트] 문제 해결 - 패키지 터치 함수 사용시 터치 좌표 알아내기"
excerpt : "GestureDetector와 동시 사용 불가시 어떻게 해야 되는 걸까"
published : true

categories : 
    - Miraework Project

date : 2024-08-20
last modified : 2024-08-20
---

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
지금 내 문제와 관련이 있는 건 그 밑의 `showMenu`인데 <u>여기서 `position`을 어떻게 줘야 될지 모르겠다는 거다.</u> 내가 터치한 좌표를 `onCellTap`에서 이 함수를 부를 때 인자로 줘야 될 거 같다. 근데 내가 터치한 좌표를 도대체 어떻게 알아내야 하는 걸까... `GestureDector`를 쓰면 된다는 건 아는데, `GestureDector`와 `onCellTap`을 같이 썼더니 둘 중 하나만 동작한다. 터치라는 이벤트를 뺏어가서 그런듯.  

이걸 한참 고민했었는데 희망적인 부분을 발견했다. `GestureDector`와 유사한 동작을 하는 `onCellTap`이라 그런지 터치시 디버그 콘솔에 관련 내용을 출력했다.  
```debug console
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