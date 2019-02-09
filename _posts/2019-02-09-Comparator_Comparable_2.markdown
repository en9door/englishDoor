---
layout: post
title:  "Java Comparable, Comparator Interface (2)"
date:   2019-02-09 23:42:00
author: Dongy
categories: Java
cover:  "/assets/the_red_sea.jpg"
tags:	Java Comparable Comparator
---

<br>
Java Comparable, Comparator Interface (1)에 이어서 <br>
키로 오름차순 정렬하되, 비교 대상의 키가 서로 같다면 몸무게로 내림차순 정렬해한다면 어떻게 구현해야할까요?<br>
<br>
이 때도 Comparator를 이용하면 됩니다.<br>
<br>


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

//	이전 키를 기준으로 내림차순 정렬
	<!-- class MyComparator implements Comparator<Friend>{

		@Override
		public int compare(Friend f1, Friend f2) {

			if(f1.getHeight() > f2.getHeight() )
				return 1;
			else if(f1.getHeight() < f2.getHeight() )
				return -1;
			else
				return 0;
		}
		
	} -->

//	수정
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

￼<br>
결과를 보면 키가 커지는 순으로 정렬되고 키가 같은 경우 몸무게가 작아지는 순으로 정렬된 것을 확인할 수 있습니다.<br>
<br>
<결과><br>
김동*:184,77 민상*:190,70 장재*:200,80 강성*:200,72<br>
<br>
