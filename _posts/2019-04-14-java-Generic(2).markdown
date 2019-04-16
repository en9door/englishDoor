---
layout: post
title:  "제네릭 (Generic) (2)"
date:   2019-04-14 21:20:11
author: Dongy
categories: Java
cover:
---


## 제네릭 사용 범위

<strong>1. 클래스(생성자), 인터페이스</strong><br>
- 타입을 파라미터로 가지는 클레스와 인터페이스를 <strong>제네릭 타입</strong>이라고 합니다.<br>
- 클래스 또는 인터페이스 이름 뒤에 붙는 “<>” 사이에 T를 <strong>타입 파라미터</strong>라고 합니다.<br>
- 클래스 레벨에서 제네릭이 설정되어있으면 static 메서드에서는 사용할 수 없습니다.<br>
(인스턴스가 만들어질 때 type parameter를 받아오기 때문입니다.)<br>

<strong>2. 메서드</strong><br>
- <strong>파라미터, 리턴 타입</strong>으로 사용합니다.<br>
- static, instance 메소드에 사용할 수 있습니다. <br>
(인스턴스 생성할 떄 type parameter를 받아와 타입이 설정되기 때문입니다.)<br>
<br>

아래에서 구체적인 사용 방식을 살펴보겠습니다.<br>
<br>
<br>
<br>

## 제네릭 사용 방식

### 1. Class generic type, Interface generic type
Class generic type과 Interface generic type은 사용방법이 유사합니다.<br>
<br>
아래 소스와 같이 Class generic type을 사용합니다.
```
//구현
class ClassGenericType<T> {
        private T t;
 
        public void set(T t) {
               this.t = t;
        }
        public T get() {
               return t;
        }
}

//사용방법
ClassGenericType<String> classGenericType = new ClassGenericType<>();
```
<br>


### 2. multi type parameter
멀티 타입 파라미터는 <strong>2개 이상의 제네릭 타입을 정의</strong>한 것입니다. <br>
이 경우 각 타입 파라미터를 콤마로 구분하여 사용합니다.<br>
<br>
아래 코드에서는 <strong><T1, T2>와 같이 제너릭 타입을 두 개 선언한 멀티 타입 파라미터</strong>를 사용하였습니다.

```
interface InterfaceGenericType<T1, T2> {
	public T1 doSomething(T2 t);
        public T2 doSomething2(T1 t);
}


//InterfaceGenericType 인터페이스 구현 클래스
class InterfaceGenericTypeImpl implements InterfaceGenericType<String, Integer> {
        @Override
        public String doSomething(Integer t) {
               return null;
        }
 
        @Override
        public Integer doSomething2(String t) {
               return null;
        }
}
```
<br>


### 3. Method Generic Type
제네릭 메소드는 <strong>파라미터 타입, 리턴 타입으로 타입 파라미터를 갖는 메소드</strong>를 말합니다.<br>
<br>
선언 방법은 메서드의 <strong>리턴 타입 앞에 <> 기호를 추가</strong>하고 타입 파라미터를 기술한 다음,<br>
리턴 타입과 파라미터 타입으로 타입 파라미터를 사용하면 됩니다.

```
public class Util {
        public static <T> Box<T> boxing(T t) {
		Box<T> box = new Box<T>();
	        box.set(t);
		return box;
        }
}
```
<br>
<strong>호출방법</strong><br>

1. 명시적으로 구체적 타입을 지정
리턴타입 변수 = <구체적타입> 메소드명(파라미터값);
2. 매개값을 보고 컴파일러가 구체적 타입을 추정 
리턴타입 변수 = 메소드명(파라미터값); 			  

```
//1. 타입 파라미터를 명시적으로 Integer로 지정
Box&lt;Integer&gt; box = &lt;Integer&gt;boxing(100); 
//2. 타입 파라미터를 컴파일러가 Integer로 추정
Box<Integer> box = boxing(100);			
```
<br>





