---
layout: post
title:  "예외 (Exception)"
date:   2019-04-24 20:14:20
author: Dongy
categories: Java
cover:
---


## 예외와 예외 클레스

<strong>컴퓨터 하드웨어의 오동작 또는 고장으로 인해 응용프로그램 실행 오류가 발생하는 것</strong>을 자바에서는 <strong>에러(error)</strong>라고 합니다.<br>
에러는 JVM 실행에 문제가 생겼다는 것이므로 JVM 위에서 실행되는 프로그램이 정상이더라도 결국 실행 불능이 됩니다.<br>
<strong>개발자는 이런 에러에 대처할 방법이 없습니다.</strong><br>
<br>
자바에서는 에러(error) 이외에 <strong>예외(Exception)</strong>라고 부르는 오류가 있습니다.<br>
<strong>예외란 사용자의 잘못된 조작 또는 개발자의 잘못된 코딩으로 인해 발생하는 프로그램 오류</strong>입니다.<br>
<br>
예외 발생시 프로그램이 곧바로 종료된다는 점에서 에러와 동일하지만,<br>
예외는 <strong>예외 처리(Exception Handling)를 통해 프로그램을 종료하지 않고 정상 실행 상태를 유지할 수 있습니다.</strong><br>
<br>
<br>

### 예외의 종류
1. 일반 예외(Exception) : 컴파일러 체크 예외 (자바 소스를 컴파일 하는 과정에서 예외 처리 코드가 필요한지 검사하고 없다면 컴파일 오류를 발생)<br>
2. 실행 예외(Runtime Exception) : 컴파일러가 체크하지 않는 예외<br>
<br>
차이는 컴파일 시 예외 처리를 확인하는 것 뿐, 두 가지 예외는 모두 예외 처리가 필요합니다.<br>
<br>
자바는 예외를 클레스로 관리하고<br>
JVM은 프로그램 실행 중 예외가 발생하면 해당 예외 클래스 객체를 생성합니다.<br>
그리고 예외 처리 코드에서 예외 객체를 이용할 수 있게 해줍니다.<br>
(잘 이해가 가시지 않아도 좋습니다. 제 블로그의 Exception 시리즈를 끝까지 읽으신다면 이해할 수 있을 것입니다.)<br>
<br>
모든 예외 클래스는 java.lang.Exception 클래스를 상속 받습니다.<br>
<strong>일반 예외는 Exception 을 상속받지만 Runtime Exception 을 상속받지 않는 클래스들이고,</strong><br>
<strong>실행 예외는 Exception 을 상속받은 Runtime Exception 을 상속받은 클래스들입니다.</strong><br>
JVM은 RuntimeException 을 상속했는지 여부를 보고 실행 예외를 판단합니다.<br>
흔히 아는 RuntimeException 에는 NullPointerException, ClassCastException, NumberFormatException 등이 있습니다.<br>
<br>
<img src="{{ site.baseurl }}/assets/exceptionTree.gif" title="exception" class="exception">
*checked, unchecked는 컴파일러가 체크하는지 안하는지 여부를 의미.<br>
<br>
<br>

## 실행 예외(Runtime Exception)

자바 컴파일러가 체크하지 않는 예외로 개발자의 경험에 의해 예외처리 코드를 삽입해야합니다.<br>
만약 개발자가 실행 예외에 대해 예외 처리 코드를 넣지 않을 경우, 해당 예외가 발생하면 프로그램은 곧바로 종료됩니다.<br>
가장 빈번하게 발생하는 실행 예외들에 대해 알아봅시다.<br>
<br>
### NullPointerException
객체 참조가 없는 상태, 즉 null 값을 갖는 참조 변수로 객체 접근 연산자인 도트(.)를 사용했을 때 발생합니다.<br>
객체가 없는 상태에서 객체를 사용하려 했으니 예외가 발생하는 것입니다.<br>
<br>
### ArrayIndexOutOfBoundsException
배열에서 인덱스 범위를 초과하여 사용할 경우 발생하는 예외입니다.<br>
<br>
### NumberFormatException
문자열 데이터를 숫자로 변경하는 경우, Integer, Double 같은 포장(Wapper) 클래스를 사용합니다.<br>
이 클레스들의 정적 메소드인 parseXXX() 메소드를 이용하면 문자열을 숫자로 변환할 수 있습니다. ( Integer.parceInt(String s) 등)<br>
이 메소드들은 파라미터 값인 문자열이 숫자로 변환할 수 있다면 숫자를 리턴하지만, <br>
숫자로 변환될 수 없는 문자가 포함되어있다면 java.lang.NumberFormatException을 발생시킵니다.<br>
<br>
### ClassCastException
타입 변환(Casting)은 상위 클레스와 하위 클레스 간과 구현 클래스와 인터페이스 간에만 발생합니다.<br>
억지로 타입 변환을 시도할 경우, ClassCastException이 발생합니다.<br>
<br>
예를 들어 Animal 추상 클레스를 Dog와 Cat이 상속하고 있다고 가정해봅시다.<br>

```
//문제 없는 코드
Animal animal = new Dog();
Dog dog = (Dog) animal;

//ClassCastException 발생 코드
Animal animal = new Dog();
Cat cat = (Cat) animal;

//ClassCastException 발생 방지 코드
Animal animal = new Dog();
if(animal instanceof Dog) {
	Dog dog = (Dog) animal;
}else if(animal instanceof Cat){
	Cat cat = (Cat) animal;
}
```
<br>
<br>

## 관련 포스팅
https://dongyyy.github.io/java/2019/04/24/java-Exception.html <br>
https://dongyyy.github.io/java/2019/04/24/java-Exception(2).html <br>
https://dongyyy.github.io/java/2019/04/24/java-Exception(3).html <br>