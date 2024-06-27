---
layout : single
title : "미래일경험 3일차 - 함수, 컬랙션, 클래스"
published : true

categories : 
    - 미래일경험
  
toc : true
toc_sticky : true

date : 2024-06-26
last modified : 2024-06-27
---

# 3일차
## 목차
1. 함수 정의 및 호출 방식
   + 정의 첫 번째 방식
   + 정의 두 번째 방식
   + 호출
2. Collection
   + 리스트
     - 리스트 타입
     - iteration의 함수들
     - foreach, map, fold, reduce
   + 튜플
   + 맵
     - json 형태
     - 데이터베이스의 형태
   + list 등 복사 시 주의사항
3. Callback 함수
   + 익명 함수를 만들어서 쓰는 경우
   + 이름 있는 함수를 만들어서 쓰는 경우
4. 비동기 프로그래밍
5. 클래스
   + 멤버 변수 선언
   + 접근제한자
   + 생성자
   + Getter, Setter
6. 용어, 오류 해결
   + 클래스의 구성 요소
   + JSON
   + RESTful API
   + JSON과 RESTful API의 관계
   + 람다 함수
  
  </br>

## 함수 정의 및 호출 방식
#### 정의 첫 번째 방식
```dart
String makeHello1([String? name, int? age]) { 
  String hello = 'hello, im $name, aged $age';
  return hello; 
}
```
named parameter. 배열 형식, 잘 안 씀.
#### 정의 두 번째 방식
```dart
String makeHello2({String? name="no name", int? age=0}) { 
  String hello = 'hello, im $name, aged $age';
  return hello; 
}
```
마찬가지로 named parameter.  
초기값 안 주면 null.  
함수 만들 떄 웬만하면 이 방식으로 만듦, 안전한 방식임. 

#### 호출
```dart
void main(List<String> arguments) {
  makeHello2(name: 'Dart', age: 10); //호출 시 이런 식으로.
}
```
인자가 복잡해 보일 수 있지만 다 생성자 부르는 거일 뿐이다.

## Collection
### 리스트
#### 리스트 타입
```dart
  var list = [];
  ``` 
  이렇게 만들어도 일단 리스트는 맞다.  
  그러나 이런 식으로 타입 섞인 리스트는 **dynamic 타입**이 된다. 넣을 땐 편리하나 나중에 사용할 땐 불편하기에 잘 안 쓴다.  
```dart
List<String> list3 = [];
```
이런 형식을 더 권장한다.  

  질문 : dynamic 타입은 컴파일 시 타입 추론이 발생하니까 이 리스트에 `add 10`할떈 정수였다가 `add 'Dart'`할때 dynamic으로 바뀌나요?  

  답변 : 이 리스트는 **만들 때부터** (원소가 추가되기 전부터) dynamic 타입이다. 고로 이후 원소가 추가된다고 타입이 바뀌진 않는다.  

```dart
  list.add(10);
  list.add('Dart');
  list.add(3.14);
  list.add(true);
  print('list[0] : ${list[0]}'); //이렇겐 잘 안 부름 
  ```
얘네는 리스트에 들어갈 땐 dynamic 타입이지만, 런타임엔 각자 int double bool 등 속성을 가짐.

리스트를 이렇게 타입 한정시켜서 만들면 관리하기 편하다.
  ```dart
  List<int> list2 =[10, 20, 30]; 
  ```

  #### iteration의 함수들
  ```dart
  List<int> list2 =[10, 20, 30]; 
  list2.add(10); // 중복된 값 추가 가능
  list2.insert(0, 50);
  list2.remove(30); 
  list2.removeAt(0); //0번째 인덱스 요소 삭제
  list2.contains(20); //bool값 반환.
  list2.length; //property.(속성이란 소리)
  print('list의 길이 : ${list2.length}'); //
  list2.clear(); 
  list2.isEmpty; // property
  list2.sort(); // 오름차순
list2.sort((a, b) => b.compareTo(a)); //역순
```

