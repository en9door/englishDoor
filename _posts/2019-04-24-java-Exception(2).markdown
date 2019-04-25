---
layout: post
title:  "예외 (Exception) (2)"
date:   2019-04-24 20:14:22
author: Dongy
categories: Java
cover:
---


# 예외 처리 코드

프로그램에서 예외가 발생했을 경우, <br>
프로그램의 갑작스러운 종료를 막고 정상 실행을 유지할수 있도록 처리하는 코드를 <strong>예외 처리 코드</strong>라고 합니다.<br>
<br>
예외 처리 코드는 <strong>생성자 내부와 메소드 내부에 try-catch-finally 블록</strong>을 작성합니다.<br>
<br>
try 블록에는 예외 발생 가능 코드가 위치합니다. <br>
try 블록 코드가 예외 발생 없이 정상 실행되면, <br>
catch 블록의 코드는 실행되지 않고 finally 블록으로 코드를 실행합니다.<br>
만약 try 블록의 코드에서 예외가 발생하면,<br>
즉시 실행을 멈추고 catch 블록으로 이동하여 예외 처리 코드를 실행한 후, finally 블록의 코드를 실행합니다.<br>

```
public class TryCatchFinallyExample {
	public static void main(String[] args) {
		try {
            // String2라는 class 가 없으므로 ClassNotFoundException(일반 예외) 발생
			Class clazz = Class.forName(“java.lang.String2”); 
		} catch(ClassNotFoundException e) {
			System.out.println(“클래스가 존재하지 않습니다.”);
		} finally{
			System.out.println(“끝”);
		}
	}
}
```
<br>
<br>

## 예외 종류에 따른 처리 코드

### 다중 catch
try 블록 내부는 다양한 종류의 예외가 발생할 수 있습니다.<br>
이 경우, 발생되는 예외별로 예외 처리 코드를 다르게 하려면 다중 catch 블록을 작성하여 처리하면 됩니다.<br>
<br>
catch 블록의 예외 클래스 타입은 try 블록에서 발생된 예외의 종류를 말하는데, <br>
try 블록에서 해당 타입의 예외가 발생하면 catch 블록을 실행하도록 되어있습니다.<br>

```
try {
	//ArrayIndexOutOfBoundsException 발생 가능한 코드
	//NumberFormatException 발생 가능한 코드

} catch(ArrayIndexOutOfBoundsException e) {
	//ArrayIndexOutOfBoundsException 발생시 실행

} catch(NumberFormatException e) {
	//NumberFormatException 발생시 실행
}
```
catch 블록이 여러 개라 할지라도 단 하나의 catch 블록만 실행됩니다. <br>
그 이유는 try 블록에서 동시 다발적으로 예외가 발생하지 않고, 하나의 예외가 발생하면 즉시 실행을 멈추고<br>
해당 catch 블록으로 이동하기 때문입니다.<br>


### catch 순서
다중 catch 블록을 작성할 때 주의할 점은 상위 예외 클래스가 하위 예외 클래스보다 아래쪽에 위치해야합니다.
```
try {
	//ArrayIndexOutOfBoundsException 발생 가능한 코드
	//NumberFormatException 발생 가능한 코드

} catch(Exception e) {
	//ArrayIndexOutOfBoundsException 과 NumberFormatException 발생시 실행

} catch(ArrayIndexOutOfBoundsException e) {
	//ArrayIndexOutOfBoundsException 발생시 실행
}
```
ArrayIndexOutOfBoundsException 과 NumberFormatException 은<br>
모두 Exception 을 상속받기 때문에 첫 번째 catch 블록만 선택되어 실행됩니다.<br>
두 번째 catch 블록은 어떤 경우에라도 실행되지 않습니다.<br>
따라서 다음과 같이 수정해야합니다.<br>
<br>

