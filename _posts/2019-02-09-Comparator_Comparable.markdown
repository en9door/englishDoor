---
layout: post
title:  "Java Comparable, Comparator Interface"
date:   2019-02-09 22:42:00
author: Dongy
categories: Java
cover:  "/assets/the_red_sea.jpg"
tags:	Java Comparable Comparator
---
<br>
Java로 개발을 하면서 배열이나 List 등의 Collection 프레임워크를 Arrays.sort(), Collections.sort()를 이용하여 정렬해본 경험이 있을겁니다.<br>
<br>
Arrays.sort(), Collections.sort() 메소드가 Merge Sort, Insertion Sort, QuickSort 등 <strong>정렬 기법</strong>들을 구현하고 있다면, 오름차순으로 정렬할지 내림차순으로 정렬할지에 대한 <strong>정렬 기준은 Comparable, Comparator Interface가 정합니다.</strong><br>
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
<br>
이해를 돕기 위해 Arrays.sort(), Collections.sort() 예제를 확인해보면<br>
### Code

```
//Arrays.sort() 예제
	int array[] = {4,3,5,6,-1,-2};
	String array2[] = {"기아", "포르쉐", "현대", "벤츠", "도요타", "폭스바겐"};
			
	Arrays.sort(array);
	Arrays.sort(array2);
			
	int length = array.length;
	for(int i = 0; i < length ; i++){
		System.out.print(array[i] + " ");  // 결과는 -2 -1 3 4 5 6로 오름차순 정렬
	}
			
	System.out.println();
			
	int length2 = array2.length;
	for(int i = 0; i < length ; i++){
		System.out.print(array2[i] + " ");  // 결과는기아 도요타 벤츠 포르쉐 폭스바겐 현대로 오름차순 정렬
	}

```
위 예제에서 Arrays.sort(array), Arrays.sort(array2)는 int 와 String 의 Comparable 구현에 의해 오름차순의 기준을 가지고 정렬할 수 있었던 것입니다.<br>
<br>
이것이 <span style="color:red">Comparable 인터페이스는 기본 정렬(오름차순 정렬)할 때 사용한다는 말입니다.</span><br>
<br>
Comparable 을 구현하고 있는 클래스들은 같은 타입의 인스턴스끼리 서로 비교할 수 있는 클래스들, String, Integer, Date, File 등과 같은 클래스들 입니다.<br>
<br>
<br>
<br>
이번엔 배열이 아닌 List 형태로 sort() 를 구현해보면, 상단 배열에서 했던 방식과 똑같지만 int 타입의 배열 을 List 로 바꿨습니다.<br>
<br>
List 는 Collection 프레임워크 이므로 Arrays.sort()가 아닌 Collections.sort() 를 적용해야 합니다.<br>
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
<br>
이제부터는 기본 클레스가 아닌 우리가 만든 클레스로 정렬을 시도해보겠습니다.<br>
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
Friend 는 이름, 키, 몸무게 맴버변수를 가지고 있고 각각 setter와 getter를 선언해줬습니다. <br>
여기서 동일하게 Friend 객체를 생성하고, 배열이나 ArrayList 형태로 만들었을 경우
Arrays.sort(), Collections.sort() 는 작동할까요?<br>
<br>
오류가 발생합니다.<br>
<strong>이유는 객체내의 어떤 맴버변수를 기준으로 정렬할지 정하지 않았기 때문입니다.</strong><br>
객체를 정렬할 경우 객체 내의 어떤 변수로 정렬할지 만들어줘야 합니다.<br>
<br>
<strong>Comparable 인터페이스를 implements한 뒤 compareTo 메소드를 오버라이드하면 해결할 수 있습니다.</strong><br>
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
위와같이 수정하고 실행하면 강성* 김동* 민상* 순(오름차순)으로 정렬이 되는 것을 확인할 수 있습니다.<br>
<br>
다시 한번 <span style="color:red">Comparable 인터페이스는 기본 정렬(오름차순 정렬)할 때 사용한다</span>는 말을 확인했습니다.
<br>
<br>
String, Integer, Date, File 등과 같은 클래스들이 어떤 식으로 Comparable 을 구현하고 있을 지도 대략 알았습니다.<br>
<br>
<br>
그렇다면 내림차순도 정렬은 어떻게 해야할까요<br>
<strong>Compatator</strong>을 사용하면 내가 원하대로 정렬 기준을 바꿀 수 있습니다.<br>
<br>
이제 <span style="color:red">Compatator - 원하는데로 정렬 순서를 지정할 때 사용합니다.</span>가 무슨 말인지 알아보겠습니다.<br>

### Code

```

//Compatator 예제

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

//		@Override
//		public int compareTo(Friend friend) {
//			// TODO Auto-generated method stub
//			return name.compareTo(friend.getName());
//		}
	}

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

Friend 클래스에 Comparable 을 주석으로 막았습니다.<br>
MyComparator 클래스에 Comparator<Friend>를 implements 하여 선언하였습니다.<br>
메인함수 내에서 Collections.sort()의 두번째 파라미터로 MyComparator의 인스턴스를 만들어 넣었습니다.<br> 
<br>
아래와 같은 결과를 얻을 수 있습니다.<br>
<br>
<결과><br>
￼김동*:184 민상*:190 강성*:200<br>
<br>
<br>
마지막으로 내용을 정리해보면 <br>
<br>
<strong>Arrays.sort() - 배열을 정렬합니다.</strong><br>
<strong>Collections.sort() - Collection 프레임워크를 정렬합니다.</strong><br>
<br>
<strong>Comparable - 기본 정렬(오름차순 정렬)할 때 사용합니다.</strong><br>
<strong>Compatator - 원하는데로 정렬 순서를 지정할 때 사용합니다.</strong><br>
<br>
<br>
<br>
<br>
마지막으로 위 예제를 조금 더 생각해본다면<br>
키로 오름차순 정렬하되, 비교 대상의 키가 서로 같다면 몸무게로 내림차순 정렬해한다면 어떻게 구현해야할까요?<br>
<br>
이 때도 Comparator를 이용하면 됩니다.<br>

### Code

```
	class MyComparator implements Comparator<Friend>{
		
		@Override
		public int compare(Friend f1, Friend f2) {
			// TODO Auto-generated method stub
			if(f1.getHeight() > f2.getHeight() )
				return 1;
			else if(f1.getHeight() < f2.getHeight() )
				return -1;
			else{
				if(f1.getWeight() < f2.getWeight() )
					return 1;
				else if(f1.getWeight() > f2.getWeight() )
					return -1;
				else
					return 0;
			}
		}
	}

```

￼<br>
결과를 보면 키가 커지는 순으로 정렬되고 키가 같은 경우 몸무게가 작아지는 순으로 정렬된 것을 확인할 수 있습니다.<br>
<br>
<결과><br>
김동*:184,77 민상*:190,70 장재*:200,80 강성*:200,72<br>
<br>
