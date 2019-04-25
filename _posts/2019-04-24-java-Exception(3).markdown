---
layout: post
title:  "예외 (Exception) (3)"
date:   2019-04-24 20:14:24
author: Dongy
categories: Java
cover:
---

# 예외 떠넘기기와 사용자 정의 예외

## 예외 떠넘기기 (throws 키워드)
메소드 내부에서 예외가 발행할 수 있는 코드를 작성할 때 <br>
try-catch 블록으로 예외를 처리하는 것이 기본이지만, <br>
경우에 따라서는 <strong>메소드를 호출한 곳으로 예외를 떠넘길 수 있습니다.</strong><br>
<br>
이때 사용하는 키워드가 <strong>throws</strong> 입니다. <br>
throws 키워드는 <strong>메소드 선언부 끝에 작성되어 메소드에서 처리하지 않은 예외를 호출한 곳으로 떠넘기는 역할</strong>을 합니다.<br>
throws 키워드 뒤에는 떠넘길 예외 클래스를 쉽표로 구분해서 나열해주면 됩니다.<br>
```
리턴타입 메소드명 (매개변수, …) throws 예외클래스1, 예외클래스2, …. {
}
```
<br>
발생할 수 있는 예외의 종류별로 throws 뒤에 나열하는 것이 일반적이지만, throws Exception만으로 모든 예외를 떠넘길 수도 있습니다.
```
리턴타입 메소드명 (매개변수, …) throws Exception {
}
```
또한 throws 키워드가 붙어있는 메소드는 반드시 try 블록 내에서 호출되어야 합니다.<br>
그리고 catch 블록에서 떠넘겨 받은 예외를 처리해야 합니다.
```
public void method1() {
	try {
		method2();
	} catch(ClassNotFoundException e) {
		//예외 처리 코드
		//System.out.println(“클래스가 존재하지 않습니다.”);
	}
}

public void method2() throws ClassNotFoundException {
	Class clazz = Class.forName(“java.lang.String2”);
}
```
<br>
자바 API 도큐먼트에서 클래스 생성자와 메소드 선언부에 throws 키워드가 붙은 것을 흔히 볼 수 있습니다.<br>
이러한 생성자와 메소드를 사용하고 싶다면, 반드시 try-catch 블록으로 예외 처리를 해야합니다.<br>
아니면 throws를 다시 사용해서 예외를 호출한 곳으로 떠넘겨야 합니다.<br>
그렇지 않으면 컴파일 오류가 발생합니다.<br>
<br>



## 사용자 정의 예외와 예외 발생
표준 API에서 제공하는 예외 클레스만으로는 다양한 종류의 예외를 표현할 수 없습니다.<br>
<br>
예를 들어 은행 업무 프로그램에서 잔고보다 더 많은 출금 요청이 들어왔을 경우, <br>
프로그램은 잔고 부족 예외를 발생시켜야 합니다.<br>
하지만 잔고 부족 예외는 자바 표준 API에 존재하지 않습니다.<br>
잔고 부족 예외와 같이 <strong>애플리케이션 서비스와 관련된 예외를 애플리케이션 예외</strong>라고 합니다.<br>
애플리케이션 예외는 <strong>개발자가 직접 정의해서 만들어야 하므로 사용자 정의 예외</strong>라고도 합니다.<br>
<br>
### 사용자 정의 예외 클래스 선언
사용자 정의 예외 클래스는 일반 예외와 실행 예외로 선언할 수 있는데<br>
일반 예외로 선언할 경우 Exception을 상속하면 되고, 실행 예외로 선언할 경우에는 RuntimeException으로 상속하면 됩니다.<br>

```
public class XXXException extends [ Exception | RuntimeException ] {
	public XXXException() { }
	public XXXException(String message) { supur(message); } 
}
```
1. 사용자 정의 예외 클래스 이름은 Exception으로 끝나는 것이 좋습니다.<br>
2. 사용자 정의 예외 클래스에는 대부분 생성자 선언만을 포함합니다.<br>
생성자는 두 개를 선언하는 것이 일반적인데, 하나는 매개 변수가 없는 기본 생성자이고, 다른 하나는 예외 발생 원인(예외 메시지)을 전달하기 위해 String 타입의 매개 변수를 갖는 생성자입니다.<br>
String 타입의 매개 변수를 갖는 생성자는 상위 클래스의 생성자를 호출하여 예외 메시지를 넘겨줍니다.<br>
예외 메시지의 용도는 catch{} 블록의 예외 처리 코드에서 이용하기 위해서입니다.<br>
<br>

