---
layout: post
title:  "예외 (Exception)"
date:   2019-04-24 20:14:20
author: Dongy
categories: Java
cover:
---

# 예외의 개념과 실행 예외

Java의 예외를 살펴보기 전에 <br>
런타임(Runtime)과 컴파일타임(Compiletime)의 차이점을 알 필요가 있습니다.<br>
<br>
런타임과 컴파일타임에 대해 가장 쉽게 설명한 글이 있어 퍼왔습니다.(원문은 글 하단 링크 참고)<br>
<br>
`런타임(Runtime)과 컴파일타임(Compiletime)은 소프트웨어 프로그램개발의 서로 다른 두 계층의 차이를 설명하기 위한 용어이다. 프로그램을 생성하기 위해 개발자는 첫째로 소스코드를 작성하고 컴파일이라는 과정을 통해 기계어코드로 변환 되어 실행 가능한 프로그램이 되며, 이러한 편집 과정을 컴파일타임(Compiletime) 이라고 부른다.`<br>
<br>
`컴파일과정을 마친 프로그램은 사용자에 의해 실행되어 지며, 이러한 응용프로그램이 동작되어지는 때를 런타임(Runtime)이라고 부른다.`<br>
<br>
`"런타임"과 "컴파일 타임"이라는 용어는 종종 서로다른 두 가지 타입의 에러를 나타내기 위해 사용되어지곤 하는데, 컴파일 타임 에러는 프로그램이 성공적으로 컴파일링되는 것을 방해하는 신택스에러(Syntax error)나 파일참조 오류와 같은 문제를 말하며, 이런 경우 컴파일러는 컴파일 타임 에러를 발생시키고 일반적으로 문제를 일으킨 소스코드 라인을 지시해준다.`<br>
<br>
`만약, 어떤 소스코드가 이미 실행가능한 프로그램으로 컴파일 되었다 할지라도 이것은 여전히 프로그램의 실행중에 버그를 일으킬 수 있다. 예를 들자면, 예상치 못한 오류 또는 충돌로 동작하지 않을 수 있는데 이렇게 프로그램이 실행중에 발생하는 형태의 오류를 런타임오류 라고 한다.`<br>
<br>
<strong>컴파일타임 오류의 유형</strong><br>
신택스 오류<br>
타입체크 오류<br>
<br>
<strong>런타임 오류의 유형</strong><br>
0나누기 오류<br>
널(Null)참조 오류<br>
메모리 부족 오류<br>
<br>
## 예외와 예외 클레스

<strong>컴퓨터 하드웨어의 오동작 또는 고장으로 인해 응용프로그램 실행 오류가 발생하는 것</strong>을<br>
자바에서는 <strong>에러(error)</strong>라고 합니다.<br>
에러는 JVM 실행에 문제가 생겼다는 것이므로 JVM 위에서 실행되는 프로그램이 정상이더라도 결국 실행 불능이 됩니다.<br>
<strong>개발자는 이런 에러에 대처할 방법이 없습니다.</strong><br>
<br>
자바에서는 에러(error) 이외에 <strong>예외(Exception)</strong>라고 부르는 오류가 있습니다.<br>
예외란 <strong>사용자의 잘못된 조작 또는 개발자의 잘못된 코딩으로 인해 발생하는 프로그램 상의 오류</strong>입니다.<br>
<br>
예외 발생시 프로그램이 곧바로 종료된다는 점에서 에러와 동일하지만,<br>
예외는 <strong>예외 처리(Exception Handling)를 통해 프로그램을 종료하지 않고 정상 실행 상태를 유지할 수 있습니다.</strong><br>
예외를 처리하는 자세한 방법은 다음 포스팅에서 살펴보도록 하겠습니다.<br>
(예외 (Exception) (2) - https://dongyyy.github.io/java/2019/04/24/java-Exception(2).html)<br>
<br>

### 예외의 종류
-<strong>일반 예외(Exception)</strong> : 컴파일러 체크 예외 (자바 소스를 컴파일 하는 과정에서 예외 처리 코드가 필요한지 검사하고 없다면 컴파일 오류를 발생)<br>
-<strong>실행 예외(Runtime Exception)</strong> : 컴파일러가 체크하지 않는 예외<br>
<br>
<br>
두 예외의 차이는 컴파일 시 예외 처리 확인 여부일 뿐, 두 가지 예외는 모두 예외 처리가 필요합니다.<br>
<br>
자바는 예외를 클레스로 관리하고<br>
JVM은 프로그램 실행 중 예외가 발생하면 해당 예외 클래스 객체를 생성합니다.<br>
그리고 예외 처리 코드에서 예외 객체를 이용할 수 있게 해줍니다.<br>
(지금은 잘 이해가 가지 않아도 제 블로그의 Exception 시리즈를 끝까지 읽으신다면 이해할 수 있을 것입니다.)<br>
<br>
모든 예외 클래스는 <strong>java.lang.Exception 클래스</strong>를 상속 받습니다.<br>
<strong>일반 예외</strong>는 Exception 을 상속받지만 <strong>Runtime Exception 을 상속받지 않는 클래스</strong>들이고,<br>
<strong>실행 예외</strong>는 <strong>Runtime Exception 을 상속받은 클래스</strong>들입니다.</strong><br>
JVM은 RuntimeException 을 상속했는지 여부를 보고 실행 예외를 판단합니다.<br>
흔히 아는 RuntimeException 에는 NullPointerException, ClassCastException, NumberFormatException 등이 있습니다.<br>
<br>
<br>
<img src="{{ site.baseurl }}/assets/exceptionTree.gif" title="exception" class="exception">
*checked, unchecked는 컴파일러가 체크하는지 안하는지 여부를 의미.<br>
<br>
<br>
<br>

## 실행 예외(Runtime Exception)

<strong>자바 컴파일러가 체크하지 않는 예외</strong>로 개발자의 경험에 의해 예외처리 코드를 삽입해야합니다.<br>
만약 개발자가 실행 예외에 대해 <strong>예외 처리 코드를 넣지 않을 경우,<br> 
해당 예외가 발생하면 프로그램은 곧바로 종료</strong>됩니다.<br>
아래는 빈번하게 발생하는 실행 예외들입니다.<br>
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
이 클레스들의 정적 메소드인 parseXXX() 메소드를 이용하면 문자열을 숫자로 변환할 수 있습니다.<br>
(Integer.parceInt(String s) 등등)<br>
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


### 참고
이것이 자바다<br>
https://spaghetti-code.tistory.com/35 - 런타임이란? 컴파일타임 과의 차이는?<br>
<br>
### 관련 포스팅
https://dongyyy.github.io/java/2019/04/24/java-Exception.html - 예외의 개념과 실행 예외<br>
https://dongyyy.github.io/java/2019/04/24/java-Exception(2).html - 예외 처리 코드<br>
https://dongyyy.github.io/java/2019/04/24/java-Exception(3).html - 예외 떠넘기기, 사용자 정의 예외<br>