---
layout: post
title:  "Java Comparable, Comparator Interface (1)"
date:   2019-02-09 22:42:00
author: Dongy
categories: Java
cover:  "/assets/the_red_sea.jpg"
tags:	Java Comparable Comparator
---
<br>
Java로 개발을 하면서 배열이나 List 등의 Collection 프레임워크를 Arrays.sort(), Collections.sort()를 이용하여 정렬해본 경험이 있을겁니다.<br>
<br>
Arrays.sort(), Collections.sort() 메소드가 Merge Sort, Insertion Sort, QuickSort 등 <strong>정렬 기법</strong>들을 구현하고 있다면,<br>
오름차순으로 정렬할지 내림차순으로 정렬할지에 대한 <strong>정렬 기준은 Comparable, Comparator Interface가 정합니다.</strong><br>
<br>
Java는 정렬의 기준을 정할 수 있도록 Comparable과 Comparator라는 두 가지 Interface를 제공합니다.<br>
<br>
<span style="color:red">Comparable - 기본 정렬(오름차순 정렬)할 때 사용합니다.</span><br>
<span style="color:red">Compatator - 원하는데로 정렬 순서를 지정할 때 사용합니다.</span><br>
<br>
우리가 사용했던 Arrays.sort(), Collections.sort() 가 오름차순 정렬을 해줄 수 있었던 이유는 Integer, String 등의 클레스에서 Comparable 를 구현하고 있으므로 자동으로 오름차순 정렬이 되었던 것이었습니다.<br>
<br>
<br>
<br>
여기까지 읽어도 도통 무슨 소린지 이해가 안가더라도 괜찮습니다.<br>
이해를 돕기 위해 Arrays.sort(), Collections.sort() 예제를 확인해보죠<br>
### Code

```
	//Arrays.sort() 예제

	int array[] = {4,3,5,6,-1,-2};
	String array2[] = {"기아", "포르쉐", "현대", "벤츠", "도요타", "폭스바겐"};
	
	//정렬
	Arrays.sort(array);
	Arrays.sort(array2);
			
	//array[] 출력		
	int length = array.length;
	for(int i = 0; i < length ; i++){
		System.out.print(array[i] + " ");  // 결과는 -2 -1 3 4 5 6로 오름차순 정렬
	}
			
	System.out.println();

	//array2[] 출력			
	int length2 = array2.length;
	for(int i = 0; i < length ; i++){
		System.out.print(array2[i] + " ");  // 결과는 기아 도요타 벤츠 포르쉐 폭스바겐 현대로 오름차순 정렬
	}

```
위 예제에서 Arrays.sort(array), Arrays.sort(array2)는 <br>
int 와 String는 내부적으로 <strong>Comparable Interface을 구현하여 오름차순의 기준으로 정렬</strong>할 수 있는 것입니다.<br>
<br>
이것이 <span style="color:red">Comparable 인터페이스는 기본 정렬(오름차순 정렬)할 때 사용한다는 말입니다.</span><br>
<br>
Comparable 을 구현하고 있는 클래스들은 서로 비교할 수 있는 클래스들<br>
예를들면 String, Integer, Date, File 같은 클래스들 입니다.<br>
<br>
<br>
<br>
이번엔 배열이 아닌 List 형태로 sort() 를 구현해보면, <br>
int 타입의 배열 을 List 로만 바꿨습니다.<br>
<br>
List 는 Collection 프레임워크 이므로 Arrays.sort()가 아닌 Collections.sort() 를 이용해야 합니다.<br>
### Code

```

	// Collections.sort() 예제
	List<Integer> list = new LinkedList<>();  
	list.add(4);
	list.add(3);
	list.add(5);
	list.add(6);
	list.add(-1);
	list.add(-2);
			
	Collections.sort(list);
			
	int size = list.size();
	for(int i = 0; i < size ; i++){
		System.out.print(list.get(i) + " "); // 결과는 -2 -1 3 4 5 6로 오름차순 정렬
	}


```
<br>
<br>
<br>
이제부터는 기본 클레스가 아닌 <strong>우리가 만든 클레스로 정렬</strong>을 시도해보겠습니다.<br>
Friend 라는 클래스를 하나 만들고, 객체배열 또는 List 을 이용해 sort를 해보려고 합니다.<br>
### Code

```

class Friend{
		String name;
		int height;
		int weight;

		public Friend(String name, int height, int weight){
			this.name = name;
			this.height = height;
			this.weight = weight;
		}

		public String getName() {
			return name;
		}

		public void setName(String name) {
			this.name = name;
		}

		public int getHeight() {
			return height;
		}

		public void setHeight(int height) {
			this.height = height;
		}

		public int getWeight() {
			return weight;
		}

		public void setWeight(int weight) {
			this.weight = weight;
		}
	}

	public static void main(String[] args) {
		List<Friend> friends = new ArrayList<>(); 
		friends.add(new Friend("김동*", 184, 77));
		friends.add(new Friend("민상*", 190, 70));
		friends.add(new Friend("강성*", 200, 72));
		
		Collections.sort(friends);

		int size = friends.size();
		for(int i = 0; i < size ; i++){
			System.out.print(friends.get(i).name + " "); 
		}
	}

```
<br>
Friend 클래스는 이름, 키, 몸무게 맴버변수를 가지고 있고 각각 setter와 getter를 선언해줬습니다. <br>
Friend 객체를 배열이나 ArrayList 형태로 만들었을 경우, 위 자동차, 숫자 정렬 예제와 동일하게 <br>
Arrays.sort(), Collections.sort() 가 작동할까요?<br>
<br>
오류가 발생합니다.<br>
<strong>이유는 Friend 클래스가 Comparable Interface 를 구현하지 않아 어떤 기준으로 정렬할지 정하지 않았기 때문입니다.</strong><br>
Comparable Interface 를 구현하여 Friend 객체의 어떤 맴버변수를 기준으로 정렬할지 정해야 합니다.<br>
<br>
<br>
<strong>Comparable Interface 를 구현(implements) 한 뒤 compareTo 메소드를 오버라이드하면 해결할 수 있습니다.</strong><br>
<br>
### Code

