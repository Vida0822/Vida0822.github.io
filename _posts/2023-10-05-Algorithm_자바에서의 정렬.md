---
published: true
title: "Sort-Comparator(Java)" 
categories: Algorithm 
tag: [algorithm, Sort, Java] 
toc: true
author_profile: false 

---



##### compare(To) 함수 

매개변수 : 비교 대상 Other 객체. 

즉 this 와 other 의 비교 

**반환 값 : 음수/양수 --> This가 앞으로/뒤로 온다 **

<br>





##### 1. 클래스에서 implements Comparable

```java
public class Fruit { // Comparable<Fruit>{
	
	public String name ; 
	public int quantity ; 

	public Fruit(String name , int quantity) {
		this.name = name ; 
		this.quantity = quantity ; 
	}
	
	// Fruit 객체간의 크기관계를 정의해줘야함 
	public int compareTo(Fruit other) { 
		// 음수 --> 내가 더 작음, 양수 --> 내가 더 큼 , 0 --> 두개가 같음
		
		//return name.compareTo(other.name) ; 
		// 나의 이름과 다른 과일의 이름을 비교해서 이름이 더 작은애가(문자열 상 앞에 있는애가) 더 작은애 (이름의 사전식 순서가 빠른애를 더 작은애로 보겠다) 
		
		return quantity - other.quantity ; // 재고 수량으로 정렬 (내가 수가 더 작으면 음수 리턴 ) 	
	}	
}  
```

<br>



##### 2. 비교기 직접 구현

클래스는 compareTo함수를 두개 이상 동시에 가질 수 없음 

=> 하나의 객체 타입에 대해서 2개 이상의 기준으로 정렬을 지원하려면 Comparator을 사용 ! 

=> Comparator은 원하는만큼 만들어서 적용할 수 있음 

```java
package ch06_sort;

import java.util.Comparator;

public class Fruit { // Comparable<Fruit>{
	
	public String name ; 
	public int quantity ; 
        :	
	public int compareTo(Fruit other) { 
		return quantity - other.quantity ; // 재고 수량으로 정렬 (내가 수가 더 작으면 음수 리턴 ) 	
	}	
	
	public static Comparator<Fruit> nameComparator = new Comparator<Fruit>() {	
		@Override
		public int compare(Fruit fruit1, Fruit fruit2) {
			return fruit1.name.compareTo(fruit2.name) ; // String 객체 안에 있는 compareTo() 사용 
		}
	 }; // nameComparator
	
	 public static Comparator<Fruit> quantComparator = new Comparator<Fruit>() {
		 
		 @Override
		 public int compare(Fruit fruit1, Fruit fruit2) {
			 return fruit1.quantity - fruit2.quantity ; 
		 }
	 }; // quantComparator
}

```

<br>

> 엥 interface(abstract의 극단적 예) 인데 어떻게 new로 객체를 만들 수 있지? 

익명 클래스로 그 구현부를 아래에 작성해준 Comparator를 implement한 어떤 클래스를 정의한 후 new 해준거임

ㄴ 이름을 생략 

ex) 익명 클래스 --> 한번  클래스를 생성하고 사용하면 더 이상 필요 x (다른데서는 사용 x) : 그냥 여기서 생성하고 바로 써줌  

<br>



##### 3. 비교기 직접 구현 (익명 클래스)

```java
Arrays.sort(graph, new Comparator<int[]>() { // int 배열을 정렬하는 기준을 넣어줌
	@Override
	public int compare(int[] o1, int[] o2) {
		return o1[2]-o2[2] ;  // o1이 더 작으면 음수 --> o1이 앞선다 (오름차순) 
	}
});
```

<br>



