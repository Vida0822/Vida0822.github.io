---
published: true
title:  "No enclosing instance of type 에러 해결 "
categories: Troubleshooting 
tag: [java, CompileError] 
toc: true
author_profile: false 

---



##### 문제 상황 

```java
public class LSP {

	class Cat extends Animal{
		void spaek() {
			System.out.println("냐옹");
		}
	}
    
	public static void main(String[] args) {
		List<Animal> list = new ArrayList<>(); 
		list.add(new Cat()); // 문제 
	}
}
```

<br>





##### 원인 

static 함수(main)에서 참조하려는 클래스가 닫혀 있어 접근을 하지 못한다는 오류. 

보통 내부에 있는 클래스를 static 함수가 참조할 때 에러가 난다.

* static이 붙은 변수나 클래스는 클래스가 메모리에 올라갈 때 자동으로 생성이 된다. 

* 하지만 LSP 클래스 안에 있는 Cat은 외부 클래스 LSP의 인스턴스가 생성된 후에만 접근할 수 있다. 

<br>



##### 해결 

상위 클래스에 포함된 하위 클래스는 미리 생성하고 나서 사용이 가능

1. Cat 클래스에 static 키워드 붙이기 
2. 내부 클래스인 Cat을 외부 클래스로 분리 

<br>





