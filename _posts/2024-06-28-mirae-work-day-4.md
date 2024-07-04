---
layout : single
title : "미래일경험 4일차 - 기초 앱 제작 실습 분석"
published : true

categories : 
    - 미래일경험
  
toc : true
toc_sticky : true

date : 2024-06-28
last modified : 2024-07-04
---
# 목차
+ ### 서론
+ ### 동작
+ ### 코드
+ ### 용어, 질문, 오류

<hr>

## 서론
오늘은 https://codelabs.developers.google.com/codelabs/flutter-codelab-first?hl=ko#0 이걸 따라하는 시간이었다.  
완성된 코드는 https://github.com/flutter/codelabs/blob/main/namer/step_08/lib/main.dart  
내가 만든 건 주석이 워낙 많아서 이후 올리는 게 좋을듯.  
그와중에 12시 넘어서 수업은 27일이었는데 28일로 해야 되네  

코드에 대한 설명은 대부분 사이트에서 따라 썼을 뿐이며, 코드도 실습 그대로이기 때문에 플러터로 뭔가 만들어봤고, 그 구조를 확인한다에 의의를 두는 게 좋겠다.

## 동작

1. 프로그램을 시작하면 랜덤한 단어 2개가 뜬다.  
2. Next 버튼을 누르면 다음 단어가 나온다. 
3. 단어가 마음에 든다면, Like 버튼을 눌러 즐겨찾기 할 수 있다.
4. 좌측의 홈 버튼을 누르면 이 화면으로 돌아오며, 하트 버튼을 누르면 즐겨찾기한 단어들을 볼 수 있다.  

## 코드
전체 코드는 제일 밑에. 제일 위에 놔야되나 고민됐는데 길어서 그냥 밑에 두기로 했다.
```dart
void main() { 
  runApp(MyApp()); 
}
```
main에서는 MyApp에서 정의된 앱을 실행하라고 Flutter에 지시할 뿐.  
2-1에 배웠던 C++에서 main은 run만 불렀던 거랑 비슷하다. 그런데 그 땐 `.h` 헤더 파일에 클래스와 멤버들을 정의하고, `.cpp` 파일에 실제 구현을 했었는데, 다트는 그런 게 없다. 왜냐면 그건 C++이 초창기 객체지향언어라 있는 거고, Java같은 다른 객체지향 언어에서도 그런 방식은 쓰지 않는다.

```dart
class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return ChangeNotifierProvider(
      create: (context) => MyAppState(), // 앱 전체 상태 생성
      child: MaterialApp(
        title: 'Namer App', // 앱 이름
        theme: ThemeData( // 시각적 테마
          useMaterial3: true,
          colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepOrange),
        ),
        home: MyHomePage(), // 홈 위젯 설정
      ),
    );
  }
}
```
`MyApp`의 코드는 전체 앱을 설정하는 부분이다. `build` 메서드는 반드시 `override` 해줘야 하는데, 여기서 앱 전체 상태 생성, 앱 이름 지정, 시각적 테마 정의, '홈' 위젯(앱의 시작점) 설정 등의 일을 한다. 조금 자세히 살펴보자.  
```dart
const MyApp({super.key});
```
이 부분은 생성자다. 다트에서 생성자는 기본적으로 리턴 타입이 없으며, 클래스의 인스턴스를 생성하기 위한 특별한 메서드일 뿐이다. const는 리턴 타입이 아니라 이 때 생겨난 인스턴스가 불변이라는 걸 알려주는 키워드다.  




```dart
class MyAppState extends ChangeNotifier {
  var current = WordPair.random();
  void getNext() {
    current = WordPair.random(); // 임의의 새 WordPair를 current에 재할당
    notifyListeners(); // 변화 알림
  }

  var favorites = <WordPair>[]; 

  void toggleFavorite() { // favorites 리스트에 넣고 빼기
    if (favorites.contains(current)) {
      favorites.remove(current);
    } else {
      favorites.add(current);
    }
    notifyListeners();
  }
}
```
`MyAppState`는 앱이 작동하는 데 필요한 데이터를 정의한다. 지금은 임의의 단어 쌍이 있는 단일 변수 `current` 만 포함되어 있다.  
이 클래스는 `ChangeNotifier`를 상속 받는다.  여기서 상태가 만들어지면 `ChangeNotifierProvider`를 사용하여 앱 전체에 알려준다. 예를 들어 여기서 현재 단어 쌍이 변경되는 변화가 생기면, '새 상태가 만들어진다'고 할 수 있다. 이 상태 변화는 앱의 다른 위젯들이 알아야 추가적인 처리가 가능하기에 알려야 한다.  
코드에서 이 일이 벌어지는 부분은 `notifyListeners()` 메서드다. MyAppState를 보고 있는 위젯들에게 알림을 보내는 메서드로, `ChangeNotifier`의 메서드다. 