사용 가능한 함수들.  
+ add(a) : 값 a 추가. 중복된 값도 가능.  
+ insert(a,b) : 특정 위치 a에 b 넣기.
+ remove(a) : 값이 a인 요소 삭제.
+ removeAt(a) : a번째 인덱스 요소 삭제
+ list2.contains(a) : 데이터 중 a가 있는지.
+ clear() : 모든 요소 삭제
+ isEmpty : 리스트가 비어있는지. 함수가 아니라 property.  
+ length : 마찬가지로 property. 함수가 아니라 클래스가 가지는 값 중 하나라는 뜻.
+ sort() : 리스트 값을 오름차순으로 정렬.
+ sort((a,b) =? b.compareTo(a)) : sort 하는 방법을 알려주는 함수를 매개변수로 전달.
+ b.compareTo(a): 앞`(b)`이 크면 1 뒤 크면 -1 같으면 0. `b.` 이니까 뒤에꺼 기준으로 판단.  
  b는 정수인데 이게 가능한 이유는 **모든 게 객체**이기 때문. 20은 그냥 숫자지만, **20.toString()** 같이 쓸수도 있다.  
  int 클래스에 compareTo가 정의돼있어서 b.compareTo가 가능.

iterable(반복가능한 개체. list, map, set(중복 안 되는 iterable) 등)에서 이 함수들은 전부 사용 가능.  

#### foreach, map, fold, reduce
굉장히 많이 쓰는 4가지 함수.   

이 넷은 for 같은 함수에 비해서 리스트에서 데이터 순회 최적화 해줌. 쓸데없는 반복이 없기 때문에 성능에 좋다.

1. forEach()
```dart
list2.forEach((a) => print(a)); 
```
반환값이 없음 = 내부적으로 처리한다는 뜻이다.  
리스트 안 값 순회하면서 일괄적으로 전부 변경한다.  
`(a)` 이후 함수 줘도 되고 람다 줘도 되고.

```dart
list2.forEach(print); 
```
이렇게 써도 위와 같은 동작. 들어오는 매개변수와 사용할 매개변수가 같을 때. (위의 경우엔 `a`)
```dart
list2.forEach((a) => a*10);
```
등등.

2. map()  
```dart
list2.map((a) => a*10).toList(); //a*10 한 리스트 반환.
```
순회 후 또 다른 리스트 내뱉어줌. 

3. fold()  
```dart
int number = list2.fold<int>(1, (a,b) => a+b);
```  
기존 값들로 list를 만들어 내는 게 아니라 새로운 데이터 한 개를 만들어냄.

초기값인 `1`이 들어오고, `b`에 `list2`의 첫 값(10) 들어옴.   
두 번째 돌 때 `1+10`이 `a`로 들어오고, `b`에 두번째 값 20이 들어옴.   
그 결과가 다시 `a`로, `b`에 세번째 값 30이.  

4. reduce()  
```dart
list2.reduce<int>((a,b) => a+b);
```
시작값이 리스트의 첫 번째 값인 fold라고 보면 된다.


### 튜플
  list와 같지만 값 변경이 불가함. DB에서 데이터를 Read only로 가져왔다면 tuple.  
  어떤 데이터를 가져와서 여기저기서 이용했을 때, 이게 바뀌어버리면 연쇄 오류 일으킬 가능성이 굉장히 큼. 이 때 튜플을 이용하면 유용하다.   

  dart엔 tuple의 개념이 없어서 다른 식으로 작성해야 한다.
  ```dart
  var tuple1 = List<int>.filled(3, 0, growable: false);
  ```
  3칸 짜리로 만들고 값 0으로 초기화. 추가 불가. 추가 안 되는 리스트.
  ```dart
  UnmodifiableListView<int> tuple2 = UnmodifiableListView([10,20,30]);
  ```
  완전히 수정 불가능한 리스트. 가장 많이 쓰는 방식. 
  ```dart
  List<String> tuple3 = const ['Dart', 'Java', "Kotlin"]; // tuple3[0] = 'Python'; // runtime error.
  List<String> list4 = List.empty(growable : true); // 빈 리스트 생성. 이렇게도 쓰기는 함. 
  ```
  리스트 만들면서 초기화할 일 잘 없어서 잘 안 쓰지만, `const` 이용해서 초기화 가능. 하단의 방법도 잘 안 쓰는 방법이다.
  

### 맵
key, data 쌍으로 데이터 관리
```dart
Map<String, dynamic> playerInfo = {};
```
  map 같은 경우는 이렇게 많이 씀.  
  JSON Data, Restful API는 어떤 데이터가 올지 모르기 때문. 남들이 만든 데이터를 받아야 될 때 복잡한 형태로 올 수 있으니까.  
통신하려면 데이터를 json 형태로 줘야함. 그래서 무슨 개발자 될거든간에 map, list는 계속 따라다님.  
  list는 왜 안 되나? 인덱싱의 차이. **Map은 key가 있으니까** 이걸로 가져오면 됨. 
 ```dart
  playerInfo['name'] = 'Dart'; // { "name" : "Dart"}
  playerInfo['age'] = 10; // {"name" : "Dart", "age" : 10}
  playerInfo['isStudent'] = true; // {"name" : "Dart", "age" : 10, "isStudent" : true}
  playerInfo['name'] = 'Python'; // 값 수정
```
없는 키 주고 데이터 넣으면 추가. 
위의 정의상 `key`만 `String`이면 되고 데이터 자체는 아무 자료형이나 가능함.
이미 있는 키일 경우 데이터 수정.