### 4. Bounded Type Parameter (제한된 타입 파라미터)
원래 <strong>타입 파라미터</strong>(“<>” 사이에 들어갈 type)에는 어떤 타입이든 지정할 수 있지만,<br>
extends 키워드를 이용하여 <strong>구체적인 타입들만 들어갈 수 있도록 제한</strong>할 수 있습니다.<br>
<br>
<br>
<strong>사용방법</strong><br>
제한된 타입 파라미터를 선언하려면, 타입 파라미터 뒤에 extends 키워드를 붙이고 상위 타입을 명시하면 됩니다.<br>
아래 소스는 Number 클레스 타입과 Number 클레스의 하위 클래스 타입(Byte, Short, Integer, Long, Double)만 타입 파라미터로 지정할 수 있도록 제한하고 있습니다.,<br>
```
//public <T extends 상위타입> 리턴타입 메소드이름(매개변수, …) { … }
public <T extends Number> int compare(T t1, T t2){ //… }
```
<br>
<strong>주의할 점</strong><br>
- extends 뒤에 올 타입은 클래스뿐만 아니라 인터페이스도 가능하지만 인터페이스라고해서 Implements를 사용하지 않습니다.<br>
- 메소드의 중괄호 {} 안에서 타입 파라미터 변수로 사용 가능한 것은 extends 뒤에 올 타입의 맴버 (필드, 메소드)로 제한됩니다.<br>
즉, 하위 타입에만 있는 필드와 메소드는 사용할 수 없습니다.<br>
자세한 내용은 아래 소스를 참고하세요.


```
public class Util {
	public <T extends Number> int compare(T t1, T t2){
		double v1 = t1.doubleValue(); // Number의 doubleValue() 메소드 사용
		double v2 = t2.doubleValue(); // Number의 doubleValue() 메소드 사용
		return Double.compare(v1, v2); //v1과 v2를 비교하여 첫번째 매개값이 작으면 -1, 같으면 0, 크면 1을 리턴
	}
}

public class BoundedTypeParameterExample {
	public static void main(String[] args){
		//String str = Util.compare(“a”, “b”); //(x) String은 Number의 하위 클레스 타입 아님
		int result1 = Util.compare(10, 20); //int -> Integer 자동 Boxing (Integer는 Number의 하위 클레스 타입)
		int result2 = Util.compare(4.5, 3); //double -> Double 자동 Boxing (Double은 Number의 하위 클레스 타입)
	}
}
```
<br>


### 5. Wildcard Generic Type
스포츠에서 와일드 카드 (wild card) 는 정규규칙에 의해 통과하지 못한 선수나 팀에 자격을 주는 것을 뜻하는 말로, <br>
'아무렇게나 쓸 수 있는' 이라는 의미에서 나온 말입니다.<br>
<br>
코드에서 <strong>? 를 일반적으로 와일드 카드 (wild card)</strong> 라고 부릅니다.<br>
<br>
제네릭 타입에 와일드카드를 다음과 같이 <strong>세 가지 형태</strong>로 사용할 수 있습니다.<br>
- <strong><?></strong> : 모든 클레스나 인터페이스 타입이 올 수 있습니다. 내부적으로는 Object 로 인식합니다.<br>
- <strong><? extends 객체자료형></strong> : 명시된 클레스나 인터페이스 타입 또는 이를 상속한 하위 타입만 올 수 있습니다.<br>
- <strong><? super 객체자료형></strong> : 명시된 클레스나 인터페이스 타입 또는 이의 상위 타입만 올 수 있습니다.<br>
<br>
예를 들면, A > B > C > D 순으로 상속된 class 들이 있을 때,<br>
<? super B> 가 의미하는 class 는 B, A class 이고<br>
<? extends C> 가 의미하는 class 는 C, D class 입니다.<br>
<br>
제네릭 타입과 와일드 카드 어떻게 활용할 수 있는지 아래 소스를 통해 살펴보겠습니다.<br>
<br>
<strong>WildCardGeneric.java</strong><br>

```
public class WildCardGeneric<T> {
    T wildCard;

    public void setWildCard(T wildCard){
        this.wildCard = wildCard;
    }

    public T getWildCard(){
        return wildCard;
    }
}
```
<br>

WildCardGeneric 클래스를 사용하는 <strong>WildCardSample.java</strong><br>