이 클래스의 멤버 변수로 favorites가 있는데, 빈 `WordPair`형 `List`로 초기화돼있다. 목록에 `<WordPair>[]` , 즉 단어 쌍만 포함될 수 있다고 지정돼있다. WordPair가 아닌 건 추가할 수 없기 때문에 null check를 안 해도 된다는 장점이 있는데, 이건 큰 장점이다.

`toggleFavorite()` 함수는 꽤 직관적이다. 이미 좋아요 목록에 들어가 있으면 빼고, 반대의 경우엔 넣은 후 상태 변경을 알린다.

```dart
class MyHomePage extends StatefulWidget { 
  @override
  State<MyHomePage> createState() => _MyHomePageState();
}
```
클래스 `MyHomePage`는 `StatefulWidget`을 상속받는데, 상태가 변할 수 있는 위젯이 된다는 뜻이다.  
`MyHomePage`는 `State`객체를 생성하기만 하는데, 이 객체는 실제로 UI를 구성하고 관리하는 역할을 한다.  
`createState`가 있는 줄에 대해서 자세히 알아보자.
리턴 타입은 `State<MyHomePage>`이다. `State` 객체인데, `MyHomePage`에 대한 상태를 나타낼 거라는 뜻이다.  
함수명은 `createState`고, 인자는 `()`로 없다.   
반환값은 `_MyHomePageState`의 인스턴스이다.  


```dart
class _MyHomePageState extends State<MyHomePage> { // State를 확장하므로 자체 값을 관리(변경)할 수 있습니다. MyHomePage의 build 그대로 얻어옴.
  var selectedIndex = 0; // 새 스테이트풀(Stateful) 위젯은 하나의 변수 selectedIndex만 추적하면 됩니다. 
  @override
  Widget build(BuildContext context) {
```

## 용어, 질문, 오류

### MyHomepage, _MyHomePageState 상세 분석
코드를 짜다 보면 "대충 뭘 하는지 알고있고, 시간이 없으니 넘어가야겠다" 식으로 넘어갈 수 있는 부분도 있다. 그러나 난 더이상 저학년이 아니고, 이 부분은 flutter 코드를 짤 때마다 보게 될 부분이기 때문에 자세히 알아봐야겠다.

```dart
class MyHomePage extends StatefulWidget { 
  @override
  State<MyHomePage> createState() => _MyHomePageState();
}
```
+ 1행 : 클래스 `MyHomePage`는 `StatefulWidget`을 상속받는데, 상태가 변할 수 있는 위젯이 된다는 뜻이다. `StatefulWidget`은 상태를 가지는 위젯을 만들기 위한 기본 클래스다.  `StatefulWidget`이 직접 상태를 변경하는 건 아니고, 상태는 `State` 클래스에서 관리된다.

+ 2행 : `StatefulWidget` 클래스에는 `createState`라는 추상 메서드가 있다. 추상 메서드니까 당연히 `StatefulWidget`을 상속하는 모든 클래스는 `createState` 메서드를 구현해야 한다.