#### json 형태
```dart
  Map<String, dynamic> playerInfo1 = {'name': 'Dart', 'age': 10, 'isStudent' : true};
  Map<String, dynamic> playerInfo2 = {'name': 'Son', 'age': 20, 'isStudent' : false};
  List<Map<String,dynamic>> player = []; 
  player.add(playerInfo1);
  player.add(playerInfo2);
  ```
  **리스트의 원소가 map 형태**임.  
  하나의 개체에 대한 여러 정보를 각각의 맵 `playerInfo`에 저장하고, 이걸 리스트 `player`로 만든다.

#### 데이터베이스의 형태
  ```dart
  Map<String, List<String>> player2 = {
    'name': ['Dart', 'Java', 'Kotlin'], 
    'age' : ['10', '20', '30']
    }; 
```
  맵의 키는 `String`, 데이터는 `list`다. 이런 식으로도 나옴.  
  **데이터베이스의 데이터**를 표현했다고 볼 수 있다.  
  `key`값 헤더(스키마), `list`가 각 행. `name`이라는 열, `age`라는 열. 첫 번째부터 열 데이터는 dart 10, java 20, kotlin 30. 

### list 등 복사 시 주의사항
```dart
  List<int> list5 = list2;
  list5[0] = 100; //list5[0] = 100, list2[0]도 100.
```
복사한 이후 list5의 값을 100으로 바꿨지만, **list2의 값까지** 같이 바뀜. 

```dart
  List<int> newList(List<int>? list) {
    return list ?? [];
  }
```
리스트 받아서 새 리스트 리턴하는 함수. 내부에서의 처리에서 원본을 건든다. 위 list5 예시와 같음.  
해결하려면 
1. 튜플로 복사본 만들어서 데이터 보내던가, 
2. 원본의 카피를 만들어서 카피는 바뀌든 말든 신경 쓰지 않으면 된다.  

어차피 준 list도 바뀌니까 새로 만든 list 반환할 필요 없음.

## Callback 함수
콜백 함수 : 다른 함수의 매개변수로 전달되는 함수.
```dart
void performAction(String msg, Function(String) action) {
print(action(msg));
}
``` 
위 함수는 문자열 `msg`와 함수 `action`을 매개변수로 받는다. `action`은 문자열을 입력받아 무언가를 반환하는 함수다. `performAction`이 호출되면 `action` 함수가 실행된다.  
위 예시에서 Function으로 넘어오는 건 함수의 포인터다. (이 경우엔 action에 대한 포인터)  
이 함수를 main에서 실제로 활용하는 예시를 살펴보자.

### 익명 함수를 만들어서 쓰는 경우
```dart
performAction("Hello Dart", (msg) {
  return "Message: $msg";
});
```
익명 함수를 즉석으로 만들어서 사용하는 방식이다. 버튼 처리 등에 사용한다.
- 익명 함수 : 이름 없는 함수로, Dart에서 익명 함수는 `(매개변수) { 함수의 내용 }` 형태로 작성된다.
- 위 예시에서 익명 함수는 `(msg) { return "Message: $msg"; }` 이고, 이 익명 함수 전체가 `performAction`의 두 번째 인자다.

함수의 동작 : 
1. `"Hello Dart"`는 `msg`에, `(msg) { return "Message: $msg"; }`는 `action`에 전달된다. 즉 각각의 인자가 전달된다.
2. `performAction` 함수 안에서 `action(msg)`는 `("Hello Dart")`를 매개변수로 호출된다.  
   이게 무슨 소리냐면, 우리의 두 번째 인자인 익명 함수에 `"Hello Dart"`를 넣어서 실행한다는 소리다. 우리의 익명 함수는 인자 `msg`를 받아서 `"Message : $msg"`를 리턴하는 함수라는 걸 기억하자.
3. `action("Hello Dart")`는 `"Message: Hello Dart"`를 반환한다.  
   위에서 말했듯 익명 함수한테 인자로 `"Hello Dart"`를 줬으니까 `"Message: Hello Dart"`를 반환할 거다.
