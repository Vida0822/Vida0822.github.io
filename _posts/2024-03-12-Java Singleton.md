---
published: true
title: "Singleton Pattern" 
categories: Java
tag: [Java, DesighnPatern, Singleton] 
toc: true
author_profile: false 
  
---



##### 싱글톤 패턴?

싱글톤 패턴(Singleton)은 생성 패턴 중 하나로 특정 클래스에 대한 단 하나만의 인스턴스를 생성해 사용하는 패턴이다. 즉 객체가 필요할 때 마다 생성하는 것이 아닌 단 한번만 생성해 전역에서 이를 공유, 사용한다. 

생성자를 여러번 호출하더라도 인스턴스가 하나만 존재하도록 보장해 애플리케이션 여기저기에서 동일한 객체 인스턴스에 접근할 수 있도록 한다. 

 <br>



##### 예시 : 커넥션 풀

설정, 연결 객체 등과 같은 경우 인스턴스를 여러개 만들면 불필요한 자원을 사용하게 되고 예상치 못한 문제를 만들 수 있다. 

<br>





##### 장/단점

장점 

* 유일한 인스턴스 : 객체의 일관된 상태를 유지하고 관리하기 용이하다
* 메모리 절약 : 인스턴스가 단 하나만 존재하기에 메모리 차지가 적다 
* 지연 초기화 : 인스턴스가 실제로 사용되는 시점에 생성해 초기비용을 줄일 수 있다. 

<br>

단점 

* 결합도 증가 : 전역 공유기 때문에 해당 인스턴스에 의존하는 경우 여러 클래스의 결합도가 증가한다

   --> 전역에서 사용되어 변경되기에 예상치 못한 동작이 발생할 수 있다

* 무분별한 사용 : 이를 방지하기 위해 변경, 사용에 대한 복잡성이 증가할 수 있다. 

<br> 



##### 핵심 아이디어

두개 이상의 객체를 만들면 안되므로 **new 생성자에 제약**을 걸어야한다. 

객체를 직접 사용하는게 아닌 만들어진 단일 객체를 반환할 수 있는 **정적 메서드** 가 필요하다.

정적 참조변수로 해당 객체를 사용한다.  

<br>



##### 구현 코드 

```java
package design_pattern;

public class Singleton {

	// 생성한 싱글톤 객체를 담아둘 전역 참조변수 
	private static Singleton instance ;
	
	// 기본 생성자를 막아놓는다 
	private Singleton() {} ; 

	// 싱글톤 객체 반환 메서드 
	public static synchronized Singleton getInstance() {
		if(instance == null)
			instance = new Singleton() ; 
		return instance ; 
	}
}

class Cient{
	public static void main(String[] args) {
		Singleton instance1 = Singleton.getInstance() ; 
		Singleton instance2 = Singleton.getInstance() ; 
		Singleton instance3 = Singleton.getInstance() ; 
		
		System.out.println(instance1 == instance2
						&& instance2 == instance3); // true 
	}
}
```

<br>





##### 주의사항 

싱글톤 패턴은 멀티 스레드 환경에서 Thread Safe 하지 않는 문제점이 있다. 

Thread Safe는 여러 스레드가 동시에 접근하는 경우 해당 어플리케이션에 어떤 문제도 발생하지 않음을 의미한다. 

가령, 두 스레드 A, B가 존재할 때 스레드 A가 if문을 통과해 싱글톤 객체를 생성하기 직전, 

스레드 B도 if 검사를 통과할 수도 있다. 

<br>



##### Thread Safe한 싱글톤 패턴

1. *sychronized* 키워드 

해당 키워드를 통해 getInstance 메서드에 하나의 스레드만 접근할 수 있도록 한다.

단, Lock으로 인해 다른 작업들은 대기해야하니 성능 저하가 발생할 수 있다.  

```java
// 싱글톤 객체 반환 메서드 
public static synchronized Singleton getInstance() {
	if(instance == null)
		instance = new Singleton() ; 
	return instance ; 
}
```

<br> 

2. 이른 초기화 (*eager initialization*)

싱글톤 객체 생성 비용이 크지 않은 경우 미리 생성하여 변수에 담아둔다. 

final 변수를 붙여 변하지 않도록 설정한다. 

단, 어플리케이션 실행 동시에 인스턴스화 하는 방식이므로 메모리를 필수 점유하게 되는데, 

해당 리소스를 사용하지 않는다면 자원 낭비일 수 있다. 

```java
package design_pattern;

public class Singleton {

	// 생성한 싱글톤 객체를 담아둘 전역 참조변수 
	private static Singleton instance = new Singleton() ;
	
	// 기본 생성자를 막아놓는다 
	private Singleton() {} ; 

	// 싱글톤 객체 반환 메서드 
	public static Singleton getInstance() {
		if(instance == null)
			instance = new Singleton() ; 
		return instance ; 
	}
}
```

<br> 

3. Bill Pugh Solution (권장) 

private static inner class를 사용해 Thread Safe한 싱글톤 패턴을 구현한다.

SingletonHolder는 해당 인스턴스를 사용하려고 getInstance()를 호출 할 때 로드되는데, 이 때 싱글톤 객체가 생성+초기화(final)된다.  

 JVM의 Class Loader에 의해 로드될 때 내부적으로 실행되는 synchronized 키워드를 이용해 

Thread Safe를 구현하며 이후엔 초기화된 SINGLETON_OBJECT 필드가 반환된다. 

```java
public class Singleton {

	// 기본 생성자를 막아놓는다 
	private Singleton() {} ; 
	
    // 싱글톤 객체 담아놓을 static inner class 
	private static class SingletonHolder{
		private static final Singleton SINGLETON_OBJECT = new Singleton() ; 
	}

	// 싱글톤 객체 반환 메서드 
	public static synchronized Singleton getInstance() {
		return SingletonHolder.SINGLETON_OBJECT ; 
	}
}
```

다만 이 문제도 자바 리플렉션과 직렬화를 통해 싱글톤이 파괴 될 수 있다 

<br>



4. Enum 사용 (권장)

* Enum: 상수들만 모아놓은 '클래스'. 즉 생성자, 메서드를 모두 가질 수 있다. 
* private 생성자로 인스턴스 생성을 제어하며, 상수만 갖는 특별한 클래스이기 때문에
  싱글톤의 성질을 자동적으로 갖게된다.
*  자바 리플렉션과 직렬화 문제를 해결한다. 

```java
public enum Singleton{
    SINGLETON_OBJECT 
}
```

추가 : [[JAVA\] Enum을 이용해서 Singleton을 구현하자 : 네이버 블로그 (naver.com)](https://m.blog.naver.com/PostView.naver?blogId=kbh3983&logNo=220907314096&proxyReferer=https:%2F%2Fwww.google.co.kr%2F)

<br> 





##### 결론 

OCP에선 적당한 추상화 레벨을 선택함으로써, 어떤 행위에 대한 본질적인 정의를 

서브 클래스에 전파하는 추상화 설계가 핵심이다. 

이 때 이 '추상화할 특징'에 대해 명확히 정의하는 능력이 필요하다. 

 

<br>



##### 참고 사이트 

[💠 완벽하게 이해하는 OCP (개방 폐쇄 원칙) (tistory.com)](https://inpa.tistory.com/entry/OOP-💠-아주-쉽게-이해하는-OCP-개방-폐쇄-원칙)

<br>

