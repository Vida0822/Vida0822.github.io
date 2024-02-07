---
published: true
title:  "Spring security"
categories: Spring
tag: [java, Spring] 
toc: true
author_profile: false 
---



TIL _ 2023-07-04

#### Spring security 



1) 기본 세팅 

[notice.htm] 요청 URL
Ctrl + F11



[스프링 시큐리티]
[1] Spring Web Security

```xml
1) pom.xml
<dependency>
		<groupId>org.springframework.security</groupId>
		<artifactId>spring-security-web</artifactId>
		<version>${org.springframework-version}</version>
	</dependency>
<!--beans 설정 버전까지 한글자 한글자 세팅 제대로 해야함 -->

	<dependency>
		<groupId>org.springframework.security</groupId>
		<artifactId>spring-security-config</artifactId>
		<version>${org.springframework-version}</version>
	</dependency>

	<dependency>
		<groupId>org.springframework.security</groupId>
		<artifactId>spring-security-core</artifactId>
		<version>${org.springframework-version}</version>
	</dependency>

	<!-- https://mvnrepository.com/artifact/org.springframework.security/spring-security-taglibs -->
	<dependency>
		<groupId>org.springframework.security</groupId>
		<artifactId>spring-security-taglibs</artifactId>
		<version>${org.springframework-version}</version>
	</dependency>
```


​		
```xml
[2] security-context.xml 
<security:http pattern="/static/**" security="none"></security:http>	
<security:http pattern="/design/**" security="none"></security:http>	

<security:http> 
   <security:form-login/>  
</security:http>	
	
<security:authentication-manager> 
</security:authentication-manager>
<!-- AithenticationManager 인증 관리자 : 가장 중요한 역할, 다양한 방식의 인증을 처리할 수 있도록 구조 설계됨(인터페이스) 
```


​		   
```xml
[3] web.xml 설정 
1) 
<context-param>
  <param-name>contextConfigLocation</param-name>
  <param-value>
     /WEB-INF/spring/root-context.xml
     /WEB-INF/spring/security-context.xml
  </param-value>
</context-param>
```


```xml
2) 
<filter>
  <filter-name>springSecurityFilterChain</filter-name>
  <filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>
</filter>
<filter-mapping>
      <filter-name>springSecurityFilterChain</filter-name>
      <url-pattern>/*</url-pattern>
</filter-mapping>


```


[4] 시큐리티가 필요한 uri 설계 
   	ㄱ. 게시판 글쓰기는 인증(로그인) 해야만 사용가능 
   	ㄴ. 게시글 작성자는 인증받은 id로 사용가능 
   	ㄷ. 게시글 수정/ 삭제는 작성자 확인 후 가능 
   	ㄹ. 그 외는 인증받지 않도록 모두가 사용 가능 



[5] 시큐리티 개요

   1. 인증(Authentication)과 권한(==인가)(Authorization)

   2) 스프링 시큐리티 구조
      (1) AuthenticationManager 인증관리자 - 가장 중요한 역할, 다양한 방식의 인증을 처리할 수 있도록 구조 설계됨.(인터페이스)
      (2) ProviderManager 제공관리자 - 인증처리를 AuthenticationProvoider 객체를 이용해서 처리를 위임하는 역할.(인터페이스)
      (3) AuthenticationProvoider 인증제공자 - 실제 인증 작업을 진행(처리)하는 역할(인터페이스)
      (4) UserDetailsService 사용자 상세서비스 - 인증된 실제 사용자의 정보와 권한 정보를 처리해서 반환하는 객체(인터페이스)        
      
      * (3) 또는 (4)는 직접 구현할 경우가 있다. 
      
        - 대부분 (4)을 직접 구현 , 새로운 프로토콜이나 인증 구현 방식을 직접 구현하는 경우에는 (3) 구현.
      
          

[6] 접근 제한 설정 - intercept-url (pattern, access  속성) 사용

1) 

```xml
<http>
<intercept-url pattern="url패턴" access="권한 체크(권한명, 표현식)"/>
</http>
```



2. access 속성을 표현식을 사용하지 않을 경우에는 <http use-expression = false> 설정

/sample/member 접근하면 강제로 로그인 페이지로 이동하는 것을 볼 수 있다, 



3) 표현식  p.673 
hasRole() , hasAuthority() - 해당 권한이 있으면 true
hasAnyRole(), hasAnyAuthority() - 여러 권한중에 하나라도 해당 권한이 있으면 true 
principal - 현재 사용자 정보를 의미
permitAll - 모든 사용자에게 허용
denyAll - 모든 사용자에게 거부
inAnomymous() - 익명의 사용자의 경우 (로그인을 하지 않은 경우도 해당)



[7] 실습

```xml

1) 접근 허용 정책 
	<security:intercept-url pattern="/customer/noticeReg.htm" access="isAuthenticated()" />
	<security:intercept-url pattern="/customer/noticeDel.htm" access="hasRole('ROLE_ADMIN')" />
	<security:intercept-url pattern="/**" access="permitAll"/>
	
2) 인 메모리 방식으로 사용자 계정 + 역할(권한) 설정 
	<security:authentication-manager> 
	<security:authentication-provider> <!-- 실제 인증을 처리하는 객체 -->
		<security:user-service>
			<security:user name="hong" authorities="ROLE_USER" password="1234"/>
			<security:user name="admin" authorities="ROLE_USER, ROLE_ADMIN, ROLE_MANAGER" password="1234"/>
		</security:user-service>
	</security:authentication-provider>
</security:authentication-manager>

3) There is no PasswordEncoder mapped for the id "null"

=> password encoding이 필요하다 (spring 5.0~ ) 

password="{noop}1234" => "난 일단 패스워드 인코딩 안주겠다"
```





4) 아래와 같은 오류가 발생할 때 특정 페이지로 이동 시킬 수 있다. 

# HTTP 상태 403 – 금지됨

------

**타입** 상태 보고

**메시지** Forbidden

**설명** 서버가 요청을 이해했으나 승인을 거부합니다.

ㄱ.

```xml
 <security:access-denied-handler error-page="/common/accessError.htm"/>
```

ㄴ. CommonController 추가

ㄷ. 뷰 페이지 /common/accessError



5) (권장) 접근 제한이 된 경우 다양한 처리를 하고 싶다면 AccessDeniedHandeler 인터페이스를 구현하는 것이 좋다 

   

6. 로그인 페이지 - 스프링 시큐리티 제공되는 로그인 페이지...

   -> 사용자 로그인 페이지 사용하도록 설정 

   ```xml
    <security:form-login 
   	    	login-page="/joinus/login.htm"
   	    	authentication-success-handler-ref="customLoginSuccessHandler"
   	    	authentication-failure-url="/joinus/login.htm?error=true" 
   />  
   	    
   ```

* login-page 속성의 url은 반드시 get 방식 요청이라야 된다 (이미 되어있음 @Getmapping)

* form-login
  : Sets up a form login configuration for authentication with a username and password

  ; 즉 로그인 '폼'을 띠우는거 -> 그걸 default 페이지가 아닌 우리 페이지로 설정 : login-page 



7. 로그인 성공 후 특정한 동작을 하도록 제어하고 싶은 경우 ! 

