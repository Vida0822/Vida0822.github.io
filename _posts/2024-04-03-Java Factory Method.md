---
published: true
title: "Factory Method Pattern" 
categories: Java/Spring
tag: [Java, DesighnPatern, Factory Method] 
toc: true
author_profile: false 
  
---



#### Factory Method 패턴이란?

​	Factory 패턴은 말 그대로 객체를 **사용자 대신** 생성해 주는 것을 의미한다. 그 중 팩토리 메서드 패턴은 **팩토리 클래스를 상,하위로 구분하여 각각의 서브 클래스에서 특정 객체를 직접 생성**함으로써, 어떤 클래스의 인스턴스를 만들지 서브(하위)클래스에서 결정하도록 하는 패턴이다. 

​	보통 생성되는 객체의 카테고리(구현체)는 다양하고 잦은 변화가 있지만 일련의 기능(행동)이 고정되어있는 경우 Factory Method 패턴을 사용한다. 

>  ex) Wanted Ticketing 서비스에서 고객 마다 적용하는 할인 정책의 종류는 다르지만, 할인 정책을 적용한 티켓 최종 가격을 계산하는 과정(정가 가격 - 할인금액 = 최종 가격)은 모든 고객이 동일하다.

​	뿐만 아니라 객체의 구현체가 트리 형태 등으로 구조화, 분류화 되어있을 때 각각의 서브트리마다 다른 Factory 클래스로 정의하고 이를 최종적으로 상위 클래스에서 묶어줄 때도 사용한다. 

<br>

<br>



#### 구조

핵심은, 상위 팩토리 클래스에선 인스턴스를 만드는 방법(인터페이스)만 정의하고 하위 클래스에서 관련 데이터 조작 함수들을 오버라이딩 해 인스턴스를 생성하는 것이다. 



![Untitled](https://github.com/Vida0822/OOP/assets/132312673/27c0fe7e-8b5f-44ef-bb4c-eb0842173341)

 

**Creator : 상위 팩토리 클래스 **

> 매니저 : 음식을 만들 요리사를 지정한다

두가지 메서드를 제공하는데 첫째는 객체를 생성하는 메서드, 즉 팩토리 메서드용 인터페이스를 제공하는 것이다. 해당 메서드를 호출해 Product 클래스를 구현한 구체적인 객체를 얻어온다. 

둘째는 팩토리 메서드에 의해 생산된 ConcreteProduct 객체로 Product를 구현한 모든 객체가 공통으로 해야하는 일련의 작업을 실행하는 퍼블릭 인터페이스이다. 

<br>

**ConcreteCreator : 하위 팩토리 클래스**

> 요리사 : 실제 음식을 만든다 

구체적인 Product 형 객체를 생산하는 factory Mathod를 구현한다. 

<br>

**Product :생산 대상 추상클래스**

> 음식 

생산 대상 객체를 묶어주는 추상화 클래스 또는 인터페이스이다.

<br>

**Concrete Product**

> 파스타, 피자, 짜장면

Product 클래스를 구현한 실질적인 객체들이다. 

<br>



​	이전에 살펴본 Factory 클래스가 직접 구현체를 생성하는 것이 아닌 사용하는 서브클래스에 따라 생성되는 객체의 인스턴스가 결정된다. 즉 하위 클래스가 실질적인 Factory 패턴이며 이에 상위클래스를 정의해 실제 생산 과정을 캡슐화함으로써 구체적인 타입을 감추고 느슨한 결합 구조를 만든다.  

<br>

<br>



#### 예시

고객 통신사 종류에 따라 그에 맞는 티켓 할인 정책 객체를 생성해야한다. 

카드사 할인, 통신사 할인, 포인트 할인 등 어떤 종류의 할인 정책(팩토리)를 생성할지 결정하여 서브 팩토리 객체를 생성한다. 각각의 팩토리 객체에서 더욱 세부적인 분류값(lg u+, skt)에 맞춰 실제 구현 객체를 생성하고 최상위 제품 클래스에 담는다. 

<br>



**코드**

```java
package design_pattern;

import java.math.BigDecimal;
import java.util.Optional;

//Client 
public class FactoryMethod{
	public static void main(String[] args) {
		String type = "kt"; 
		DiscountPolicyFactory f  // 매니저 호출 
		= DiscountPolicyFactory.chooseFactory(type) ; // 요리사 지정 : 보통 얘를 매니저 클래스에서 type에 따라 지정(생성)해줘야함 --> 그 지정된 요리사 클래스가 구체적인 객체 생성
		BigDecimal finalFee = f.calculateTicketFee(10000, type) ; 
		System.out.println(finalFee.toString());
	}
}

// Creator 
abstract class DiscountPolicyFactory{
	
	// factory Method
	protected abstract DiscountPolicy createDiscountPolicy(String type) ;
	
	public static DiscountPolicyFactory chooseFactory(String type){
		if(type == "lg" || type == "skt" || type == "kt")
			return new TelecomeDiscountPolicyFactory() ;
		return null; 
	}
	
	// operation method
	public BigDecimal calculateTicketFee(int price, String type) {
		DiscountPolicy discountPolicy = createDiscountPolicy(type) ; 
		return discountPolicy.caluateDiscountFee(price);  // 각 할인정책별 계산법 사용 
	}
}

// Concrete Creator 
class TelecomeDiscountPolicyFactory extends DiscountPolicyFactory{

	@Override
	protected DiscountPolicy createDiscountPolicy(String type) {
		switch(type.toLowerCase()) {
		case "lgu" : return new LGUDiscountPolicy(new BigDecimal(String.valueOf(1000)), new BigDecimal(String.valueOf(0.1))) ; 
		case "skt" : return new SKTDiscountPolicy(new BigDecimal(String.valueOf(2000)), new BigDecimal(String.valueOf(0.1))) ;
		case "kt" : return new KTDiscountPolicy(new BigDecimal(String.valueOf(3000)), null) ; 
		default : return null ; 
		}
	}
}

// Product 
abstract class DiscountPolicy{
	protected BigDecimal discountFee; 
	protected BigDecimal discountRate; 
	
	// Product의 공통된 기능 
	public BigDecimal caluateDiscountFee(int price) {
		if(Optional.ofNullable(discountRate).isPresent())
			return new BigDecimal(String.valueOf(price)).multiply(new BigDecimal(String.valueOf(discountRate))) ; 
		return new BigDecimal(String.valueOf(price)).subtract(discountFee) ; 
	}
}

// Concreate Product 
class LGUDiscountPolicy extends DiscountPolicy{
	public LGUDiscountPolicy(BigDecimal discountFee, BigDecimal discountRate) {
		this.discountFee = discountFee ; 
		this.discountRate = discountRate; 		
	}
}
class SKTDiscountPolicy extends DiscountPolicy{
	public SKTDiscountPolicy(BigDecimal discountFee, BigDecimal discountRate) {
		this.discountFee = discountFee ; 
		this.discountRate = discountRate; 
	}
}
class KTDiscountPolicy extends DiscountPolicy{
	public KTDiscountPolicy(BigDecimal discountFee, BigDecimal discountRate) {
		this.discountFee = discountFee ; 
		this.discountRate = discountRate; 
	}
}
```

<br>

<br>



#### Tip 

결국 자바에선 어디선가는 객체를 생성해야하기 때문에 new를 피할 순 없다. 

대신 new 를 사용할 땐 '변하는 것'과 '변하지 않는 것'을 구별해 '변하는 것'은 직접 생성하지 않고 생성과 사용을 분리해 느슨한 결합도를 유지해야 한다. 

<br>

<br>



