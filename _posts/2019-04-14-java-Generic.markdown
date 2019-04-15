---
layout: post
title:  "제네릭 (Generic)"
date:   2019-04-14 20:18:32
author: Dongy
categories: Java
cover:
---

<br>
코드블럭(class, interface, method) <strong>내부에서 쓸 자료형을 외부에서 지정하는 기법</strong>을 <strong>제네릭 (Generic)</strong>이라고 합니다.<br>
<br>
간단한 코드로 살펴보면,<br>
```
List<String> stringList = new ArrayList<>();
```
이처럼 선언, 생성, 할당하면서 동시에 ArrayList 내부에 담을 타입을 지정하는 기법입니다.<br>
<br>
반면에, 아래와 같이 아무 타입도 지정하지 않고 (자동으로 Object 타입로 지정) 여러가지 자료형을 허용하면, <br>
<strong>두 가지 문제</strong>가 발생합니다.<br>

```
List stringList = new ArrayList<>();
```
<br>
<br>
### 첫 번째 문제는 컴파일 시 타입 체크를 할 수 없습니다.
다음 코드를 살펴보시면, IDE는 어떤 애러도 발견하지 못합니다.<br>
```
List list = new ArrayList<>();
list.add("1");

Integer one = (Integer)list.get(0);
```
하지만 실행하면 다음과 같은 애러를 만나게 됩니다.<br>
<br>
java.lang.ClassCastException: java.base/java.lang.String cannot be cast to java.base/java.lang.Integer<br>
<br>
<strong>이와 같이 실행 전까지는 잘못된 캐스팅인지 알 수 없다는 문제가 생깁니다.</strong><br>
<br>
<br>
반면, 자바 컴파일러는 <strong>제네릭 코드에 대해 강한 타입 체크</strong>를 합니다.<br>
<strong>강한 타입 체크란 컴파일 시에 미리 타입 유효성을 체크하여 실행 시 일어날 수 있는 타입 에러를 사전에 방지</strong>합니다.<br>
```
List<String> list = new ArrayList<>();
list.add("1");

Integer one = list.get(0);
```
위와 같은 코드는 IDE가 코드 실행 전에 다음과 같이 타입 애러가 발생했음을 알려줍니다.<br>
<strong>Incompatible types.</strong><br>
<strong>Required: java.lang.Object</strong><br>
<strong>Found:     java.lang.String</strong><br>
<br>
<br>


### 두 번째 문제는 타입 변환(casting)을 명시적으로 해줘야합니다.
비 제네릭 코드는 불필요한 타입 변환을 해야하기 때문에 프로그램 성능에 악영향을 미칩니다.<br>
다음 코드를 보면 List에 문자열 요소를 저장했지만, 요소를 찾아올 때는 반드시 String으로 타입 변환을 해야 합니다.<br>
```
List list = new ArrayList();
list.add(“1”);
String one = (String) list.get(0);
```
<br>
위 코드를 제네릭 코드로 수정하면 List에 저장되는 요소를 String 타입으로 국한하기 때문에 요소를 찾아올 때 타입 변환을 할 필요가 없어 프로그램 성능이 향상됩니다.<br>
```
List<String> list = new ArrayList<String>();
list.add(“1”);
String one = list.get(0);
```

## 관련 포스팅
https://dongyyy.github.io/java/2019/04/14/java-Generic.html <br>
https://dongyyy.github.io/java/2019/04/14/java-Generic(2).html