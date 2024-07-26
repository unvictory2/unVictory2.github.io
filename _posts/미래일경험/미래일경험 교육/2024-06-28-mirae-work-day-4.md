---
layout : single
title : "미래일경험 4일차 - 기초 앱 제작 실습 분석"
published : true

categories : 
    - Miraework
  
toc : true
toc_sticky : true

date : 2024-06-28
last modified : 2024-07-26
---
# 목차
+ ### 서론
+ ### 동작
+ ### 코드 
+ ### 용어, 질문, 오류

<hr>

## 서론
오늘은 [flutter에서 제공하는 기본적인 앱 만들기](https://codelabs.developers.google.com/codelabs/flutter-codelab-first?hl=ko#0)를  따라하는 시간이었다.  
[완성된 코드 예제 코드](https://github.com/flutter/codelabs/blob/main/namer/step_08/lib/main.dart)  
[내가 만든 최종 코드](https://github.com/unvictory2/mirae_work/blob/main/flutter_application_1/lib/main.dart)

## 동작

1. 프로그램을 시작하면 랜덤한 단어 2개가 뜬다.  
2. Next 버튼을 누르면 다음 단어가 나온다. 
3. 단어가 마음에 든다면, Like 버튼을 눌러 즐겨찾기 할 수 있다.
4. 좌측의 홈 버튼을 누르면 이 화면으로 돌아오며, 하트 버튼을 누르면 즐겨찾기한 단어들을 볼 수 있다.  

## 코드 분석
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
`MyApp`의 코드는 전체 앱을 설정하는 부분이다. `build` 메서드는 반드시 `override` 해줘야 하는데, 여기서 앱 전체 상태 생성, 앱 이름 지정, 시각적 테마 정의, '홈' 위젯(앱의 시작점) 설정 등의 일을 한다.   

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

`toggleFavorite()` 함수는 꽤 직관적이다. 이미 좋아요 목록에 들어가 있으면 빼고, 반대의 경우엔 넣는다. 어떤 경우든 마지막엔 상태 변경을 알린다.

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
class _MyHomePageState extends State<MyHomePage> { // 
  var selectedIndex = 0;
  @override
  Widget build(BuildContext context) {
    Widget page;
    switch (selectedIndex) { // 
      case 0:
        page = GeneratorPage();
        break;
      case 1:
        page = FavoritesPage();
        break;
      default:
        throw UnimplementedError('no widget for $selectedIndex');
    }
```
`_MyHomePageState`는 State를 확장하므로 자체 값을 관리(변경)할 수 있고, MyHomePage의 `build`를 그대로 얻어온다. `build`에서는 `selectedIndex`의 현재 값에 따라 switch문으로 다른 화면을 `page`에 할당한다. `GeneratorPage`는 새 단어쌍을 보여주는 페이지, `FavoritesPage`는 하트 눌러둔 단어를 볼 수 있는 페이지다. 

몇몇 중간 부분은 전체 코드의 주석과 flutter 홈페이지로 이해할 수 있을 거 같아서 생략한다. 솔직히 말하면 시간도 너무 지났고 블로그 구조 관련된 걸 하느라 여력이 없다...
```dart
class BigCard extends StatelessWidget {
  const BigCard({
    super.key,
    required this.pair,
  });
```
단어 쌍을 보여주는 부분을 더 꾸미기 위해 별도의 위젯 BigCard로 분리한다. 

```dart
  final WordPair pair;

  @override
  Widget build(BuildContext context) {
    final theme = Theme.of(context); 
    final style = theme.textTheme.displayMedium!.copyWith( 
      color: theme.colorScheme.onPrimary, 
    );
```
이 부분은 테마와 관련된 부분이다.  
`Theme.of(context)`로 앱의 현재 테마를 요청한다. `theme.textTheme`을 사용하여 앱의 글꼴 테마에 액세스합니다. `displayMedium` 속성은 디스플레이 텍스트를 위한 큰 스타일이다. `nullable`이라 원래는 못 부르지만, ! 연산자 붙이면 개발자가 null 아님을 보장하기 때문에 부를 수 있다.`displayMedium`에서 `copyWith()`를 호출하면 정의된 변경사항이 포함된 텍스트 스타일의 사본이 반환된다. 해당 함수 내부의 코드는 앱 테마에 액세스해서 새로운 색상을 가져온다.

## 용어, 질문, 오류

### MyHomepage, _MyHomePageState 구조 분석
이 프로그램의 로직에 사용되는 부분말고, 모든 플러터 코드에 반복적으로 나오는 부분에 대해서 자세히 알아보자. 

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

### MyApp 상세 분석

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

```dart
const MyApp({super.key});
```
이 부분은 생성자다. 다트에서 생성자는 기본적으로 **리턴 타입이 없으며,** 클래스의 인스턴스를 생성하기 위한 특별한 메서드일 뿐이다. 

1. const는 리턴 타입이 아니라 이 때 생겨난 인스턴스가 불변이라는 걸 알려주는 키워드다.  

2. {}는 named parameter라는 뜻이다. 만약 이 생성자를 외부에서 부를 일이 있으면 `MyApp(key: Key('myKey'));`같이 부르게 된다. 부모 클래스에 `key`를 전달하기 때문에 `super.key`값을 주는 게 아니라 그냥 `key`값을 준다.

3. - `key`란 데이터베이스에서의 `primary key`와 비슷한, 위젯들을 구분해주는 역할을 한다. 즉 `key`를 이용해 동일 종류의 위젯이 여러 개여도 서로 구분할 수 있다. 
   - 명시적으로 `key`를 지정해주지 않는 경우 위젯 트리의 위치로 위젯들이 식별되는데, 이 위치가 변할 경우 문제가 발생할 수 있기에 위치가 변할 수 있다면 `key`값을 지정해주는 게 바람직하다고 한다. 
   - `key`값을 설정해주면 변하지 않으며, 자주 사용되는 방법은 아니지만 여러 위젯의 `key`를 같게 설정할 수도 있다.

4. `super`는 상속에서 부모에게 접근하는 키워드다. `super.key`는 코드 문맥만 보면 단순히 부모 클래스의 속성을 나타내는 것처럼 보일 수 있지만, **Dart의 규칙에 따라 부모 클래스의 생성자를 호출하고 인자를 전달하는 역할**을 한다.  
이런 관례는 주로 플러터에서만 사용되고, 다른 언어에서는 `super.key`를 자주 쓰지 않는다. 물론 다음과 같이 자식 클래스에서 부모 클래스의 생성자를 부르는 일은 다른 곳에서도 있다.

```java
class Parent {  
  Parent(String name) {
    // ...
  }
}

class Child extends Parent {
  Child(String name) {
    super(name);
  }
}
```
참고로... 당연하지만 데이터베이스의 `super key`와는 다르다. (절대 찾아보다가 이건 뭐였더라? 싶어서 쓰는 거 아님)
`super key`는 `primary key + a`로 행끼리 구분만 되면 쓸데없는 다른 속성까지도 포함할 수 있는 키임. 고로 모든 `primary key`는 `super key`지만 v/v는 성립하지 않는다.


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