```
public class WildCardSample {
    public static void main(String[] ar){
        WildCardSample ex = new WildCardSample();
        ex.callWildCardMethod();
    }

    public void callWildCardMethod(){
        //1
        WildCardGeneric<Number> wildcard = new WildCardGeneric<>();
        wildcard.setWildCard(1.2); //Double type
        //2
        wildcardStringMethod(wildcard); // print 1.2

        //3
        WildCardGeneric<Integer> wildcard2 = new WildCardGeneric<>();
        wildcard2.setWildCard(777);
        //4
        // wildcardStringMethod(wildcard2); // (X) WildCardGeneric<Number> 타입 파라미터만 받을 수 있음
        wildcardStringMethod2(wildcard2); // print 777

        wildcardStringMethod2(wildcard); // print “A”
    }

    //4
    public void wildcardStringMethod(WildCardGeneric<Number> c){
        Number value = c.getWildCard();
        System.out.println(value);
    }

    //5
    public void wildcardStringMethod2(WildCardGeneric<?> c){
        Object value = c.getWildCard();
        System.out.println(value);
    }
}
```
<br>
<br>
일단 <strong>callWildCardMethod()</strong> 를 보면<br>
1.WildCardGeneric 클래스의 타입 파라미터에 Number 타입을 넣어 wildcard 인스턴스를 생성했습니다.<br>
( WildCardGeneric<Number> wildcard = new WildCardGeneric<>(); )<br>
<br>
2.그리고 wildcardStringMethod(WildCardGeneric<Number> c) 메소드를 호출하여 해당 객체를 파라메터로 넣고 wildCard 값을 출력하고 있습니다.<br>
<br>
3.그런데 또 다른 WildCardGeneric 클래스의 인스턴스 wildcard2의 경우, 타입 파라미터에 Integer 타입을 넣어 인스턴스를 생성했기 때문에<br>
4.wildcardStringMethod() 의 파라미터에 사용할 수 없습니다.<br>
<br>
여기서 <strong>Generic 을 사용시에는 다형성이 허용되지 않는다는 사실</strong> 을 알 수 있습니다.
Number class 는 Integer class 의 부모 class 로 일반적으로는 다형성으로 인해 Number 타입의 변수에 Integer 인스턴스 할당이 가능합니다.<br>
하지만, Generic 사용시에는 다형성이 허용되지 않아 Integer 타입을 허용하지 않습니다.
따라서 <strong>언제나 WildCardGeneric<Number> 타입 파라미터만 허용합니다.</strong><br>
형 안정성을 고려한 정책입니다. ( 자세한 설명은 참고 : https://creator1022.tistory.com/142 )
<br>
그러면 wildcard2 의 wildCard 값을 출력하기 위해서 WildCardGeneric<Integer> 타입의 매개변수를 받는 메소드를 또 만들어야 할까요?<br>
( public void wildcardStringMethod(WildCardGeneric<Integer> c) )<br>
매개변수를 타입마다 메소드를 오버로딩해서 선언해주어야 할까요?
<br>
이럴 때 사용하는게 <strong>wildcard 타입</strong> 입니다. <br>
5.wildcardStringMethod2() 메소드를 보면 매개변수에 WildCardGeneric 클래스의 타입 파라미터를 <?> 라고 썻습니다. <br>
이 ? 에는 어떤 타입도 들어갈 수 있습니다.<br>
<?>는 <? extends Object> 라고 생각할 수 있습니다.<br>
즉, ‘Object 클래스를 상속받는 모든 클래스를 타입으로 사용할 수 있다’ 의 의미입니다.<br>
<br>
<strong>와일드카드는 파라미터로만 사용할 수 있습니다.</strong><br>
예를 들어 WildCardGeneric<?> wildcard = new WildCardGeneric<String>(); 과 같이 변수나 인스턴스에 사용하면 예외가 발생합니다.<br>
(인스턴스가 만들어질 때 type parameter 가 확정되어 있어야하기 때문입니다.)<br>
<br>




### 6.Not Allowed Generic Type
<strong>허용하지 않는 사용법</strong><br>
- static 필드는 제너릭 타입을 가질 수 없습니다.<br>
// private static T t;<br>
<br>
- Generic type은 인스턴스로 생성할 수 없습니다.<br>
```
class NotAllowedGenericType<T> {
 	new T(); //X
}
```
<br>
- 타입 파라미터로 배열을 생성하려면 new T[n] 형태로 배열을 생성할 수 없고, (T[])(new Object[n]) 으로 생성해야합니다.<br>

```
class NotAllowedGenericType<T> {
 	private T[] array = (T[]) (new Object[원하는크기]);
}
 ```

<br>
- primitives 타입으로 제너릭 타입을 선언할 수 없습니다.<br>

 ```
List<int> list = new ArrayList<>(); //(X, 애러)
 ```



### 제네릭 참고

1. Number class는 Integer class의 부모 클레스지만 List<Number>와 List<Integer>는 아무 관계가 아닙니다.<br>
- <strong>Generic 을 사용시에는 다형성이 허용되지 않습니다.</strong> (아래 면접에서 받았던 질문 항목 참고)<br>
<br>

2. 인스턴스 생성할 때 타입 인자를 2 번 주지않아도 타입을 추론합니다.(<strong>다이아몬드 연산자</strong>)<br>
- List<Integer> list = new ArrayList&lt;Integer&gt;(); : java6까지<br>
- List<Integer> list = new ArrayList<>(); : java7부터 가능<br>
<br>

3. Collections.emptyList() 를 호출시, <br>
Collections class 의 static 메서드 emptyList() 를 사용할 때, 인자로 넘기는 것 없는데 <T> 타입추론은 어떻게하는가?<br>
- List<T> list <- Collections.emptyList() 리턴되는 리스트가 사용되면서 타입추론합니다.<br>
- 명시적으로 메서드 앞에 <타입> 타입 파라미터를 넘겨줄 수도 있습니다<br>
<br>

4. 재네릭 타입도 부모 클래스가 될 수 있다. 즉, 상속이 가능하다.<br>
- class MyList<T> implements List<T>{…} 와 같이 상속 가능합니다.<br>
- class MyList<T, S, U, V> implements List<T, M>{…} 와 같이 확장도 가능합니다.<br>
- class MyList<M> implements List<T>{…} 와 같이 상위 타입 무시하고 확장은 불가능합니다.<br>
<br>

5.제네릭 타입의 이름을 짓는 강제성 없는 규칙이 있습니다.(보는 사람들의 이해를 돕기위해)<br>
E - Element(요소)<br>
K - Key(키)<br>
N - Number(숫자)<br>
T - Type(타입)<br>
S, U, V - 두번째, 세번째, 네번째에 선언된 타입<br>
V - Value(값)<br>
<br>
<br>


### 궁금한 점

1. 같은 결과를 내는 Generic Method와 Wildcard의 차이는 무엇인가?<br>
```
//Generic Method
public <T extends Number> void Method(Collection<T> parameter) {...}
//Wildcard
public void Method(Collection<? extends Number> parameter) { … } 
```
보통 Generic Method는 특정 타입 파라미터 T를 메서드 파라미터와 리턴값에 모두 사용할 때 사용하면 좋습니다.<br>
하지만 리턴값이 없을 때는 Wildcard 를 사용하면 메서드 시그니처가 훨신 간단해 집니다.<br>

```
//Generic Method
public static <?> void compact(List<?> list) 
//Wildcard
public static void compact(List<?> list)
```
반환값이 없기 때문에 타입 추론의 장점도 없고, Wildcard 쪽이 의도가 훨씬 명확합니다.<br>
결국 타입 파라미터를 선언하고 2회 이상 쓸 일이 없을 경우, 시그니처 단순화의 관점에서 타입 파라미터를 선언하지 않는 와일드카드가 유용하다고 볼 수 있지 않을까 합니다.<br>
참고: https://www.slipp.net/questions/202<br>




### 면접에서 받았던 Generic 관련 질문
(기억을 더듬어 재구성 하였습니다.)<br>
<br>
1.아래와 같은 소스가 유효할까?<br>
```
List<String> stringList = new ArrayList<>();
List<Object> objectList = stringList;
```
답은 “유효하지 않다.” 입니다.<br>
<br>
위와 같은 코드는 다음과 같이 타입 애러가 발생합니다.<br>
//Incompatible types.<br>
//Required: ArrayList&lt;java.lang.Object&gt;<br>
//Found:      ArrayList&lt;java.lang.String&gt;<br>
<br>
면접에서 질문을 들었을 때는 Object는 모든 Java 클레스들의 부모 클레스 이므로 <br>
가능한 문법이 아닐까라고 생각했지만, <strong>Generic 을 사용시에는 다형성이 허용되지 않습니다.</strong><br>
애러 문구를 통해서 stringList의 타입은 그냥 String 타입이 아니라 ArrayList&lt;java.lang.String&gt; 이고 <br>
objectList의 타입은 그냥 Object 타입이 아니라 ArrayList&lt;java.lang.Object&gt; 라서<br>
유효하지 않은 문법입을 확인했습니다.<br>
<br>
<br>

2.Object > Animal > Dog 순으로 상속한다고 할때,<br>
Class<? super Animal>  일 떄, Cass <Dog> class = new Class<>(); 이 유효한가?<br>
<br>
- “답은 유효하지 않다.” 입니다. <br>
Class<? super Animal>는 Animal과 Object Class의 인스턴스를 파라미터로 받습니다.<br>
<br>

Class <? extends Object> class 일 떄, Class <Animal> class = new Class<>(); 이 유효한가?<br>
<br>
- “답은 유효하다.” 입니다. <br>
Class <? extends Object>는 Object와 그의 하위 클래스 즉,  Object, Animal, Dog 인스턴스를 모두 파라미터로 받습니다.<br>




## 관련 포스팅
https://dongyyy.github.io/java/2019/04/14/java-Generic.html <br>
https://dongyyy.github.io/java/2019/04/14/java-Generic(2).html