+ 3행 : 구조 : 
  - 리턴타입 `State<MyHomePage>`
  - 함수명과 인자 `createState()`
  - 람다식 `=>`
  - 리턴값 `_MyHomePageState();  
   
  **리턴타입 함수명(인자) => (리턴값)** 형태는 람다 함수의 형식 중 하나로, 문법이라고 보면 된다.

  `State<MyHomePage>`는 `List<int>`와 다를 바 없는 제너릭이다. `State`의 정의를 잠깐 보면
  ```dart
    abstract class State<T extends StatefulWidget> extends Diagnosticable {
    T get widget => _widget;
    BuildContext get context => _element;
    void setState(VoidCallback fn);
    Widget build(BuildContext context);
    // ...
  }
  ```
  로, `State` 클래스는 제네릭 타입 `T`를 사용하며, 이 `T`는 `StatefulWidget`을 상속받는 타입이어야 한다는 걸 알 수 있다. 이 클래스는 이후 `build` 메서드를 통해 ui를 구성한다. 코드 자체의 의미는 `MyHomePage`에 대한 상태를 나타낼 `State` 객체라는 뜻이다.

  `createState()`는 `StatefulWidget`의 상태를 관리할 `State` 객체를 반환해야 한다. 이는 프레임워크상의 규칙이다. 위 코드에서 인자는 없다.

  반환값은 `_MyHomePageState`의 인스턴스이다. 뒤에 괄호가 붙어있기 때문에 새로운 `_MyHomePageState`의 인스턴스를 만들어 반환한다는 걸 알아차릴 수 있다. `new` 키워드가 생략돼있다.  

**정리하면**  
+ 앱 시작은 `MyHomePage`에서 한다. 
+ 이건 `StatefulWidget`으로, 화면이 바뀔 수 있는 위젯이다. 상태 관리는 `StatefulWidget`인 `MyHomePage`가 직접 하지 않고, `state`클래스의 객체를 만들어서 따로 해줘야 한다.
+ 그렇기에 `MyHomePage`는 `state` 객체와 연결돼 있어야 한다. 그걸 하기 위해 `createstate` 메서드가 정의돼있다. 이 메서드는 `MyHomePage`형 `state` 객체인 `_MyHomePageState`를 반환한다. 

### 파라미터의 종류 (named parameter)
+ positional parameter : 지금까지 쓰던 방식, 순서에 따라 인자를 넣어 준다.
+ named parameter : 이 인자가 어느 인자인지 명시하면서 인자를 넣는 방식. 프로그램이 크다면 이 방식을 써야 하기 때문에 익숙해지는 게 좋다고 한다. 이 경우엔 인자를 **중괄호**로 한 번 더 감싸준다.

```dart
void printMessage({String? prefix, required String message}) {
  print('$prefix $message');
}
```
라는 `named parameter` 형식으로 인자를 받는 함수가 있을 때,
```dart
printMessage(prefix: 'Hello', message: 'World'); // named
```
이런 식으로 호출하면 된다.

### required
`Named parameter`에서만 사용되며, 실수를 막기 위한 키워드다. 함수에서 필수적인 인자 앞에 `required`를 씀으로써 함수를 호출할 때 그 인자가 없으면 오류를 발생시킨다.  
위에서 정의한 함수를 호출하는 예시를 살펴보자.
```dart
void main() {
  printMessage(message: 'World'); // 올바른 호출
  printMessage(prefix: 'Hello', message: 'World'); // 올바른 호출
  printMessage(); // 오류: 'message' 인자가 필요함
}
```
`required`인 `message`가 없기 때문에 마지막 줄에서만 오류가 발생한다.

  ### 제너릭




## 전체 코드

```dart
import 'package:english_words/english_words.dart';
import 'package:flutter/material.dart';
import 'package:provider/provider.dart';

void main() {
  runApp(MyApp()); 
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return ChangeNotifierProvider(
      create: (context) => MyAppState(),
      child: MaterialApp(
        title: 'Namer App',
        theme: ThemeData(
          useMaterial3: true,
          colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepOrange),
        ),
        home: MyHomePage(),
      ),
    );
  }
}

class MyAppState extends ChangeNotifier {
  var current = WordPair.random();
  void getNext() {
    current = WordPair.random(); 
    notifyListeners(); 
  }
  var favorites = <WordPair>[]; 
  void toggleFavorite() {
    if (favorites.contains(current)) {
      favorites.remove(current);
    } else {
      favorites.add(current);
    }
    notifyListeners();
  }
}

