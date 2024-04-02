---
published: true
title: "Builder Pattern" 
categories: Java
tag: [Java, DesighnPatern, Builder] 
toc: true
author_profile: false 
  
---



##### Builder 패턴이란?

생성 패턴 중 하나로, 복잡한 객체의 생성 방법으로부터 간단한 표현 방법으로 분리해 다양한 구성의 인스턴스를 만든다.

생성자 방식으로 객체를 생성하면 매개변수가 많아질수록 매개변수가 무엇을 의미하는지 파악하기 힘들어지며, 매개변수의 순서를 혼동할 수 있어, 가독성 및 유지 보수성이 떨어진다. 

메서드로서 입력하게 하면 전달되는 인자의 순서가 달라도 되며 어떤 필드를 입력해야 하는지 명확하게 표시된다. 또한 전달되는 매개변수가 잘못된 경우에도 컴파일 시점에 파악하기 쉽다. 

<br>



##### 도입 이유

```java
public class Developer {
    private int year;
    private String name;
    private String mainSkill;
    private String phoneNumber;

	public Developer() {}

    public Developer(int year) {
		this.year = year;
    }

	public Developer(String mainSkill, String phoneNumber) {
        this.mainSkill = mainSkill;
      	this.phoneNumber = phoneNumber;
	}

    public Developer(int year, String name, String mainSkill, String phoneNumber) {
	     this.year = year;
        this.name = name;
        this.mainSkill = mainSkill;
        this.phoneNumber = phoneNumber;
    }
}
```

점층적 생성자 패턴으로 객체를 생성할 때 다양한 매개변수 조합을 사용하여 여러 옵션을 설정할 수 있도록 한다. 다만 이렇게 하면 매개변수의 순서가 정말 혼동되고 복잡해진다. 

이러한 문제를 해결하기 위해 자바빈 패턴(기본 생성자+getter, setter)을 사용한다. 매개변수가 없는 기본 생성자로 객체를 생성한 후, setter 메서드를 이용해 클래스 필드의 초깃값을 설정하는 방식이다. 

그러나 이렇게 하면 두 가지 문제가 발생한다. 첫째, 아무 조건 없이 객체를 생성해놓고 모든 필드에 제대로 set 하지 않으면 필드 값의 null로 인해 퍼블릭 인터페이스 호출 시 치명적인 NPE 가 발생할 수 있다. 둘째, 불변성의 문제이다. Setter 메서드를 노출해야 하기 때문에 객체의 필드 값이 언제 어디서든지 변경될 수 있다. 

따라서, 처음 알맞게 매개변수를 넘겨줘야 객체가 생성+불변성 보장되는 생성자 객체 방식을 결국 사용해야 하는데, 그 가독성의 문제를 해결하는 패턴이 Builder 패턴이다.

<br>



##### Builder패턴 구조(GOF)

생성자에 들어갈 매개변수를 **메서드**를 통해 하나하나 받아들이고 마지막에 통합 빌드해(생성자를 호출함으로써) 객체를 생성하는 방식이다.

<br>

> Data Class 

```java
public class Developer{
    private int year; 
    private String name = "유프링" ; 
    private String mainSkill = "java";
    private String phoneNumber ;
    
    // 실제 생성 방법
    public Developler(int year, String name, String mainSkill, String phoneNumber){
        this.year = year ; 
        this.name = name ;
        this.mainSkill = mainSkill;
        this.phoneNumber = phoneNumber;
    }
}
```

<br>



> Builder Class

Data class 와 구분되는 별도의 클래스를 정의하고 그 안의 **Setter와 동일한 기능의 메서드**를 이용해 저장해둔 필드 값으로 데이터 class 인스턴스에 할당&생성하는 역할을 가진다.

```java
public claa DeveloperBuilder{
    private int year;
    private String name = "신빌더"; // 디폴트 값을 지정해줌으로써 매개변수 생략을 간접적으로 지원한다 
    private String mainSkill;
    private String phoneNumber;
    
    // 표현방법 : 필드명과 동일한 메서드명으로 setter 수행
    public DeveloperBuilder year(int y) {
        this.year = y ; 
        return this ; 
        //DeveloperBuilder 반환(year가 채워짐) --> year 이 객체에다가 다음 필드 set(인스턴스 변수)
    }
    public DeveloperBuilder name(String n){ 
        // 인스턴스 메서드 : 해당 인스턴스의 필드를 참조해 변경한다
        this.name = n ; 
        return this;
    }
    public DeveloperBuilder mainSkill(String m){
        this.mainSkill = m ; 
        return this; 
    }
    public DeveloperBuilder phoneNumber(String p){
        this.phoneNumber = p;
        return this; 
    }
    // 실제 생성을 자동으로 되도록 
    public Developer build(){
        return new Developer(year, name, mainSkill, phoneNumber); 
    }
}
```

​	각 setter 마지막에 있는 **return this**가 중요한 부분인데 여기서 this는 BurgerBuilder와 같은 빌더 클래스여야 한다. 왜냐하면 빌더 객체 자신을 리턴함으로써 메서드 호출 후 연속적으로 빌더 메서드를 체이닝하여 호출할 수 있게 된다.

<br>



> Making Instance

Builder 클래스에 정의해둔 Setter 메서드를 통해 생성자 파라미터들을 순서 상관없이 입력 받은 후 

최종적으로 build() 호출시 인스턴스를 생성해 리턴한다.

