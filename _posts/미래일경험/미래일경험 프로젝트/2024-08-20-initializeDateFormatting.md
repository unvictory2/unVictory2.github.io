---
layout : single
title : "[미래일경험 프로젝트] 문제해결 - initializeDateFormatting 인자 넣기"
excerpt : "요일을 한국어로 띄우려면 필요한 작업"
published : true

categories : 
    - Miraework Project

date : 2024-08-20
last modified : 2024-08-20
---
날짜를 8.20(화) 같은 형식으로 띄우려면 어떻게 해야 할까? 일단 `DateTime.now()`로 현재 시간을 받아온다. 이 때 받아오는 형식은 `2024-08-19 15:30:15.123456`로 받아온다. 얘를 원하는 형식으로 바꾸려면 가공 작업을 거쳐야 하는데,
```dart
String _formatDate(DateTime date) {
    String formattedDate = DateFormat('M.d').format(date);
    String weekday = DateFormat('E', 'ko').format(date);
    return "$formattedDate($weekday)";
}
```  
이런 식으로 하면 된다. 윗줄은 8.20을 만들어주고, 아래쪽은 (요일)을 한들어준다. 근데 영어가 아니라 한국어로 요일을 표현하려고 하기 때문에 이대로 실행하면 오류가 난다. (실행 시에는 오류가 안 나지만 저 함수를 실행할 일이 생기면 난다)

```debug console
════════ Exception caught by gesture ═══════════════════════════════════════════
Locale data has not been initialized, call initializeDateFormatting(<locale>).
════════════════════════════════════════════════════════════════════════════════
```
로컬 데이터를 초기화 시키라는 뜻이다. main에 가서 
```dart
  WidgetsFlutterBinding.ensureInitialized();
  await initializeDateFormatting('ko', '');
```
이런 코드를 더해준다. 어디서 더해도 상관은 없나본데 주로 `main` 시작하자마자 더하는 거 같길래 나도 그렇게 했다. <u>`initializeDateFormatting`은 어느 패키지를 import하는지가 중요하다.</u> `import 'package:intl/date_symbol_data_file.dart';`로 가져오면 안 된다. `<asynchronous suspension>`이 뜨면서 프로그램이 실행되지 않는 걸 볼 수 있다! 꼭 `import 'package:intl/date_symbol_data_local.dart';`로 가져와야 정상적으로 작동한다.  
왜 그런 걸까? `initializeDateFormatting`에 대해서 좀 알아보고 추측해본다.
```dart 
Future<void> initializeDateFormatting([String? locale, String? ignored])
Type: Future<void> Function([String?, String?])

package:intl/date_symbol_data_local.dart

//This should be called for at least one [locale] before any date formatting methods are called. 
//It sets up the lookup for date symbols. Both the [locale] and [ignored] parameter are ignored, as the data for all locales is directly available.
```
이 함수에 대한 설명은 뒤가 `local`로 끝나는 쪽 패키지에 대한 설명이란 걸 알 수 있다. 왜냐면 뒤가 `file`로 끝났던 패키지의 함수는 인자에 `null`을 허용하지 않아서 빈 문자열을 넣어줘야 했기 때문이다. 앞쪽 인자는 지역명, 뒤쪽 인자는 관련 파일 경로다. 추측해보자면 `local`쪽 함수는 인자가 비어있으면 (설명대로) 무시하지만 `file`쪽 함수는 무조건 해당 경로에 가서 파일을 찾아보는 거 아닌가 싶다. 그래서 영원히 찾지 못해서 멈춰버리는것.  
이 문제에 대한 해결책은 [어느 깃허브 이슈](https://github.com/dart-lang/i18n/issues/742)에서 찾았다. 역시 인터넷엔 모든 게 있고 개발자들은 친절하다.  

![calendar](https://github.com/user-attachments/assets/f8be3bb6-53d8-4d79-b268-e6611562591e)  

덕분에 여기까지 만드는 데 성공했다. 지금 쓰면서 알게 됐는데 터치한 날짜의 일정이 나오는 거니까 "오늘의 일정"이란 표현은 틀렸다. 저 사진만 해도 오늘은 8.20일인데 21일을 터치해서 `오늘의 일정 8.21(수)`라고 뜬다. 고쳐야겠다.  

![figma_calendar](https://github.com/user-attachments/assets/0058f358-126e-4de2-92cf-3be512c5dfbc)  

어쨌든 조금씩 이 사진과 닮아가게 만들고 있는 거 같다!