class MyHomePage extends StatefulWidget { 
  @override
  State<MyHomePage> createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> { // State를 확장하므로 자체 값을 관리(변경)할 수 있습니다. MyHomePage의 build 그대로 얻어옴.
  var selectedIndex = 0; // 새 스테이트풀(Stateful) 위젯은 하나의 변수 selectedIndex만 추적하면 됩니다. 
  @override
  Widget build(BuildContext context) {
    Widget page; // Widget 유형의 새 변수 page를 선언
    switch (selectedIndex) { // selectedIndex의 현재 값에 따라 switch 문이 화면을 page에 할당
      case 0:
        page = GeneratorPage();
        break;
      case 1:
        page = FavoritesPage();
        // page = Placeholder(); // 배치하는 곳마다 교차 사각형을 그려 UI의 해당 부분이 미완성임을 표시하는 편리한 위젯인 Placeholder를 사용
        break;
      default:
        throw UnimplementedError('no widget for $selectedIndex');
    }
    return LayoutBuilder( // 이 위젯을 사용하면 사용할 수 있는 공간의 양에 따라 위젯 트리를 변경할 수 있습니다.
      builder: (context, constraints) { // LayoutBuilder의 builder 콜백은 제약 조건이 변경될 때마다 호출됩니다. 예를 들어 창 크기가 바뀐다던지.
        return Scaffold(
          body: Row( // 하위 요소 두 개가 있는 Row. SafeArea, Expanded.
            children: [
              SafeArea( // NavigationRail(탐색 레일)를 래핑하여 하위 요소가 하드웨어 노치나 상태 표시줄로 가려지지 않도록
                child: NavigationRail( //  탐색 레일
                  extended: constraints.maxWidth >= 600, // 라벨이 아이콘 옆에 표시될지. MyHomePage의 너비가 600픽셀 이상일 때만 라벨을 표시.
                  destinations: [
                    NavigationRailDestination( // 탐색 레일에 두 가지 대상이 있음 : Home, Favorites.
                      icon: Icon(Icons.home),
                      label: Text('Home'),
                    ),
                    NavigationRailDestination(
                      icon: Icon(Icons.favorite),
                      label: Text('Favorites'),
                    ),
                  ],
                  selectedIndex: selectedIndex, // NavigationRail 위젯의 selectedIndex 속성에 현재 상태 클래스 _MyHomePageState의 selectedIndex 변수를 할당
                  // this.selectedIndex = selectedIndex; 왼쪽은 NavigationRail의 속성, 오른쪽은 _MyHomePageState의 인스턴스 변수
                  // "위젯의 속성"은 위젯을 구성하는 데 사용되는 매개변수 또는 인자를 의미
                  // NavigationRail 위젯의 selectedIndex, destinations, onDestinationSelected, extended 등이 속성이라는 것은 Flutter 프레임워크에서 미리 정의된 것
                  // 콜론 :은 Dart의 객체 초기화에서 속성 값을 지정할 때 사용
                  // Flutter에서는 위젯을 초기화할 때 이러한 구문을 자주 사용 selectedIndex: selectedIndex
                  // 왼쪽의 selectedIndex는 NavigationRail 위젯의 생성자에서 정의된 속성입니다.
                  // 오른쪽의 selectedIndex는 현재 클래스(여기서는 _MyHomePageState)의 인스턴스 변수입니다.
                  // 문맥과 클래스 정의를 통해 왼쪽과 오른쪽이 각각 무엇을 의미하는지 알 수 있습니다. > 왼 오에 뭐가 오는지는 일종의 약속이라고 봐도 된다고 함.
                  onDestinationSelected: (value) { // 탐색 레일은 또한 사용자가 onDestinationSelected로 대상 중 하나를 선택할 때 발생하는 작업을 정의
                    setState(() { // onDestinationSelected 콜백이 호출되면 새 값을 콘솔로 인쇄하는 대신 setState() 호출 내 selectedIndex에 할당합니다.
                      selectedIndex = value;
                    });
                  },
                ),
              ),
              Expanded( // 일부 하위 요소는 필요한 만큼만 공간을 차지하고(여기서는 NavigationRail) 다른 위젯은 남은 공간을 최대한 차지해야 하는(여기서는 Expanded) 레이아웃을 표현할 수 있습니다
                child: Container( // 색상이 지정된 Container가 있고 컨테이너 안에는 GeneratorPage가 있습니다.
                  color: Theme.of(context).colorScheme.primaryContainer,
                  child: page,
                ), // 플러터는 정말 무한한 괄호가 있는듯 ㅋㅋ
              ),
            ],
          ),
        );
      }
    );
  }
}

class GeneratorPage extends StatelessWidget { // MyHomePage의 전체 콘텐츠가 새 위젯 GeneratorPage로 추출
  @override
  Widget build(BuildContext context) {
    var appState = context.watch<MyAppState>();
    var pair = appState.current;

    IconData icon;
    if (appState.favorites.contains(pair)) {
      icon = Icons.favorite;
    } else {
      icon = Icons.favorite_border;
    }

    return Center(
      child: Column(
        mainAxisAlignment: MainAxisAlignment.center,
        children: [
          BigCard(pair: pair),
          SizedBox(height: 10),
          Row(
            mainAxisSize: MainAxisSize.min,
            children: [
              ElevatedButton.icon(
                onPressed: () {
                  appState.toggleFavorite();
                },
                icon: Icon(icon),
                label: Text('Like'),
              ),
              SizedBox(width: 10),
              ElevatedButton(
                onPressed: () {
                  appState.getNext();
                },
                child: Text('Next'),
              ),
            ],
          ),
        ],
      ),
    );
  }
}
  @override
  Widget build(BuildContext context) { // 위젯의 상황이 변경될 때마다 자동으로 호출되는 build() 메서드
    var appState = context.watch<MyAppState>(); //MyHomePage는 watch 메서드를 사용하여 앱의 현재 상태에 관한 변경사항을 추적
    var pair = appState.current;

    IconData icon; // 좋아요 눌렀는지에 따라 적당한 아이콘 설정
    if (appState.favorites.contains(pair)) {
      icon = Icons.favorite;
    } else {
      icon = Icons.favorite_border;
    }

    return Scaffold( //모든 build 메서드는 위젯 또는 중첩된 위젯 트리(좀 더 일반적임)를 반환해야 합니다. 최상위 위젯은 Scaffold입니다.
      body: Center( // 다른 위젯들은 알아서 돼있지만, 가운데에 있지 않았던 Column 자체를 가운데 정렬
        child: Column( // 레이아웃 위젯 중 하나. 얘를 wrap with center 해서 윗줄이 생겨남.
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            // Text('A random AWESOME idea:'), // 그냥 텍스트
            // Text(appState.current.asLowerCase), //appState를 사용하고 해당 클래스의 유일한 멤버인 current(즉, WordPair)에 액세스
            BigCard(pair: pair), //이걸로 변경. 원래는 전체 appState에 액세스하지만 실제로는 현재 단어 쌍이 무엇인지만 알면 됩니다.
            SizedBox(height: 10), // 여백 
            Row( // Next 버튼 옆에 새 버튼 추가하려고 wrap with row
              mainAxisSize: MainAxisSize.min, // 사용 가능한 모든 가로 공간을 차지하지 말라고 Row에 지시. 없으면 버튼이 한 쪽으로 몰림.
              children: [
                ElevatedButton.icon(
                  onPressed: () { //람다. 매개 변수로 아무것도 주지 않으면서 toggleFavorite 함수를 부르라
                    appState.toggleFavorite(); // 좋아요 함수 호출
                  },
                  icon: Icon(icon),
                  label: Text('Like'),
                ),
                SizedBox(width: 10), // 여백

                ElevatedButton( //Next 라고 쓰인 버튼
                  onPressed: () {
                    appState.getNext();
                  },
                  child: Text('Next'),
                ),
              ],
            ),
          ],
        ),
      ),
    );
  }

