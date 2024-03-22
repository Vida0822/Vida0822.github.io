---
published: true
title: "Strategy Pattern" 
categories: Java
tag: [Java, DesighnPatern, Strategy] 
toc: true
author_profile: false 
  
---



##### Observer 패턴이란?

한 객체의 상태가 바뀌면 그 객체를 의존하는 다른 객체들에 연락이 가고, 특정 부분에 자동으로 내용이 갱신되는 방식의 일대다 의존성을 정의한다. 

간단히 얘기하자면, 어떤 객체의 상태가 변할 때 그와 연관된 객체들에게 알림을 보내는 디자인 패턴이 옵저버 패턴이라고 할 수 있다. 주로 분산 이벤트 핸들링 시스템을 구현하는 데 사용된다. 

이는 우리가 일반적으로 떠올리는 유튜브등의 구독 플랫폼 동작원리와 일치한다. 유튜브 채널이 새로운 영상을 업로드하면 구독을 누른 사용자, 즉 구독자들에게 ‘데이터 변화’사실을 메세지로 전달하고, 이를 전달받은 구독자들은 적절한 행동을 수행한다.

<br>



##### 구조

>  발행자

*Subject, Observable, Publisher* 

객체의 상태 변경여부, 상태 변경에 대한 메세지를 발행하는 주체다.

주체에는 일반적으로 새로운 옵저버를 목록에 등록하고 후자는 목록에서 옵저버를 빼는 기능이 있다. 주체에 상태 변화가 생겼을 때 해당 객체의 Observer 컬렉션에 담긴 Observer들에게 메세지로 통지한다. 

<br> 

> 구독자

*Observer, Subscriber*

객체의 상태변경에 대한 자세한 정보를 전달 받는 구독자, 상태 변경에 대한 정보가 담겨있는 메세지를 구독하는 주체다. 메세지를 기반으로 자신의 상태를 업데이트하는 기능을 가진다. 

<br>



##### 동작 원리

 객체의 상태 변화를 관찰하는 관찰자들, 즉 옵저버들의 목록을 객체에 등록하여 상태 변화가 있을 때마다 메서드 등을 통해 객체가 직접 목록의 각 옵저버에게 통지한다. 

구체적으로,  옵저버 또는 리스너(listener)라 불리는 하나 이상의 객체를 관찰 대상이 되는 객체에 등록시킨 후 각각의 옵저버들은 관찰 대상인 객체가 발생시키는 이벤트를 받아 처리하는 것이다.

<br>

