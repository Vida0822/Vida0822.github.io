---
published: true
title: "JPA" 
categories: DB/ORM
tag: [Database, JPA, ORM, procedure] 
toc: true
author_profile: false 


---



* 라이브러리 (의존) 확인 

1. 프로젝트 폴더 우클릭 --> OpenIn --> Terminal 

   ```bash
   PS D:\Programming\Spring Boot\김영한의 스프링부트\jpashop> ./gradlew dependencies
   
   Welcome to Gradle 8.2.1!
   
   annotationProcessor - Annotation processors and their dependencies for source set 'main'.
   \--- org.projectlombok:lombok -> 1.18.30
   
   bootArchives - Configuration for Spring Boot archive artifacts. (n)
   No dependencies
   
   compileClasspath - Compile classpath for source set 'main'.
   +--- org.projectlombok:lombok -> 1.18.30
   +--- org.springframework.boot:spring-boot-starter-data-jpa -> 2.7.16
   |    +--- org.springframework.boot:spring-boot-starter-aop:2.7.16
   |    |    +--- org.springframework.boot:spring-boot-starter:2.7.16
   |    |    |    +--- org.springframework.boot:spring-boot:2.7.16
   |    |    |    |    +--- org.springframework:spring-core:5.3.30
   |    |    |    |    |    \--- org.springframework:spring-jcl:5.3.30
   |    |    |    |    \--- org.springframework:spring-context:5.3.30
   |    |    |    |         +--- org.springframework:spring-aop:5.3.30
   |    |    |    |         |    +--- org.springframework:spring-beans:5.3.30
   |    |    |    |         |    |    \--- org.springframework:spring-core:5.3.30 (*)
   
   									:
   									:
   ```

   

2. Intellij 

![image-20230924001736456](D:\Programming\github.io\images\2023-09-24-SpringBootJPA\image-20230924001736456.png)

--> Project Structure에선 그냥 나열했지만 옆에 Gradle 구조에서 보면 의존 계층 관계까지 볼 수 있다. 





View 세팅 

##### timeLeaf 

'template Engine' : 특정 코드를? html 코드로 변환해주는 도구 

그 中 timeLeaf 

	* 장점 : natural template --> 마크업을 깨지않고 그대로 사용 (웹 브라우저에서 그대로 출력됨--> 다른 애들은 안보임 html이 아니니까)
	* 단점 : 정확하게 매칭을 안하면 에러 (html에선 br만 썼으면 됐잖아 ) --> 근데 3.0 되면서 다 개선됨 ㅋ 걍 쓰면 됨 ! , 문법을 익혀야함 , 서버에서 프론트는 어차피 잘 안만들고 프론트단에서 react, vue.js 사용 

Tip : Spring 홈페이이 - Guide 진짜 짱 ! 뭐든 검색해도 이론적인 책 보는 것보다 그 제품을 만든데에서 확인 하고, 그 다음 공부하는게 나음 