4. 이 결과를 출력하는 게 performAction의 동작의 끝이니까 `print("Message: Hello Dart")`가 호출되어 콘솔에 출력된다.
```dart
performAction("Hello Dart", (msg) => $"Message : $msg");
```
이렇게 같은 동작을 하는 함수를 람다를 이용해서 표현할 수도 있다.

```dart
  performAction('Hello', makeString); // 할 일을 딱 정해둠
```
### 이름 있는 함수를 만들어서 쓰는 경우
```dart
  //얜 main 밖에서 정의
  String makeString(String? msg) {
    return "Message : $msg";
  }
```
이건 또 다르게 부르는 방법이다. 복잡한 루틴 처리해야 할 때 스레드 처리 등에 쓰이는데, 이름있는 함수를 사용하는 방법이다. 함수 구현을 따로 밖으로 빼버렸을 뿐, 동작은 익명 함수 쪽을 이해했다면 이해할 수 있을 거다.  어느 쪽을 쓰든 구현하기 나름이다.

## 비동기 프로그래밍
io가 있을 때(다운로드 등) cpu는 논다. 이 때 ui 먹통되는 일이 잦다. 노느라 ui 업데이트 못 하니까.   

고로 스레드가 따로 동작한다. 화면 업데이트 하는 콜백 함수를 다운 받고 있는 스레드에게 넘겨준다. 파일 얼마나 받았는지는 이 스레드가 아니까. 얘가 화면에 다운로드 얼마나 진행됐는지 업데이트한다.

## 클래스

### 멤버 변수 선언
```dart
class Person {
    String name;
    int age;
}
```
이 형식으로 변수들을 정의하면 변수들이 **non nullable**이기 때문에 선언과 동시에 초기화 돼야 한다.

```dart
class Person {
    String? name;
    int? age;
}
```
첫 번째 해결 방안은 변수들을 nullable로 바꾸는 거다.
```dart
class Person {
    late String name;
    late int age;
}
```
두 번째 해결 방안은 `late`를 붙여주는 거다. null값 안 들어간다고 개발자가 보장하겠다는 의미.

참고로 개발하면서 null check를 자주 하게 된다. 포인터 등을 잘못 사용해서 null값이 생기면 런타임 에러가 생기기 때문이다.  
그렇기에 non nullable인 건 좋은 거다. null check를 안 해줘도 되니까.  


### 접근제한자
Dart에서 접근제한자는 private, public 둘 뿐. `_`가 그어졌으면 private이다.
```dart
class Person {
    late String _name; //private
    late int age; //public
}
```
### 생성자
```dart
Person() { // 기본 생성자
_name ='Guest';
_age = 0;
}
```
생성자 만드는 건 다른 언어와 같다. 기본 생성자를 만들어봤다.
```dart
Person.withValue(String name, int age) {
this._name = name;
this._age = age;
}
```
다만 오버로딩이 안 되기 때문에, 인자를 받는 생성자를 만들 땐 다른 이름을 넣어줘야 한다.
```dart
Person.withValue(this._name, this._age);
```
이렇게 줄여서 쓸 수도 있다.
```dart
void introduce() {
print('him im $_name and im $_age');
}
```
이런 함수도 `Person` class 내부에 있다고 가정했을 때, main에서 사용하는 예시를 보자.
```dart
Person kim = new Person.withValue('Kim, 20'); //new는 생략 가능하다
kim.introduce();

Person lee = Person('lee', 30);
lee.introduce();
```

### Getter, Setter
setter를 아무 제약 없이 만들 거라면, 관리 편하게 그냥 public으로 만드는 게 낫다.  
setter에 제약이 없으면 잘못된 값으로 데이터를 설정할 수 있고, 그 책임은 외부 개발자(다른 사람)이 지게 된다.

```dart
get name => _name; // getter는 이래도 괜찮다.
set name(value) => _name = value; // 잘못된 setter.

//이렇게 만들어야. 오류 줄여줌.
set name(value) {
if (value.isEmpty)
    throw Exception('empty strings cant be used');
_name = value;
}
```
이런 식으로 value가 비어있으면 안 된다는 식으로 **제약 사항**을 걸어줘야 setter의 의미가 있다.

이걸 main에서 호출할때는 
```dart
kim.name = 'strong';
```
getter와 setter는 프로퍼티라 함수 안 부르고 이렇게 바로 setter 사용해도 된다.
메서드 호출하듯 `person.getName()` 또는 `person.setName('John')`이 아니라, `person.name` 또는 `person.name = 'John'`과 같이 변수에 접근하듯이 호출할 수 있다.