class BigCard extends StatelessWidget { // Text(pair.asLowerCase) 위에서 ctrl + . >> Extract Widget >> BigCard 했더니 BigCard(pair: pair로 바뀌고 이 클래스 자동 추가)
  const BigCard({
    super.key,
    required this.pair,
  });

  final WordPair pair;

  @override
  Widget build(BuildContext context) {
    final theme = Theme.of(context); // Theme.of(context)로 앱의 현재 테마를 요청
    final style = theme.textTheme.displayMedium!.copyWith( // theme.textTheme,을 사용하여 앱의 글꼴 테마에 액세스합니다. 
    // displayMedium 속성은 디스플레이 텍스트를 위한 큰 스타일. nullable이라 원래는 못 부르지만 ! 연산자 붙이면 개발자가 null 아님을 보장
    // displayMedium에서 copyWith()를 호출하면 정의된 변경사항이 포함된 텍스트 스타일의 사본이 반환
      color: theme.colorScheme.onPrimary, // 앱 테마에 액세스해서 새로운 색상 가져옴
    );

    return Card( // Padding 위젯과 Text 위젯이 Card 위젯으로 래핑
      color: theme.colorScheme.primary, // colorScheme 속성과 동일하도록 카드의 색상을 정의합니다. 색 구성표에는 여러 색상이 포함되어 있으며 primary가 앱을 정의하는 가장 두드러진 색상
      child: Padding(
        padding: const EdgeInsets.all(20.0), //padding은 text의 속성이 아니라 위젯임
        child: Text(
          pair.asLowerCase,
          style: style,
          semanticsLabel: "${pair.first} ${pair.second}",
        ),
      ),
    );
  }
}

class FavoritesPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    var appState = context.watch<MyAppState>(); // 앱의 현재 상태를 가져옵니다.

    if (appState.favorites.isEmpty) { // 즐겨찾기 목록이 비어 있으면 No favorites yet이라는 메시지를 중앙에 표시
      return Center(
        child: Text('No favorites yet.'),
      );
    }

    return ListView( // 목록이 비어 있지 않으면 스크롤 가능한 목록을 표시
      children: [
        Padding(
          padding: const EdgeInsets.all(20),
          child: Text('You have ' // 목록은 요약으로 시작됩니다(예: You have 5 favorites*.*).
              '${appState.favorites.length} favorites:'),
        ),
        for (var pair in appState.favorites) // 그런 다음 코드가 모든 즐겨찾기를 반복하고 각 즐겨찾기의 ListTile 위젯을 구성합니다.
          ListTile(
            leading: Icon(Icons.favorite),
            title: Text(pair.asLowerCase),
          ),
      ],
    );
  }
}

```