[Spring | Guides](https://spring.io/guides)



ㄴ 세팅할건 x... 어차피 boot가 해두었기 때문



static --> 정적인 페이지 (순수한 html) : webServer 가 넘기는것 

​	ㄴ 그냥 application 실행만 시키고 해당 포트번호로 접근하면 그냥 그 페이지를 띄움 

templates --> 서버 거쳐서 동적으로 랜더링 해서 보여주는 페이지  



※ resource 파일도 src 파일 안에있기 때문에 고치면 서버 다시 띄워줘야함 

--> 근데 src 안에 있는거 바꿀때마다 계속 서버 리스타트..... 화면 태그 하나 바꾸는데 

--> Tip: library 추가 (spring boot -  devtools )

* devtools  --> 개발 당시 이것저것 귀찮은거 없애주고 도와줌 

  ㄴ 이거 의존 추가하고 서브 띄우면 

  (restartedMain 뜨면 잘 뜬거임 ) 

  ```bash
  2023-09-24 00:55:07.721  INFO 7324 --- [  restartedMain] jpabook.jpashop.JpashopApplication       : Starting JpashopApplication using Java 11.0.17 on DESKTOP-DGFPD6H with PID 7324 (D:\Programming\Spring Boot\김영한의 스프링부트\jpashop\build\classes\java\main started by SHIN HEEMIN in D:\Programming\Spring Boot\김영한의 스프링부트\jpashop)
  
  ```

  

  => 고친 파일만 recompile 하면 됨!!

  ![image-20230924005722656](D:\Programming\github.io\images\2023-09-24-SpringBootJPA\image-20230924005722656.png)




#### Database 생성 

H2 Database Server 설정 

![image-20230924105350582](D:\Programming\github.io\images\2023-09-24-SpringBootJPA\image-20230924105350582.png)

![image-20230924105340278](D:\Programming\github.io\images\2023-09-24-SpringBootJPA\image-20230924105340278.png)

![image-20230924105600075](D:\Programming\github.io\images\2023-09-24-SpringBootJPA\image-20230924105600075.png)

ㄴ jdbc url : db 파일을 지정할 경로 (파일 모드로 실행)

 --> jdbc:h2:~jpashop 

: **db파일을 jpashop 프로젝트 안에 생성** 

db를 프로젝트 파일 안에 파일형태로 '내장되어 '생성 : oracle 같은 외부 db x 

--> h2 console창에 우리가 아는 sql문으로 이것저것 db작업하면 실제 db처럼 동작함 ! 



=> 그 다음부터는  url -- jdbc:h2:tcp://localhost/~/jpashop 로 접근 

: 파일 생성까지는 어드민 권한이 있어야해서 ? 세션값을 물고 있어야했지만 그 다음부턴 마음대로 접근 가능 

ㄴ tcp: network 모드 



ㄴ 항상 .h2/sh을 실행하고 돌려야함 





#### Database 연결 

```yaml
spring:
  datasource:
    url: jdbc:h2:tcp://localhost/~/jpashop;MVCC=TRUE
    username: sa
    password:
    driver-class-name: org.h2.Driver

  jpa:
    hibernate:
      ddl-auto: create
    properties:
      hibernate:
        show_sql: true
        format_sql: true
```

--> 이런 옵션들 spring boot Document 에서 찾아서 해야함 ..

![image-20230924111252002](D:\Programming\github.io\images\2023-09-24-SpringBootJPA\image-20230924111252002.png)





* 문제 properties --> yml 파일로 설정파일 변경하는데 파일이름 빨간색 뜨며 인식 못함 

ㄴ github 에 등록 후 이렇게 됨 : git 에 push 안해줘서 그럼 ! 







* test 오류 : No tests found for given includes --> intellij 설정 바꿔줘야함 

![image-20230925224040786](D:\Programming\github.io\images\2023-09-24-SpringBootJPA\image-20230925224040786.png)



* Connection is broken

왠진 모르겠지만 h2에서 계속 튕김...? cmd 꺼서? ㅇㅇ 자동으로 켜진 cmd 프로ㅡ램 동자중이어서 ㅕ였음



** 쿼리 파라미터 로그 남기기 !! 

원래 그냥 ? 로 남음 (실제로 원하는 값이 들어가는지 log로 안찍힘 )

```sql
   insert 
    into
        member
        (username, id) 
    values
        (?, ?)
```



-->  설정 파일에 로그 설정 추가 

2023-09-25 22:56:12.765 TRACE 12372 --- [           main] o.h.type.descriptor.sql.BasicBinder      : binding parameter [1] as [VARCHAR] - [heemin]
2023-09-25 22:56:12.766 TRACE 12372 --- [           main] o.h.type.descriptor.sql.BasicBinder      : binding parameter [2] as [BIGINT] - [1]



```
implementation 'com.github.gavlyukovskiy:p6spy-spring-boot-starter:1.5.6'
```

ㄴ 스프링 부트는 왠만한 라이브러리 버전 호환되게 맞춰놓음 (그래서 버전 안적어놓음)

(근데 이렇게 부트가 고려 안해준 외부 라이브러리는 뒤에 버전까지 적어줘야함)



```sql
Hibernate: 
    insert 
    into
        member
        (username, id) 
    values
        (?, ?)
2023-09-25 23:03:44.763 TRACE 16036 --- [           main] o.h.type.descriptor.sql.BasicBinder      : binding parameter [1] as [VARCHAR] - [heemin]
2023-09-25 23:03:44.764 TRACE 16036 --- [           main] o.h.type.descriptor.sql.BasicBinder      : binding parameter [2] as [BIGINT] - [1]
2023-09-25 23:03:44.765  INFO 16036 --- [           main] p6spy                                    : #1695650624764 | took 0ms | statement | connection 3| url jdbc:h2:tcp://localhost/~/jpashop
insert into member (username, id) values (?, ?)
insert into member (username, id) values ('heemin', 1); // binding 된거 출력 다 됨 
```



※ 이런 로그 많이 찍는 외부 라이브러리는 성능을 확 떨어트릴 수 있기 때문에 배포/운영할 땐 주의해야함 ! 

: 성능 테스트 필수 **  



문제상황 : h2에 테이블 생성이 안됨 

**원래 스프링이 자동적으로 Entity를 스캔해주는데, 외부 모듈이나 main이 아닌 다른 패키지에 있는 Entity들을 스캔해준다고 한다.** 

<-> main이 있는 패키지와 다른 패키지에 있으면 @Entity를 스캔하지 못한다 

==> 아예 Entity를 인식하지 못하니 생성 시도조차 안하고 오류도 안나는 것 ! 

![image-20230927081252125](D:\Programming\github.io\images\2023-09-24-SpringBootJPA\image-20230927081252125.png)

-> 이렇게 main이 있는 JpaShopApplication과 도메인이 다른 패키지에 있어서 그럼 

--> 이동 

![image-20230927082049351](D:\Programming\github.io\images\2023-09-24-SpringBootJPA\image-20230927082049351.png)



문제상황 : EntityManager 생성 실패 

Error creating bean with name 'entityManagerFactory' defined in class path resource 

-> 발생원인 : '생성할때 '중복된 이름의 Entity 클래스가 있을때 발생하는 오류 

※ 기존에 있는거랑은 상관 x :

```
jpa:
  hibernate:
    ddl-auto: create
```

--> 이렇게 해줘서 기존의 테이블들은 싹 밀고 생성하기 때문 

==> '생성할때' 같은 이름의 테이블이 존재



![image-20230927081914424](D:\Programming\github.io\images\2023-09-24-SpringBootJPA\image-20230927081914424.png)

test용으로 만들었던 Member가 있음 

org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'entityManagerFactory' defined in class path resource [org/springframework/boot/autoconfigure/orm/jpa/HibernateJpaConfiguration.class]: Invocation of init method failed; nested exception is org.hibernate.AnnotationException: Associations marked as mappedBy must not define database mappings like @JoinTable or @JoinColumn: jpabook.jpashop.domain.Order.orderItems

--> 에러메세지는 at전의 문구까지 끝까지 봐야함 !! 

ㄴ 문제 원인 : "mapped By" 속성과 @JoinColumn은 같이 쓸 수 없음 

==> 잘못 적어준  @JoinColumn 삭제 

```java

    @ManyToOne // 다대일 중 다쪽이 주문 --> Orders(many) To Member(one)
    @JoinColumn(name = "member_id")  // 'FK(참조하는 컬럼)' 생성
    // @JoinColumn : join할 컬럼, 즉 FK를 생성하며 이름이 member_id 가 됨 --> 연관관계 거울 쪽에서 조인 당하는(참조되는 컬럼) "mapped By" 표시
    // fk 가 Order 테이블에 생김 (not Member 테이블) --> 연관관계 주인쪽에 JoinColumn 적어줌 !!
    private Member member ;

    @OneToMany(mappedBy = "order") // '참조 당하는 컬럼'
    // mappedBy ('참조된다') : 연관관계 거울 --> OrderItems의 필드 order가 참조하는 필드가 이 필드임
    // @JoinColumn Associations marked as mappedBy must not define database mappings like @JoinTable or @JoinColumn: jpabook.jpashop.domain.Order.orderItems
    // 연관관계 거울쪽엔 JoinColmn 적어주지 않음 : mapped By(참조당한다)와 JoinColumn(참조한다)는 같이 적어줄 수 없음
    private List<OrderItem> orderItems = new ArrayList<>() ;

} // class

```





org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'entityManagerFactory' defined in class path resource [org/springframework/boot/autoconfigure/orm/jpa/HibernateJpaConfiguration.class]: Invocation of init method failed; nested exception is org.hibernate.AnnotationException: Unknown mappedBy in: jpabook.jpashop.domain.Deleivery.order, referenced property unknown: jpabook.jpashop.domain.Order.delivery

--> 문제 원인 : 오타 

Order 테이블 

```java
@OneToOne
@JoinColumn(name = "delevery_id") // fk가 delevery_id 가 Order 테이블에 생김
private Deleivery deleivery ;
```



Delivery Table 

```java
    @OneToOne(mappedBy = "delivery")
    private Order order;
```



--> deleivery --> delivery로 수정 

ㄴ 배운점 : 직접 타이핑 하지말고 복붙 ! 



문제 상황 : 

org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'entityManagerFactory' defined in class path resource [org/springframework/boot/autoconfigure/orm/jpa/HibernateJpaConfiguration.class]: Invocation of init method failed; nested exception is javax.persistence.PersistenceException: [PersistenceUnit: default] Unable to build Hibernate SessionFactory; nested exception is org.hibernate.MappingException: Error parsing discriminator value

 Error parsing discriminator value



ㄴ 문제 원인 : discriminator  구별자로 둬야하는 DiscriminatorValue, 즉 각 item 클래스들의 dtype의 값을 중복되게 설정해줌 

```java
@Entity
@DiscriminatorValue("A")
@Getter @Setter
public class Albom extends Item {
```

```java
@Entity
@DiscriminatorValue("A")
@Getter @Setter
public class Movie extends Item {
```



ㄴ 해결 : Moview @DiscriminatorValue A --> M 

@Entity
@DiscriminatorValue("M") // A 
@Getter @Setter
public class Movie extends Item {

} 



* 여러 서비스 메서드에서 세터를 호출해 엔티티를 바꾸면 얘가 도대체 어느타이밍에, 어디서 왜 바뀌는지 알 수 없음 

  --> 변경지점이 명확히 보이도록 별도의 비즈니스 메서드를 제공해야함  , setter는 닫아줌 

  (아무데서나 셋셋셋하고 있으면 어플리케이션 수정 너무 어려움 ! )

  --> getter는 조회하는거니까 



* 칼럼명을 id 가 아닌 member_id로 하는 이유 --> 객체는 Member.id, Order.id 이렇게 구분이 되지만 테이블은 ㄴㄴ --> 다른 테이블에도 id가 있으니까 쿼리 중복 피하기 위해  편의상 테이블 이름과 맞춰줌 



* 실무 : jpa를 활용해 스크립트를 쫙 뽑고 확인해보면서 살짝씩 수정할 부분 수정 





문제상황 : software caused connection abort: recv failed

 커넥션 풀은 일정시간 사용자가 아무런 사용을 하지 않고 있으면 커넥션을 Close하는데 이미 끊어진 커넥션을 다시 Close하여 발생하는 Exception

<-> 연결하려는 Connection이 끊어져있어 발생하는 오류 

해결방법

1.  다시 재접속 해서 Connection을 연결
2. Connection Timeout을 넉넉하게 잡도록 환경변수를 수정한다 



h2 jdbc:h2:tcp://[localhost/~/jpashop](http://localhost/~/jpashop)로 접속하니 오류

TCP 소켓을 통한 접속 방법

 : 얘 자체가 tcp로 연결되어있는 Connection을 통해 콘솔작업 가능하게끔 접속하는거라 Connnection이 끊어진 시점에선 해결 x 

==> 해결:  jdbc:h2:~/jpashop 로 db 파일 ([jpashop.mv](http://jpashop.mv/).db)을 직접 접근해 Connection을 열어두면 ㅇㅋ ! 

--> Application 정상 동작 





문제 상황 :  id to load is required for loading 에러 

--> jpa 에 persist가 되지 않아 발생하는 문제 

```java
    //given
        // 회원가입 (회원 ok )
        Member member = new Member();
        member.setName("회원1");
        member.setAddress(new Address("서울", "경기", "123-123"));
        en.persist(member);

        // 상품 만들기 (상품 ok)
        Book book = new Book();
        book.setName("jpa");
        book.setPrice(10000);
        book.setStockQuantity(10);

        // when (회원이 상품을 주문할 때)
        int orderCount = 2;
        Long orderId = orderService.order(member.getId(),book.getId(), orderCount) ;

```

여기서 book 을 persist (save) 해줘야 회원이 db에 있고, 상품이 db에 있어 그 상품을 회원이 주문하는 관계가 성립되는데 

상품이 등록되지 않으니 회원이 주문을 할 수 없음 





* 문제상황 : 강제로 css, js 파일 복붙해 intellij 가 인식을 못함 

  --> 프로젝트를 강제로 싱크로라이즈 

  --> 프로젝트 rebuild 





error-Failed-to-load-ApplicationContext

뭔가 spring container 구축 과정에서 오류가 발생 

ex) 빈 생성 x 

==> 좀만 내려보면 문제 원인 써있으니 확인하기 







 Could not set value of type [org.hibernate.collection.spi.PersistentBag] 

Hibernate에서 Collection을 형변환 또는 형불일치 해서 발생하는 문제 

Hibernate --> List 반환

--> ArrayList등으로 받으면 문제됨

post쪽에서 받는 collection이 ArrayList

```
//private ArrayList<Likes> likes  = new ArrayList<>() 
private List<Likes> likes  = new ArrayList<>() 

```





