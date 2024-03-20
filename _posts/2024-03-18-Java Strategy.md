---
published: true
title: "Strategy Pattern" 
categories: Java
tag: [Java, DesighnPatern, Strategy] 
toc: true
author_profile: false 
  
---





##### 전략 패턴

전략 패턴(Strategy)은 실행(Runtime, 런타임) 중에 알고리즘 전략을 선택하여 객체 동작을 실시간으로 바뀌도록 할 수 있게 하는 행위 디자인 패턴이다. 여기서 “전략”은 특정한 목적을 수행하기 위한 행동 계획(알고리즘 or 기능 or 동작)을 말한다.

우리는 설계 단계에서 “변화는 것”과 “변하지 않는 것”을 구분해야 한다. “변하지 않는 것”은 그대로 두고 “변하는 것”은 찾아서 나머지 코드에 영향을 주지 않도록 캡슐화를 해야 한다. 전략, 즉 특정 기능을 수행하는 알고리즘을 캡슐화해두고, 상황에 맞춰 교체함으로써 변화에 동적으로 대응한다. 

변경되는 부분은 캡슐화를 통해서 확장에는 열려 있고 수정에는 닫혀 있는 구조로 런타임 시점에 적절한 알고리즘 전략을 선택하여 애플리케이션이 유연하게 동작 할 수 있도록 하는 것이다.

 <br>



##### 핵심 원리

실행 도중 전략을 교체함에도 실행, 다른 코드에 문제가 생기지 않는 원리는 아래 원칙을 지키기 때문이다. 

- 구현보다는 **추상화 또는 인터페이스**에 의존해서 코딩해라.

- 상속보다는 **구성(합성 or 위임); Composition** 활용해라.

전략 패턴은 상속이 아닌 합성(or 객체의 구성, 위임)을 사용한다. 비즈니스 환경에서 변하지 않는 것들은 Context에 그대로 두고 변할 수 있는 것들은 IStrategy는 공통 인터페이스를 통해서 캡슐화 한다.

그래서 Context는 IStrategy의 구현(CODE)이 아닌 퍼블릭 인터페이스(Message)만 알고 있기 때문에 최소한의 정보,느슨한 결합도를 가지고 있어 IStrategy의 내부 구현이 변경되더라도 Context까지는 변경의 전염성이 퍼지지 않는다.

<br>





##### 기본 구조 

![image-20240320233027571](https://github.com/Vida0822/TumblbugAPI_inSpring/assets/132312673/feb33905-ecaf-4269-acf7-97249a7a4666)

* Client : 전략 또는 기능 교체를 요청하는 주체, 전략 또는 기능의 결과를 받음
* Factory : 요청에 매칭되는 전략 인스턴스를 생성해 주입해주는 역할 (Spring container가 대체)
* Context: 기본적으로 실행되는 코드와 요청에 매칭되는 전략 또는 기능을 포함하는 역할 
* IsStrategy : 모든 전략 구현체에 대한 공용 인터페이스 
* ConcreteStrategies : 공통 추상 인터페이스를 구현한 구현체 그룹, 실제 기능/ 동젝 코드가 정의 

<br>





<details>
<summary><b> 구현 in Native Java </b></summary>
<div markdown="1">

```java
package design_pattern;

// 전략 인터페이스 
interface IStrategy{
	void execute() ; 
}

// 전략 구현체 (ConcreteStrategies) 
class FirstStrategy implements IStrategy{
	@Override
	public void execute() {
		System.out.println("First Strategy");
	}
}
class SecondStrategy implements IStrategy{
	@Override
	public void execute() {
		System.out.println("Second Strategy");
	}
}

class Context{
	private IStrategy strategy ; 
	
	public Context(IStrategy s) { // 의존성 주입 (생성자) 
		this.strategy = s ; 
	}
	
	public void setStrategy(IStrategy s) { // 의존성 주입(setter) 
		this.strategy = s ; 
	}
	
	public void doSomething(IStrategy s) {
		// 메서드 주입 : 주입과 동시에 기능호출 
		s.execute();  
	}
	
	public void doBasicLogic() {
		
	}
	public void doSomething() {
		this.strategy.execute();
	}	
}

class Client{
	/*
	public void doIt() {
		Context ctx = new Context(new FirstStrategy()); 
		ctx.doSomething();  
	}
	=> 문제 : 생성과 사용의 분리 X (결합도 up) 
		ㄴ 호출부에서 객체 타입, 생성자 같은 인자에 대한 과도한 지식(정보)를 알아야한다. 
		=> Factory 패턴 사용해 간단한 식별정보만 넘겨주면 Factory가 생성하도록 한다 
	*/
	private final Factory factory; 
	
	public Client(Factory f) {
		factory = f;
	}
	
	public void doIt(String req) {
		Context ctx = factory.createContext(req) ; 
		ctx.doSomething(); 
	}
}

class Factory{
	public Context createContext(String req) {
		IStrategy selected = createStrategy(req) ; 
		return new Context(selected);  // 주입 
	}
	
	private IStrategy createStrategy(String req) {
		IStrategy iStrategy; 
		switch (req) {
		case "first":
			iStrategy = new FirstStrategy() ; 
			break;

		default:
			iStrategy = new SecondStrategy() ; 
			break;
		}
		return iStrategy;
	}
}
```

</div>
</details> 

<br>



##### Spring과 Strategy패턴

Spring에서는 ApplicationContext라는 객체가 있고, 이 객체 안에는 ConcreateStrategies; 전략 구현체들이 주입되어 담겨 있다.(자연스럽게 생성과 사용이 분리되어 있다.) 

그래서 Java와 다르게 전략 구현체를 인스턴스화 하는 Factory 클래스가 필요 없다.(다만, 전략 구현체들이 Bean객체로 등록이 되어 있다라는 가정이 있다.)

<br>



<details>
<summary><b> 과제 : 액션 어드벤처 게임 설계 </b></summary>
<div markdown="1">

```java

interface WeaponBehavior{
	void attack() ; 
}

class KnifeBehavior implements WeaponBehavior{
	@Override
	public void attack() {
		System.out.println("KnifeBehavior");
	}
}
class SwordBehavior implements WeaponBehavior{
	@Override
	public void attack() {
		System.out.println("SwordBehavior");
	}
}
class AxeBehavior implements WeaponBehavior{
	@Override
	public void attack() {
		System.out.println("AxeBehavior");
	}
}
class BowAndArrowBehavior implements WeaponBehavior{
	@Override
	public void attack() {
		System.out.println("BowAndArrowBehavior");
	}
}

abstract class Charactor{
	WeaponBehavior weapon ; 
	
	public void setWeapon(WeaponBehavior weapon) {
		this.weapon = weapon ; 
	}
	
	public abstract void fight() ; 
}

class King extends Charactor{
	@Override
	public void fight() {
		System.out.println("I'm king");
		weapon.attack();
	}
}
class Knight extends Charactor{
	@Override
	public void fight() {
		System.out.println("I'm Knight");
		weapon.attack();
	}
}
class Queen extends Charactor{
	@Override
	public void fight() {
		System.out.println("I'm Queen");
		weapon.attack();
	}
}
```



</div>
</details> 

<br>