```
class Friend implements Comparable<Friend>{
	String name;
	int height;
	int weight;
			
	public Friend(String name, int height, int weight){
		this.name = name;
		this.height = height;
		this.weight = weight;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public int getHeight() {
		return height;
	}

	public void setHeight(int height) {
		this.height = height;
	}

	public int getWeight() {
		return weight;
	}

	public void setWeight(int weight) {
		this.weight = weight;
	}

	@Override
	public int compareTo(Friend friend) {
		return name.compareTo(friend.getName());
	}
}

```
<br>
먼저, Friend 클래스에 Comparable<Friend>를 implements 하고, <br>
compareTo 메서드를 오버라이드하였습니다.<br>
<br>
compareTo 메서드는 매개변수로 Friend 객체를 받고,<br>
리턴값으로는 Friend 객체의 name 변수와 비교하는 구문을 넣었습니다.<br>
<br>
위와같이 수정하고 실행하면 결과가 강성* 김동* 민상* 순(오름차순)으로 정렬이 되는 것을 확인할 수 있습니다.<br>
<br>
다시 한번 <span style="color:red">Comparable 는 기본 정렬(오름차순 정렬)할 때 사용한다</span>는 말을 확인했습니다.
<br>
<br>
String, Integer, Date, File 등과 같은 클래스들도 같은 방식으로 Comparable 을 구현하고 있습니다.<br>
<br>
<br>
그렇다면 내림차순 정렬은 어떻게 해야할까요<br>
<strong>Compatator Interface</strong>를 사용하면 원하는 순서대로 정렬 기준을 바꿀 수 있습니다.<br>
<br>
이제 <span style="color:red">Compatator - 원하는데로 정렬 순서를 지정할 때 사용합니다.</span>가 무슨 말인지 알아보겠습니다.<br>
위 Friend Class 소스와 어떻게 다른지 비교하며 살펴보죠.<br>

### Code

```

	//Compatator 예제

	class Friend {
		String name;
		int height;
		int weight;
		
		public Friend(String name, int height, int weight){
			this.name = name;
			this.height = height;
			this.weight = weight;
		}
		
		public String getName() {
			return name;
		}

		public void setName(String name) {
			this.name = name;
		}

		public int getHeight() {
			return height;
		}

		public void setHeight(int height) {
			this.height = height;
		}

		public int getWeight() {
			return weight;
		}

		public void setWeight(int weight) {
			this.weight = weight;
		}
	}

	//Comparator Interface 구현한 MyComparator class 선언

	class MyComparator implements Comparator<Friend>{

		@Override
		public int compare(Friend f1, Friend f2) {

			if(f1.getHeight() > f2.getHeight() )
				return 1;
			else if(f1.getHeight() < f2.getHeight() )
				return -1;
			else
				return 0;
		}
		
	}

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		
		List<Friend> friends = new ArrayList<>(); 
		friends.add(new Friend("김동*", 184, 77));
		friends.add(new Friend("민상*", 190, 70));
		friends.add(new Friend("강성*", 200, 72));
		
		Collections.sort(friends, new MyComparator());
		
		int size = friends.size();
		for(int i = 0; i < size ; i++){
			System.out.print(friends.get(i).name + " "); 
		}
	}

```
<br>

위 Friend Class 소스와 다른 점들은 
1. Friend 클래스에 Comparable Interface 를 지웠습니다.<br>
2. MyComparator 클래스를 선언하고 Comparator<Friend>를 implements 하였습니다.<br>
3. main 함수 내에 Collections.sort()의 두번째 파라미터로 MyComparator의 인스턴스를 만들어 넣었습니다.<br> 
<br>
아래와 같은 결과를 얻을 수 있습니다.<br>
<br>
<결과><br>
￼김동*:184 민상*:190 강성*:200<br>
<br>
<br>
마지막으로 배운 내용을 정리해보겠습니다. <br>
<br>
<span style="color:red"><strong>Arrays.sort() - 배열을 정렬합니다.</strong><br>
<strong>Collections.sort() - Collection 프레임워크를 정렬합니다.</strong><br>
<br>
<strong>Comparable - 기본 정렬(오름차순 정렬)할 때 사용합니다.</strong><br>
<strong>Compatator - 원하는데로 정렬 기준, 순서를 지정할 때 사용합니다.</strong></span><br>
<br>
<br>
<br>
<br>