## 용어, 오류 해결
### 클래스의 구성 요소
- 멤버 변수 (Member Variables): 클래스의 상태를 나타내는 변수들. 필드(Fields)라고도 합니다.
- 멤버 함수 (Member Functions): 클래스의 동작을 정의하는 함수들. 메서드(Methods)라고도 합니다.
- 생성자 (Constructors): 클래스의 인스턴스를 초기화하는 특별한 함수들.
- 프로퍼티 (Properties): Dart에서는 getter와 setter를 포함하여 멤버 변수에 대한 접근을 제어하는 방법을 제공합니다.
- 접근 제한자 (Access Modifiers): Dart에서는 _로 시작하는 이름을 사용하여 private 멤버를 표시합니다. Public 멤버는 기본적으로 접근이 가능합니다.

### JSON
JavaScript Object Notation. 데이터를 저장하고 전송하는 데 사용되는 경량의 **데이터 교환 형식**입니다. JSON은 인간이 읽고 쓸 수 있고, 기계가 파싱하고 생성하기 쉬운 텍스트 형식으로, 기본적으로 **키-값 쌍의 형태로 데이터를 표현**합니다. JSON은 대부분의 프로그래밍 언어에서 쉽게 사용할 수 있습니다.  

즉 맵의 형태로 데이터를 주고 받는 방식이라는 소리인가보다.

### RESTful API
REST(Representational State Transfer)는 웹 서비스를 설계하는 아키텍처 스타일입니다.  RESTful API는 HTTP 프로토콜을 기반으로 **클라이언트와 서버 간에 데이터를 주고받는 방식**입니다. RESTful API는 리소스를 URI(Uniform Resource Identifier)로 식별하고, HTTP 메서드를 사용하여 해당 리소스에 대한 CRUD(Create, Read, Update, Delete) 작업을 수행합니다.

짧게 말하면 클라와 서버 간 데이터 통신 방법이라는 소리군.

### JSON과 RESTful API의 관계
JSON은 RESTful API의 데이터 교환 형식으로 자주 사용됩니다. 클라이언트는 서버에 요청을 보낼 때 JSON 형식의 데이터를 전송하고, 서버는 JSON 형식의 응답을 반환합니다. 이러한 방식은 간결하고 읽기 쉽기 때문에 널리 사용됩니다.

JSON과 RESTful API가 서로밖에 없는 건 아니지만, 가장 자주 같이 쓰이는 짝이라는 뜻이다.

### 람다 함수
- 정의
  
람다는 함수를 간단하게 쓰는 방법이다.
함수를 요약해서 **(인자) => 리턴값** 형식으로 표현한다.  
람다 함수는 Dart, Kotlin 말고도 Python, JS, Java, C#에서도 사용된다고 한다. Python JS Java를 전부 배웠는데 아무리 1~2학년 때 배웠다고 해도 이제서야 존재를 알게 되다니 매우 신기하다... (3-1 코틀린 수업 = 대략 3달 전때 처음 존재 알게 됐음)
- 예시
  
일반 함수가 람다 함수로 바뀌는 과정을 알아보자.
```dart
String makeMessage(String msg) {
  return "Message: $msg";
}
```
일반 함수. `msg`를 받아서 `"Message: $msg"`이라는 문자열로 바꿔서 리턴한다.   
반환값 함수명 (인자) { 함수내용 }
```dart
(String msg) {
  return "Message: $msg";
}
```
익명 함수로 바뀌었다. 이름과 리턴 타입 부분 생략.  
(인자) { 함수 내용 }
```dart
(String msg) => "Message: $msg";
```
이를 더 간결하게 썼다. 이게 람다 함수. 단일 표현식일 때만 가능하다.  
(인자) => { 함수 내용이 리턴 뿐일 경우, 반환할 결과만 }

람다를 이용해서 함수 호출도 더 간단하게 할 수 있다.
```dart
void performAction(String msg, Function(String) action) {
  print(action(msg));
}
```
이런 함수가 있다고 치고, 이걸 람다로 불러보자.
```dart
performAction("Hello Dart", (msg) => "Message: $msg");
```
- 제약
  
  + 단일 표현식: 람다 함수는 **단일 표현식**으로 구성되어야 한다.
  + 명시적 반환: 람다 함수는 항상 값을 반환한다. 그렇지 않다면 일반 함수나 익명 함수를 쓰는 게 낫다.
  + 익명성: 람다 함수는 익명 함수이기 때문에 이름이 없다. 따라서 람다 함수를 직접 호출할 수 없고, 반드시 **다른 함수의 인자로 전달하거나 변수에 할당해야** 한다.