다음은 잔고 부족 예외를 사용자 정의 예외 클래스로 선언한 것입니다.<br>
```
//Exception을 상속하여 컴파일러에 의해 체크되는 예외
//따라서 소스 작성시 try-catch 블록으로 예외 처리가 필요
public class BalanceInsufficientException extends Exception {
	public BalanceInsufficientException() { }
	public BalanceInsufficientException(String message) { 
		super(message);
	}
}
```
<br>
## 예외 발생시키기 (throw new 예외 객체 생성자)
사용자 정의 예외 또는 자바 표준 예외를 강제로 발생시키는 방법은 다음과 같습니다.
```
throw new XXXException();

//catch 블록에서 예외 메시지가 필요하다면 예외 메시지를 갖는 생성자 이용
throw new XXXException(“메시지”); 
```
<br>
예외 발생 코드를 가지고 있는 메소드는 내부에서 try-catch 블록으로 예외를 처리할 수 있지만, <br>
대부분은 자신을 호출한 곳에서 예외를 처리하도록 throws 키워드로 예외를 떠넘깁니다.

```
public void method() throws XXXException {
	throw new XXXException(“메시지”);
}
```
<br>
throws 키워드를 포함한 메소드는 호출한 곳에서 예외처리를 해줘야합니다.
```
try {
	method();
} catch(XXXException e){
	//예외 처리
}
```
<br>
<br>
다음 예제는 은행 계좌(Account) 클래스입니다.<br>
출금(withdraw) 메소드에서 잔고(balance) 필드와 출금액을 비교해서 잔고가 부족하다면<br>
BalanceInsufficientException을 발생시킵니다.
```
public class Account {
	private long balance;
	
	public Account() { }
	
	public long getBalance() {
		return balance;
	}
	
	public void deposit(int money) {
		balance += money;
	}

    //사용자 정의 예외 떠넘기기 throws BalanceInsufficientException 
	public void withdraw(int money) throws BalanceInsufficientException { 
		if(balance < money) {
            //사용자 정의 예외 발생
			throw new BalanceInsufficientException(“잔고부족”+(meney-balance)+” 모자람”); 
		}	
		
		balance =- money;
	}
}
```
<br>
<br>
## 예외 정보 얻기 (getMessage() 와 printStackTrace() 메소드)
try 블록에서 예외가 발생되면 예외 객체는 catch 블록의 매개 변수에서 참조하게 되므로 <br>
매개 변수를 이용하면 <strong>예외 객체의 정보</strong>를 알 수 있습니다.<br>
<br>
이 때 사용하는 메소드가 <strong>getMessage()</strong> 와 <strong>printStackTrace()</strong> 입니다.<br>
예외를 발생시킬 때 다음과 같이 String 타입의 메시지를 갖는 생성자를 이용했다면, 메시지는 자동적으로 예외 객체 내부에 저장됩니다.<br>
그리고 getMessage() 메소드를 통해 꺼낼 수 있습니다.<br>

```
throw new XXXException(“예외 메시지”);

}catch(Exception e) {
	String message = e.getMessage();
	System.out.println(message);
    //예외 메시지 라고 콘솔에 출력
}
```

또 printStackTrace()는 메소드 이름과 같이 예외 발생 코드를 추적해서 모두 콘솔에 출력합니다.<br>
어떤 예외가 어디서 발생했는지 상세하게 출력해주기 때문에 유용하게 활용할 수 있습니다.
```
}catch(Exception e) {
	e.printStackTrace();
}
```
<br>
다음 AccountExample 클래스는 Account 클레스를 이용해서 출금을 합니다.<br>
출금할 때 withdraw() 메소드를 사용하므로 예외 처리가 꼭 필요합니다.<br>
(withdraw() 메소드는 throws BalanceInsufficientException를 이용하여 사용자 정의 예외 떠넘기기를 사용했으므로 꼭 예외처리가 필요)<br>
예외 처리 코드에서 BalanceInsufficientException 객체의 getMessage() 메소드와 printStackTrace() 메소드로 예외에 대한 정보를 얻어내고 있습니다.

```
public class AccountExample {
	public static void main(String[] args) {
		Account account = new Account();
		//예금하기
		account.deposit(10000);
	
		//출금하기
		try {
			account.withdraw(30000);
		} catch(BalanceInsufficientException e) { //예외 메시지 얻기 
			String message = e.getMessage();
			System.out.println(message); //예외 추적 후 출력 (잔고부족: 20000)
			System.out.println();
			e.printStackTrace(); 
            // Account.java *라인에서 최초의 예외발생하여 AccountExample.java *라인의 메소드 호출 위치로 예외가 떠넘겨졌음을 알 수 있습니다.
		}
	}
}
```

## 참고
이것이 자바다

## 관련 포스팅
https://dongyyy.github.io/java/2019/04/24/java-Exception.html - 예외의 개념과 실행 예외<br>
https://dongyyy.github.io/java/2019/04/24/java-Exception(2).html - 예외 처리 코드<br>
https://dongyyy.github.io/java/2019/04/24/java-Exception(3).html - 예외 떠넘기기, 사용자 정의 예외<br>