```
try {
	//ArrayIndexOutOfBoundsException 발생 가능한 코드
	//다른 Exception 발생 가능한 코드

} catch(ArrayIndexOutOfBoundsException e) {
	//ArrayIndexOutOfBoundsException 발생시 실행

}catch(Exception e) {
	//다른 Exception 발생시 실행
}
```
<br>

### 멀티 catch
자바 7부터 하나의 catch 블록에서 여려개의 예외를 처리할 수 있도록 멀티 catch 기능을 추가했습니다.<br>
다음은 멀티 catch 블록을 작성하는 방법을 보여줍니다.<br>

```
try {
	//ArrayIndexOutOfBoundsException 또는 NumberFormatException 발생 가능한 코드
	//다른 Exception 발생 가능한 코드

} catch(ArrayIndexOutOfBoundsException : NumberFormatException e) {
	//ArrayIndexOutOfBoundsException 또는 NumberFormatException 발생시 실행

}catch(Exception e) {
	//다른 Exception 발생시 실행
}
```
<br>

### 자동 리소스 닫기
자바 7에서 새로 추가된 try-with-resources 를 사용하면 <br>
예외 발생 여부와 상관없이 사용했던 리소스 객체(각종 입출력스트림, 서버 소켓, 소켓, 각종 채널)의 close() 메소드를 호출해서 안전하게 리소스를 해제해줍니다.<br>
(여기서 리소스는 데이터를 읽고 쓰는 객체 정도로 생각 ex)FileInputStream, FileOutputStream 이 리소스 객체)<br>

```
//기존 소스
FileInputStream fis = null;
try {
	fis = new FileInputStream(“file.txt”);
	…
} catch(IOException e) {
	…
} finally {
	if(fis != null){
		try {
			fis.close();
		} catch (IOException e) { }
	}
}
```
finally 블록에서 다시 try-catch 를 사용해서 close() 메소드를 예외 처리해야하므로 복잡합니다.<br>
자바 7의 try-with-resources 를 사용하여 간단히 할 수 있습니다.<br>
close() 를 명시적으로 호출하지 않습니다.<br>
<br>
```
try(FileInputStream fis = new FileInputStream(“file.txt”)){
	//…
} catch(IOException e) {
	//…
}
```
try 블록이 정상적으로 실행을 완료했거나 도중에 예외가 발생하게 되면 자동으로 fileInputStream의 close() 메소드가 호출됩니다.<br>
try {} 에서 예외가 발생하면 우선 close() 로 리소스를 닫고 catch 블록을 실행합니다.<br>
만약 복수 개의 리소스를 사용해야 한다면 다음과 같이 작성할 수 있습니다.<br>
<br>
```
try(
	FileInputStream fis = new FileInputStream(“file1.txt”)
	FileInputStream fos = new FileOutputStream(“file2.txt”)

){
	//…
} catch(IOException e) {
	//…
}
```
try-with-resources 를 사용하기 위해서는 조건이 있는데,<br>
<strong>리소스 객체는 java.lang.AutoCloseable 인터페이스를 구현</strong>하고 있어야 합니다.<br>
<strong>AutoCloseable 인터페이스에는 close() 메소드가 정의되어 있는데 try-with-resources 는 바로 이 close() 메소드를 자동 호출</strong>한다.<br>
API 도큐먼트에서 AutoCloseable 인터페이스를 찾아 “All Known Implementing Classes:” 를 보면 try-with-resources 와 함께 사용할 수 있는 리소스가 어떤 것이 있는지 알 수 있습니다.<br>
<br>
<br>


### 참고
이것이 자바다
<br>
### 관련 포스팅
https://dongyyy.github.io/java/2019/04/24/java-Exception.html - 예외의 개념과 실행 예외<br>
https://dongyyy.github.io/java/2019/04/24/java-Exception(2).html - 예외 처리 코드<br>
https://dongyyy.github.io/java/2019/04/24/java-Exception(3).html - 예외 떠넘기기, 사용자 정의 예외<br>