![image](https://github.com/Vida0822/SpringBoot-JPA/assets/132312673/d4b81dbc-d3d5-4354-aba3-1748f59b53db)

(*notify() -> update() 로 수정)

<br>

이벤트가 발생하면 각 옵저버는 콜백(callback)을 받는다.

각각의 파생 옵저버는 `notify` 함수를 구현함으로써 이벤트가 발생했을 때 처리할 각자의 동작을 정의해야 한다.

<br>



##### 사용된 OOP 원리 

- 구현보다는 **추상화 또는 인터페이스**에 의존해라.

- 상속보다는 **구성(합성 or 위임); Composition** 활용해라.

  “A는 B이다(is-A)” 보다 “A에는 B가 있다”가 더 좋을 수 있다.

- 협력하는 객체 간의 결합도는 느슨한 결합을 사용해야 한다. (의존성 주입)

<br>



##### 예제 : 잡지사와 구독자

![image (1)](https://github.com/Vida0822/SpringBoot-JPA/assets/132312673/c6358172-dd32-4a92-a437-29cea075f51a)

Publisher를 구현한 NewsMachine 클래스는 정보를 제공하는 Publisher가 되며 Observer객체들을 가지고 있다.
그리고 Observer 인터페이스를 구현한 AnnualSubscriber(매년 정기구독자), EventSubscriber(이벤트 고객) 클래스는 NewsMachine이 새로운 뉴스를 notifyObserver() 를 호출하면서 알려줄 때마다 Update가 호출된다.

<br>



##### 구현 in Native Java 

```java
package design_pattern;

import java.util.ArrayList;

interface Publisher{
	public void add(Observer observer) ; 
	public void delete(Observer observer); 
	public void notifyObservers() ; 
}

interface Observer {
	public void update(String title, String news) ; // msg
}

class NewsOffice implements Publisher{
	private ArrayList<Observer> observers ; 
	private String todayNewstitle; 
	private String todayNewsContent ; 

	public NewsOffice() {
		observers = new ArrayList<>() ; 
	}
	
	@Override
	public void add(Observer observer) {
		observers.add(observer); 
	}

	@Override
	public void delete(Observer observer) {
		observers.remove(observer);  
	}

	@Override
	public void notifyObservers() {
		for(Observer observer: observers)
			observer.update(todayNewstitle, todayNewsContent);
	}
	
	public void setNewsInfo(String title, String news) {
		this.todayNewstitle = title; 
		this.todayNewsContent = news; 
		notifyObservers() ; 
	}
}

class AnnualSubscriber implements Observer{
	private String todaysNews ; 
	private Publisher publisher ; 
	
	public AnnualSubscriber(Publisher publisher) {
		this.publisher = publisher ; 
		publisher.add(this);
	}

	@Override
	public void update(String title, String news) {
		this.todaysNews = title + " \n------------\n"+news ; 
		display() ; 
	}
	private void display() {
		System.out.println("\n\n오늘의 뉴스 \n===========================\n\n"+todaysNews);
	}
}

class EventSubscriber implements Observer{
	private String todaysNews ; 
	private Publisher publisher ;
	
	public EventSubscriber(Publisher publisher) {
		this.publisher = publisher; 
		publisher.add(this);
	}
	
	@Override
	public void update(String title, String news) {
		this.todaysNews = title + " \n------------\n"+news ; 
		display() ; 
	}
	
	private void display() {
		System.out.println("==이벤트 유저==");
		System.out.println("\n\n오늘의 뉴스 \n===========================\n\n"+todaysNews);
	}
}

class Main{ 
	public static void main(String[] args) {
		
		// 이 작업을 스프링에선 스프링 컨테이너가 대신해줌 
		NewsOffice newsOffice = new NewsOffice() ; 
		AnnualSubscriber annualSubscriber = new AnnualSubscriber(newsOffice); 
		EventSubscriber eventSubscriber = new EventSubscriber(newsOffice); 
		
		newsOffice.setNewsInfo("오늘한파", "전국 영하 18도 입니다");
		newsOffice.setNewsInfo("벛꼭 축제", "다같이 꽃보러가자!");
	}
}
```

<br>





##### 구현 in Spring 

> ApplicationEventPublisher 

ApplicationContext가 상속하는 인터페이스 중 하나이며, 옵저버 패턴의 구현체로 이벤트 프로그래밍에 필요한 기능을 제공해 준다.

<br>

* Publisher 

```java
@Transactional
public void ticketCancelBy(ReservationInfo cancelMessage){ // 이벤트 : 티켓 취소 
    Reservation reservation = getReservationInfo(cancelMessage) ; // 예약 정보 찾아서
    reservation.softDelete(); // 삭제 
    applicationEventPublisher.publishEvent(cancelMessage); 
    // ** TransactionalEventListener로 등록된 애들한테 메세지를 발생한다 
}
```

<br>

* Subscriber 

```java
@Component
public class TicketObserver{
    
    @Asyne("threadPoolTaskExecutor")
    @TransactionalEventListener // TransactionalEventListener로 등록 
    public void onTicketCancelHandler(ReservationInfo cancelMessage){ 
        // 이벤트 발생시 주체가 notify(publishEvent)한 정보(cancelMessage)를 바탕으로 
        log.info("Event Message :{}", cancelMassage) ; // 적절한 행동 수행 
    }
}
```