```java
public class Main{
    public static void main(String[] args){
        Developer developer = new DeveloperBuilder() // builder instance 생성
            .year(6) // setter --> year 필드가 탑재된 builder instance
            .name("유지노") // setter ㄴ +name 필드가 탑재된 builder instance
            .mainSkill("Spring") // setter ㄴ +mainSkill 필드가 탑재된 builder instance
            .phoneNumber("010-1234-1234") // setter ㄴ +phoneNum 필드가 탑재된 builder instance
            .build() ; // 해당 builder instance의 필드값들을 모아 Developer 객체 대리 생성 (복잡한 생성자식 객체 생성을 대신해줌)
    }
}
```

<br>



##### 개선 : 심플 빌더 패턴 (Effective Java)

롬복의 @Builder가 자동 생성해주는 빌더로써 이펙티브 자바에서 소개한 빌더 패턴이다. GoF의 빌더 패턴과는 다른 방식이다. 심플 빌더 패턴은 생성자가 많을 경우 또는 변경 불가능한 불변 객체가 필요한 경우 코드의 가독성과 일관성, 불변성을 유지하는 것에 중점을 둔다.

중점적으로 변화한 사항은 

1. Data class 안 내부 클래스(static)로 Builder 클래스 정의 
2. Data class의 객체를 생성하는 수단으로 Data Class안에 정의된 Builder의 builder()를 호출하여 쌓고, build()하도록 한정

<br>

> Data Class

```java
public class Developer{
    private int year;
    private String name ; 
    private String mainSkill ; 
    private String phoneNumber; 
    
    private Developer(int year, String name, String mainSkill, String phoneNumber){
        // 필수 요소
        this.year=year;
        this.name=name; 
        this.mainSkill=mainSkill; 
        this.phoneNumber=phoneNumber;
    }
    
    public static Developer.Builder builder(){ // static 메서드인 builder()를 호출하면 Developer안에 정의된 Builder 객체를 생성해 반환
        return new Developer.Builder(); 
    }
    
    public static class Builder{
        private int year;
        private String name; 
        private String mainSkill;
        private String phoneNumber;
        
        public Builder year(int y){
            this.year = y;
            return this;
        }
        public Builder name(String n){
            this.name = n;
            return this;
        }
        public Builder phoneNumber(String p){
            this.phoneNumber = p;
            return this;
        }
        public Developer build(){
            return new Developer(year, name, mainSkill, phoneNumber);
        }
    }
}
```

<br>



> Main

```java
public class Main{
    public static void main(String[] args){
        Developer newDeveloper = Developer.builder() // Developer 클래스 안의 builder()를 호출함으로써 Developer에 정의된 Builder 객체 생성
            .year(1) // 생성자 메서드(year을 초기화 한후 객체 재생성)
            .name("new type") // 체이닝으로 name 필드 초기화한후 재생성
            .mainSkill("python")
            .phoneNumber("010-8888-1111")
            .build() ; // Builder 클래스 안에 성의되어있는 build() 호출 -> Data class의 private 객체 생성자 호출 --> Developer 반환
        
        System.out.println(newDeveloper.toString()) ;   
    }
}
```

<br>



##### 참고: Builder 클래스 초기화

1) setter 메서드로 초기화 

> 필수로 초기화되는 값이 없다

```java
public class DeveloperBuilder {
    private int year;
    private String name = "유프링";;
    private String mainSkill = "java";
    private String phoneNumber;		
		...
    public void phoneNumber(String p) {
        this.phoneNumber = p;
    }
}

public class Main {
    public static void main(String[] args) {
        Developer developer = new DeveloperBuilder()
            .year(6)
            .build();
        System.out.println(developer.toString());
    }
}
```

​	default 값 설정을 통해 지정해 준 name, mainSkill, 그리고 직접 입력해 준 year은 초기화가 되었지만 phoneNumber는 null이기 때문에 Developer 객체가 생성되지 않는다.

​	문제는, 객체 생성이 정상적으로 되지 않음에도 main 메서드에서 Builder 값을 설정할 때 어떤 문제도 발생하지 않는다는 것이다(build() 메서드가 문제없이 호출된다) . 객체 생성에 반드시 필요한 필드의 초기화 메서드가 강제되지 않기 때문에, 초기화를 해야 할 필드를 누락할 수 있으며 이로 인해 클래스의 기능이 제대로 동작하지 않고 오류가 발생할 수 있다.  즉 필수 멤버 없이 모든 필드가 선텍적 멤버로서 다뤄지기 때문에 문제가 발생한다.

<br>



2. 생성자 + setter 메서드로 초기화

```java
public class DeveloperBuilder{
    private int year; 
    private String name;
    private String mainSkill;
    private String phoneNumber;
    
    public DeveloperBuilder(int y){
        this.year = y;
    }
}
```

필수 초기화 필드와 선택 초기화 필드를 구분하기 위해 생성자와 setter 메서드를 동시에 사용한다. 필수 멤버는 빌더의 생성자로 받게 해, 초기화 해야만 Builder 객체가 생성되도록 설정하고 선택적 사항은 Setter 함수를 통해 받는다. 

<br>



3. 생성자 메서드로만 초기화 

그러나 불변성을 보장하기 위해 Setter메서드를 모두 없애고, builder를 통한 생성자 함수만을 이용함으로써 객체를 생성한다. 즉 모든 필드가 필수 사항이 된다.

<br>
