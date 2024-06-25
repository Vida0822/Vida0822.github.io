---
published: true
title:  "Toby의 스프링부트-Spring Boot 원리"
categories: Java/Spring
tag: [java, Spring] 
toc: true
author_profile: false 

---



# 스프링 부트 시작하기



## 스프링 부트란?

* 스프링 부트 : 자바 프레임워크

* 스프링 프레임워크 (Spring framework)

  ㄴ 스프링 = 프레임워크 

'프레임워크' 

: 밑바닥에서부터 코드를 일일히 작성 안해도 이미 개발된 코드의 큰 단위들을 받아 대규모의 프로젝트를 효율적으로 개발가능

* 나한테 맞는 프레임워크를 선택해서 습득하는 건 오래걸림, 그렇다면 그 선택이 스프링인데의 장점? 
* <-> 하나의 프레임워크만 제대로 해도 된다 



* 스프링 프레임워크 장점 

  1. 방대한 프로젝트 

  : 22개의 카테고리의 수백개의 프로젝트 보유 

  -> 대규모 웹 개발/ 운영을 위한 거의 모든 기술을 제공 

  

2. 끊임없는 개선 

   최근 sw시스템은 점점 거대 + 복잡 

   -> 더 나은 sw 시스템을 위한 다양한 기술과 아키텍쳐 대두 ex) 마이크로서비스 아키텍처, NoSQL, 클라우드 컴퓨팅 및 컨테이너

   ㄴ 이런 기술들에 대응해 빠르게 새로운 프로젝트 출시 ex) Spring Cloud, Spring Native 



* 단점 

  1. 높은 러닝커브 (배우기 힘들고 어려움) 

  : Bean, DI, AOP, 객체지향 설계, 디자인 패턴 등 다양한 개념이해 필요 

  ㄴ Spring Framework은 엔터 프라이즈급 대규모 서비스 개발을 위한 목적으로 개발됨 

  --> 이 큰 서비스를 유연한 확정성을 위해 다양한 기술이 내포될수 밖에 

2. 복잡한 설정

   "Spring framework은 xml 지옥이다"

   ㄴ 간단한 웹 애플리케이션 개발 위해서도 상당한 수준의 설정이 필요 

   <-> Spring Framework은 무겁다, 대기업에서 쓸만한 기술이다 

   BUT! Spring Boot가 출시되며 단번에 문제 해결 : 자동화 된 설정(xml 대신~!!!아 그때 그래서 xml 없앤 코딩 보여주셨구나) , 간편화 된 의존성 관리 등 ('모던화된 프레임워크')



스프링 부트란? 

easy 

just run 

Spring framework을 기반으로! 이 Spring Framework를 보다 손쉽게 활용할 수 있게 지원하는 기술,즉 프로젝트 

ㄴ 설정, 의존성 관리, 애플리케이션 모니터링, 서버의 실행 등을 가볍고 빠르게 수행 가능 

* 설정 간편화를 위한 Auto Configuration 
* 의존성 관리를 위한 Starter Project
* 배포 프로세스 간소화를 위한 Embedded WAS
* 애플리케이션의 모니터링을 위한 Actuator 



[ 퍼블리싱 / 마크업 개발자  ]

: 사용자에게 노출되는 웹 화면 개발 

: 디자인을 html/ css 코드로 옮기는 과정

html, css 를 주로 사용 (js 일부 사용)



[프론트엔드 개발]

- 사용자 화면과 백엔드(DB)와의 중간 커뮤니케이션 역할

- 사용자의 입력,이벤트를 받아 백엔드로 전송

- 백엔드의 데이터를 받아 화면에 노출

- javascript, jquery 등이 전통적으로 많이 사용됨

- 최근에는 VueJS, ReactJS, AngularJS 등 프론트엔드를 위한 Framework 등이 많이 사용 됨

  

[백앤드 개발]

: 웹 서비스의 비즈니스 로직을 처리하는 부분 

 = REST API 개발, 프론트엔드에서 호출하는 API 

이러한 웹서비스를 지탱하는 infraustructure 기술들? 서버 , 클라우드, 스토리지 , 보안 ,클라우드 

브라우저에서의 사용자 요청을 받아 적절하게 처리 : 로직처리, dB연동 , 외부 시스템 연동(Mail,CRM)



[DB 설계/ 운영]

웹 서비스의 데이터가 저장될 DB 설계하고 DBMS 운영 관리 

데이터는 웹 서비스의 가장 중요한 요소 중 하나 

DB 분석 / 설계와 DBMS 운영은 다른 역할 

ㄴ 분석 / 설계 와 운영은 다름 

회사에 따라 같이 진행하기도하고... 백앤드 개발자가 설계하기도 하고 

RDBMS로는 Oracle,MySQL  , NoSQL 새로나옴 



[시스템 엔지니어링, 인프라 엔지니어링]

: 웹 서비스가 운영될 기간 인프라를 설계하고 운영 

서버, 네트위킹, 스토리지, 보안 등 설계, 구축, 운영

* 기존에는 온 프레미스 기반의 시스템 운영
* 최근에는 클라우드 및 컨테이너 기반의 시스템 운영으로 전환 중 ***
  - AWS, Azure, CGP, NCP...
  - Docker, Kubernetes



SW Framework 

: SW 개발을 효율적으로 하기위한 반제품

-> 특정 분야(웹 개발, 앱개발, 임베디드 개발) 의 SW 개발에 필요한 공통기능 제공 



-> 개발자는 Framework 위에 필요한 기능을 추가해 전체 어플리케이션 완성 

![image-20230722164813128](D:\Programming\github.io\images\2023-07-11-SpringBoot\image-20230722164813128.png)

Web Framework

: Web 개발을 위한 반제품 

=> Web 어플리케이션에 필요한 공통기능 제공 

* Web 어플리케이션에 필요한 공통기능 

  : 보안, HTTP 요청처리, DB 연동 등 이런 기능을 미리 만들어 제공

=> 사용자는 이 Framework 위에 서비스에 필요한 비즈니스 로직만 구현해서 올리면 



Library vs Framework 

공) 

- 재사용 가능한 미리 구현된 코드(모듈) 제공
- 특정 목적을 위해 구현된 코드를 사용함으로써 효율적인 개발 가능 

차) 'SW 제어의 흐름을 누가 결정하는가 ' 

Library : 제어권 사용자 코드에 있음  -> 내 코드 가 프로그램 흐름이고 그 위에서 라이브러리 함수호출 

Framework : 제어권 Framework에 있음 

-> Framework을 실행하면 그 실행부터 종료까지 그 흐름이 해당 Framework에서 정해준대로 결정됨 

-> 우리가 그 위에 적절한 메서드를 추가하여 우리 코드가 약속된 시기, 순서에 호출되서 실행되게 되는거임  (그래서 우리 코드가 언제 사용되는지 확인 어려운 경우 많음 대뜸 우리 코드 위해 controller 붙이면 controller가 호출되는 순서에 우리 코드가 호출되는거임 <-> 우리가 컨트롤러가 호출되는 순서나 흐름을 짜지 않았잖아?)

![image-20230722165342956](D:\Programming\github.io\images\2023-07-11-SpringBoot\image-20230722165342956.png)

ㄴ 처음 Framework를 학습할 땐 전체그림이 그려지지 않음 : 설정 한줄 , 메소드 하나 추가했을뿐인데 어떻게 이렇게 동작하지? 이 메서드를 언제 왜호출해주는가 ?

그래서 우린 jsp 프로젝트를 통해 그 흐름을 먼저 파악한 후 스프링 프레임워크를 사용한거!! 오오 커리큘럼 미쳤다 



Framework 공부법 

1. Framework가 제공하는 계약(약속) 하나씩 이해 

   : 당신이 특정 위치에 특정 방식으로 코드 추가하면 그 코드는 이런 의미고 나는 이렇게 동작합니다! 

2. Framework의 내부동작 매커니즘 이해 





[Postman]

: Rest API를 통합 관리하기 위한 SW 

=> Spring Boot로 구현하는 API를 테스트하기 위한 용도로 사용 

*회원가입 안해도 됨 (근데 하면 테스트했던 이력들이 남아있음)

특정 서버에 요청하고 응답하는 과정을 보여주는데 최적화된 SW 

1. spring initializr를 활용해 Spring Boot 프로젝트 생성 및 다운로드 

*spring initializr : Spring Boot 프로젝트를 쉽게  구성할 수 있도록 지원하는 사이트

2. 다운로드 한 Spring Boot 프로젝트를 IntelliJ에서 import 

3. 추가 코드 개발 (프레임워크 위에 쌓는거니까)

4. Spring Boot 애플리케이션 실행 

5. 웹 브라우저 또는 Postman을 활용하여 테스트 (우리는 그냥 ctrl+f11로 브라우저 띄워서 직접 동작해가면서 함 )



--- 토비 ---

스프링부트는 스프링을 사용하는 방법에 대한 '강한 의견'이 반영된 프레임

=> 잘 활용하려면 이 강하게 반영되어있는 스프링 사용방법(동작 방식)을 정확히 이해해야함

=> 나만의 스프링 부트를 만든다 ('커스터마이징')

=> 겸사겸사 스프링 프레임워크 





## 개발환경 세팅

#### 스프링 부트 버전 결정(2.x.x, 3.x.x) 



#### JDK

-> 스프링 부트 버전 알아서 jdk 설정해줌 : 8 , 11, 17 : 오픈 소스(공개) jdk 다운로드 후 설치 

* jdk 직접 다운로드 받는거 단점 

  : 여러가지 프로젝트를 하다보면 여러 버전의 jdk를 하나의 개발장치에 설치해야하는 경우 

  => 계속 업데이트 & 어떤 jdk를 활성화시킬지 계속감독 해야함 



* 아무런 지정을 하지 않았을때 default 버전으로 사용하겠다

```bash
// cmd
sdk install java 11. 0. 17
-> do you want to set default? 
```



* 프로젝트별로 jdk 버전 설정 

```bash
cd myproject/ 

sdk use java 17.0.5-amzn //17 버전 활성화 (java -version)
```





#### IDE (통합 개발환경)

* IntelliJ IDEA 



#### 프로젝트 초기 세팅

1. Spring initializr 에서 프로젝트 생성 



- 스프링 부트 2.7.14 기준 default setting 

![image-20230727091953084](D:\Programming\github.io\images\2023-07-11-SpringBoot\image-20230727091953084.png)



* 스프링 부트 3.0.2 기준 default setting (17 부터 됨)





2. 다운받은 프로젝트 압축풀기 => IDE에서 project open 

"우측 하단에 빌드가 돌아가는 메시지가 보일텐데요. 일단 그게 다 끝날 때까지 기다리세요. 처음 오픈하면 시간이 제법 걸립니다."





#### 실행(Build & Run) 테스트 

src>main>폴더>~Application  => 우클릭 후 main 실행 



- 성공화면

![image-20230727092953195](D:\Programming\github.io\images\2023-07-11-SpringBoot\image-20230727092953195.png)



#### 오류 

1. 빌드 오류 

```bash
finished with non-zero exit value 1 
```

> 해결방법 : Gradle 빌드 도구 수정

1. [File > Settings] 메뉴 클릭 (맥 기준 단축키 : Command + ,)
2. [Build, Excution, Deployment > Build Tools > Gradle] 클릭
3. Build and run using과 Run tests using이 아마도 **Gradle(Default)**로 되어있을텐데, 이것을
   **Intellij IDEA**로 바꿔준다.

4. 위 과정을 마친 후 프로젝트를 실행했을 때, 실행이 안된다면 Terminal 에서 `java -version` 을 입력하여 자바 버전을 확인 후 `Gradle JVM 의 버전`을 맞춰준다. 

[신희민] [오전 8:40] https://velog.io/@developerjun0615/Spring-Intellij-%EC%8B%A4%ED%96%89%EC%8B%9C-finished-with-non-zero-exit-value-1-%EC%98%A4%EB%A5%98



2. 이미 다른 프로젝트/ 컴포넌트에서 사용중인 포트 

```bash
Port 8080 was already in use
```

> 해결방법 

1. Spring boot 프로젝트 포트번호 수정 

application.properties에 8081번으로 포트를 변경한다는 코드를 입력해준다.

```properties
#configuring port   
server.port = 8081
```



2. 실행중인 :8080 포트 죽이기 

**Mac**

```java
// 8080포트 조회
lsof -i tcp:8080

// 8080포트 강제 종료 
kill $(lsof -t -i:8080)
```

 

**Window**

```java
// 현재 사용하고 있는 8080 포트를 사용하는 네트워크 통계 정보를 출력한다. 
netstat -ano | findstr 8080 

// 위 명령어를 통해 출력된 목록의 process_id를 입력하여 강제 종료시킨다. 
taskkill /F /pid [process_id]
```



[Port 8080 was already in use 에러 해결방법 — 유리코딩 (tistory.com)](https://yuricoding.tistory.com/93)



3. **Cannot resolve symbol**

file - Invalidate Caches -> Invalidate and Restart

#### HelloController 작성해서 매핑 테스트 

src>main>폴더> HelloController.java 클래스 생성 

![image-20230727093223328](D:\Programming\github.io\images\2023-07-11-SpringBoot\image-20230727093223328.png)

```java
// 컨트롤러 클래스 코드 
package tobyString.helloboot;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HelloController {

    @GetMapping(value = "/hello")
    public String hello(String name){

        return "Hello"+name ;
    }
}
```

=> 코드 작성 후 재실행(run)



브라우저에 설정해준 요청 url 패턴에 맞춰 브라우저에 url 입력 

```bash
http://localhost:8081/hello?name=Spring
```



* 성공시 화면

![image-20230727092553254](D:\Programming\github.io\images\2023-07-11-SpringBoot\image-20230727092553254.png)

* 실패시 화면 

![image-20230727093947890](D:\Programming\github.io\images\2023-07-11-SpringBoot\image-20230727093947890.png)

#### API 테스트 환경 세팅 

브라우저 테스트로 눈에 보이는 화면만 보는게 아니라 보이지 않는 뒤에서 왔다갔다하는 과정을 자세히 알아야함 

=> 'API 테스트'

1. 브라우저 개발자 도구 

2. IntelliJ의 기능인 .http 파일 (*http 기능은 IntelliJ Ultimate 버전에서 지원)
3. PostMan 



##### http 요청과 응답 

'웹 요청은 어떤식으로 보내야하고 응답은 어떻게 받는가'

=> 이걸 정의해놓은 표준기술 : 'http' ; 프로토콜, 대화방식 



* 요청과 응답의 구조 

Request 

1. Request Line : Method(Get,Post,Delete) , Path(포트 뒤 경로), HTTP version 
2. Header : accept (나는 특정 형식의 응답 데이터만 받겠다 - default : "*/" - 다 받음 )
3. Message Body : post, put 같이 Message Body가 동반되는 경우 



Response 

1. Status Line : Http Version, Status Code, Status Text 
2. Headers : content type (이 응답데이터는 html이다, text이다, json이다)
3. Message Body 



```bash
// REQUEST 

❯ http -v ":8080/hello?name=Spring"

GET /hello?name=Spring HTTP/1.1 
Accept: */* 
Accept-Encoding: gzip, deflate 
Connection: keep-alive 
Host: localhost:8080 
User-Agent: HTTPie/3.2.1 
Cookie:ajs_anonymous_id=286138aa-a4f1-451c-bf77-5e1a720ba63d; _ga=GA1.1.500140860.1689027439;
```



```bash
// RESPONSE 

HTTP/1.1 200  // 400 : 요청부터 잘못 , 500 : 서버 프로그램 문제 
Connection: keep-alive 
Content-Length: 12 
Content-Type:text/plain;charset=UTF-8 
Date: Thu, 01 Dec 2022 01:45:15 GMT 
Keep-Alive: timeout=60 
Hello Spring
```





# 독립 실행형 어플리케이션 

### 섹션 3. 서블릿 컨테이너

1. 서블릿 등록 & 매핑 

```java
public class HellobootApplication {

	public static void main(String[] args) {
		TomcatServletWebServerFactory serverFactory = new TomcatServletWebServerFactory(8081);
		// onStartup
		WebServer webServer = serverFactory.getWebServer(servletContext -> {
			// interface 가 구현해야할 클래스를 람다식 형태로 넣어줌
			// servletContext : ServletContextInitializer - 서블릿을 서블릿 컨테이너에 추가해주는 클래스 (서블릿을 등록하는데 필요하는 작업을 수행)	

			servletContext.addServlet("hello", new HttpServlet() {  // 서블릿 등록 
                
				@Override
				protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
					
					resp.setStatus(200); // 응답 구성 - 응답 코드
					resp.setHeader("CONTENT_TYPE", "text/plain");
				
					String name = req.getParameter("name") ;
					
					resp.getWriter().println("Hello Servlet"+name);
					// Writer : Object를 문자열 응답으로 만들어냄
				}
			}).addMapping("/hello"); // 서블릿 매핑 
		}) ;
		webServer.start();
	}

}
```







* Front Controller 사용 

```java
public class HellobootApplication {

	public static void main(String[] args) {
		TomcatServletWebServerFactory serverFactory = new TomcatServletWebServerFactory(8081);
		// onStartup
		WebServer webServer = serverFactory.getWebServer(servletContext -> {

			servletContext.addServlet("frontController", new HttpServlet() {
				@Override
				protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
					// 인증, 보안, 다국어처리 ...


					// 매핑
					if (req.getRequestURI().equals("/hello")&&req.getMethod().equals(HttpMethod.GET.name())) {
						String name = req.getParameter("name");

						resp.setStatus(HttpStatus.OK.value()); // 응답 구성 - 응답 코드
						resp.setHeader(HttpHeaders.CONTENT_TYPE, MediaType.TEXT_PLAIN_VALUE);
						resp.getWriter().println("Hello Servlet" + name);
						// Writer : Object를 문자열 응답으로 만들어냄
					} else if (req.getRequestURI().equals("/user")) {
						//
					}else {
						resp.setStatus(HttpStatus.NOT_FOUND.value()); // 404
					}

				}
			}).addMapping("/*"); // 이때부터 frontController 역할을 맡게 됨
		}) ;



		webServer.start();
	} // main 

}
```



↓

Handler 코딩 분리 (FrontController 안에 모든 핸들러 코딩 구현할 순 없음)

=> 기존에 있던 HelloController 사용 

```java
public class HellobootApplication {

	public static void main(String[] args) {
		TomcatServletWebServerFactory serverFactory = new TomcatServletWebServerFactory(8081);
		// onStartup
		WebServer webServer = serverFactory.getWebServer(servletContext -> {
			// interface 가 구현해야할 클래스를 람다식 형태로 넣어줌
			// ServletContextInitializer : 서블릿을 서블릿 컨테이너에 추가해주는 클래스 (서블릿을 등록하는데 필요하는 작업을 수행)
			// 		ㄴ servletContext
			

			servletContext.addServlet("frontController", new HttpServlet() {
				@Override
				protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
					// 인증, 보안, 다국어처리 ...

					/* 공통기능 코딩~ */

						// 매핑 : 어떤 로직을 수행하는 코드를 호출할 것인가
					if (req.getRequestURI().equals("/hello")&&req.getMethod().equals(HttpMethod.GET.name())) {
						// 바인딩 : 직접적으로 웹 요청과 응답을 처리하는 코드는 핸들러에 넣지 않고 (요청, 응답을 직접적으로 핸들러에 넣지않고)
						//  웹 요청을 가지고 이걸 처리하는 로직 코드에서 사용할 수 있도록 새로운 자바 데이터 타입으로 웹 요청 정보를 변환해서 넣어주는것
						// ex) form을 통해 다양한 정보들 -> dto로 만들어서 넘겨주는 것 !
                        String name = req.getParameter("name");       
                        HelloController helloController = new HelloController();
						String ret = helloController.hello(name); // HelloController안의 hello()를 실행해 문자열을 리턴받은거임

						
						resp.setStatus(HttpStatus.OK.value()); // 응답 구성 - 응답 코드
						resp.setHeader(HttpHeaders.CONTENT_TYPE, MediaType.TEXT_PLAIN_VALUE);
						resp.getWriter().println(ret);
						// Writer : Object를 문자열 응답으로 만들어냄
					} else if (req.getRequestURI().equals("/user")) {
						//
					}else {
						resp.setStatus(HttpStatus.NOT_FOUND.value()); // 404
					}

				}
			}).addMapping("/*"); // 이때부터 frontController 역할을 맡게 됨
		}) ;

		webServer.start();
	} // main
} // HellobootApplication
```



* Dispatcher Servlet 사용 

```java
		WebServer webServer = serverFactory
				.getWebServer(servletContext -> {
			/*servlet container 구성 시작 ~ */

			// application context -> Spring container
			GenericWebApplicationContext applicationContext = new GenericWebApplicationContext();

			// object를 직접 만들어서 넣어줬던 servlet context와 다르게 이것도 가능하지만!
			// 스프링은 일반적으로 어떤 클래스로 bean 객체를 만들건지 클래스 정보를 등록해줌 (설정 정보 등록)
			applicationContext.registerBean(HelloController.class);
			applicationContext.registerBean(SimpleHelloService.class);

			// 자기한테 등록된 설정정보를 반영해 spring container 구성 (빈 객체 생성)
			applicationContext.refresh();
			/*
			servletContext.addServlet("frontController"
					, new HttpServlet() {
				@Override
				protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
					// 공통기능 코딩~ : 인증, 보안, 다국어처리 ..

					if (req.getRequestURI().equals("/hello")&&req.getMethod().equals(HttpMethod.GET.name())) {

						String name = req.getParameter("name");

						HelloController helloController = applicationContext.getBean(HelloController.class) ;
						// 스프링 컨테이너 안에 HelloController 클래스 타입의 빈 객체가 하나뿐이라면 자료형만 지정해주는것만으로도 해당 빈 객체 호출
						// servlet container는 helloController에 helloController 를 생성하던 뭐하던 신경쓰지 않고 그냥 스프링 컨테이너에서 갖다 씀
						String ret = helloController.hello(name); // HelloController안의 hello()를 실행해 문자열을 리턴받은거임

						resp.setContentType(MediaType.TEXT_PLAIN_VALUE);
						resp.getWriter().println(ret); // 응답 body 에 작성
					} else {
						resp.setStatus(HttpStatus.NOT_FOUND.value()); // 404
					}

				} // service
			}).addMapping("/*");  // addServlet // 이때부터 frontController 역할을 맡게 됨
			*/
			servletContext.addServlet("dispatcherServlet"
					, new DispatcherServlet(applicationContext) {
					}).addMapping("/*");  // addServlet
					// 이때부터 frontController 역할을 맡게 됨
		}) ; // getWebServer
```





* 두번째 작업인 Servlet Container 를 만들고 초기화 하는 과정을 Spring Container 초기화 과정 중 일어나도록 수정 : Spring boot의 방식 

```java
GenericWebApplicationContext applicationContext = new GenericWebApplicationContext(){
			// 익명 클래스
	@Override
	protected void onRefresh() {
		super.onRefresh();
		// 두번째 작업인 Servlet Container 를 만들고 초기화 하는 과정을 Spring Container 초기화 과정 중 일어나도록 수정 : Spring boot의 방식
		TomcatServletWebServerFactory serverFactory = new TomcatServletWebServerFactory(8081);
		WebServer webServer = serverFactory
				.getWebServer(servletContext -> {
			servletContext.addServlet("dispatcherServlet", new DispatcherServlet(this)).addMapping("/*"); 
		// 매핑(요청 넘기기), 응답 body 작성 등 직접 코드 안해줘도 알아서 해줌 (매핑 정보만 빈객체에 작성해주면 됨)
		}) ; // getWebServer
		webServer.start();
	} // onRefresh
};
applicationContext.registerBean(HelloController.class);
applicationContext.registerBean(SimpleHelloService.class);
applicationContext.refresh(); // 템플릿 메서드  => 훅 메서드: onRefresh
```





### 7. 스프링 컨테이너 구성정보 

코드를 Spring container의 Compenent 를 등록하고, 의존하고 있다면 어느 시점에 어떻게 주입해 줄 것인가를 스프링 컨테이너에 '구성정보'로 제공 (이전 : xml) 

* 이전 : 클래스 정보를 registerBean 

#### 1. java 코드로 ! 

FactoryMethod : object 생성하는 로직을 갖고 있는 메서드 => 빈 생성 + 조립 

```java
@Configuration  // 이게 Bean 등록정보(스프링 컨테이너 구성정보)를 가진 클래스임을 알려줌 => "아 여기 BeanAnnotaion이 붙은 Factory 메서드가 있겠구나)
public class HellobootApplication {

	@Bean  // 빈 객체 생성
	public HelloController helloController(HelloService helloService){
		return new HelloController(helloService); // 의존 정보 등록
	}

	@Bean
	public HelloService helloService(){
		//SimpleHelloService로 리턴은 좋은 코딩이 아님 (확장성을 반영한 interface 타입 리턴이 better)
		return new SimpleHelloService();
	}

     
	public static void main(String[] args) {

		// application context -> Spring container 생성
	//	GenericWebApplicationContext applicationContext = new GenericWebApplicationContext(){

		AnnotationConfigWebApplicationContext applicationContext = new AnnotationConfigWebApplicationContext(){
			// 자바 코드를 설정정보로 인식하는 Spring Container
			@Override
			protected void onRefresh() {
				super.onRefresh();
				TomcatServletWebServerFactory serverFactory = new TomcatServletWebServerFactory(8081);
				WebServer webServer = serverFactory
						.getWebServer(servletContext -> {
		
							servletContext.addServlet("dispatcherServlet"
									, new DispatcherServlet(this)).addMapping("/*");  // addServlet
						}) ; // getWebServer
				webServer.start();
			} // onRefresh
		};
		// applicationContext.registerBean(HelloController.class);
		// applicationContext.registerBean(SimpleHelloService.class);
		// 이거 대신 java 코드 구성정보 담은 클래스를 등록해줘야함
		applicationContext.register(HellobootApplication.class);
		applicationContext.refresh(); // 템플릿 메서드  => 훅 메서드: onRefresh


	} // main
} // HellobootApplication
```



중요 : @Configuration

=> 처음 등록됨 ! Bean Factory 메서드를 가지는 이상으로 전체 Spring Container 를 구성하는데 많은 정보를 담을 수 있음 

```bash
Autowiring by type from bean name 'helloController' via factory method to bean named 'helloService'
```





#### 2. 스캐너 : 클래스 위에 직접 붙여줌 

빈객체로 등록하고 싶은 클래스 위에 직접 사용해줌 (위: 호출하는데서 작성)

=> @Component : "나는 Spring Container 에 들어가는 Component(빈객체)야"



1) 빈객체로 등록하고자 하는 클래스 위에 @Component 

2) @ComponentScan : @Component 이 붙은 클래스를 찾아 빈객체로 생성+조립해라 !

    (@Configuration: Container 구성정보  아래에 작성)



=> 간단,편리하지만 Bean으로 어떤 객체들이 등록되는지 찾지 못하는 단점

: 패키지 구성 잘하고 모듈 잘 나눠서 개발하면 ㄱㅊ 



```java
@MySpringBootApplication
public class HellobootApplication {

     
	public static void main(String[] args) {

	// application context -> Spring container 생성
	//	GenericWebApplicationContext applicationContext = new GenericWebApplicationContext(){

		AnnotationConfigWebApplicationContext applicationContext = new AnnotationConfigWebApplicationContext(){
			// 자바 코드를 설정정보로 인식하는 Spring Container
			@Override
			protected void onRefresh() {
				super.onRefresh();
				TomcatServletWebServerFactory serverFactory = new TomcatServletWebServerFactory(8081);
				WebServer webServer = serverFactory
						.getWebServer(servletContext -> {
		
							servletContext.addServlet("dispatcherServlet"
									, new DispatcherServlet(this)).addMapping("/*");  // addServlet
						}) ; // getWebServer
				webServer.start();
			} // onRefresh
		};
		// applicationContext.registerBean(HelloController.class);
		// applicationContext.registerBean(SimpleHelloService.class);
		// 이거 대신 java 코드 구성정보 담은 클래스를 등록해줘야함
		applicationContext.register(HellobootApplication.class);
		applicationContext.refresh(); // 템플릿 메서드  => 훅 메서드: onRefresh


	} // main
} // HellobootApplication
```



### 8. MetaAnnotation 

: Annotaion (코드)위에 붙은 Annotaion 

=> 이 MetaAnnotaion을 사용하면 그 아래에 있는 하위 Annotaion 들도 자동 반영됨 

=> '계층형 아키텍처'

: @Component 안쓰고 굳이? Spring Bean Object로 등록되는건 기본이고 이 객체가 어떤 종류(역할)인지도 나타내주고 싶어! (Controller, Service) 

=>  stereo Type Meta Annotation ex) RestController 

: 선언부 들어가보면 @Component 있음 => 이거 사용해도 Spring Container가 CompentScan으로 찾아내 빈 객체 생성 

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Component
public @interface Service {} 
```



ex) @RestController (Meta) - @Controller (Meta)  - @Component 

​		ㄴ @ResponseBody  => 코드에 넣어줬던거 지워줘도 됨



```java
// @RequestMapping("/hello") // 이 안에 매핑 정보가 담겨져있는 메서드가 있다 (/hello 요청을 처리하는 메서드가 있다) 
// @MyComponent
@RestController
public class HelloController {

    // SimpleHelloService simpleHelloService = new SimpleHelloService();
    private final HelloService helloService; // 주입

    public HelloController(HelloService helloService) {
        this.helloService = helloService;
    }

  // @GetMapping
    @GetMapping("/hello")
   // @ResponseBody
    //  return 된 String 값을 그대로 응답의 Body에 추가해주는 애노테이션 => @RestController 로 대체
    public String hello(String name){
        return helloService.sayHello(Objects.requireNonNull(name)); // 이렇게만 하면 에러! Controller에서 반환하는 String return 값을 스프링은 기본적으로 view 페이지 이름으로 인식
        // null 이면 예외를 던지고 아니면 값을 그대로 넘김 (null이 아닌경우에만 사용되도록)
    }
}
```





### 9. 서블릿 컨테이너도 빈으로 등록 

* TomcatServletWebServerFactory, DispatcherServlet  => Application이 시작되려면 필요한 두 object

  => Spring Bean으로 등록해 Spring Container 가 관리
  
   

=>**bean factory method 활용**  (다음 자동 구성 기능 수업에서 수정 예정 )

```java
@MySpringBootApplication
public class HellobootApplication {
	@Bean
	public ServletWebServerFactory serverFactory (){
		return new TomcatServletWebServerFactory(8081);
	}
	@Bean
	public DispatcherServlet dispatcherServlet(){
		return new DispatcherServlet() ; // 여기선 Spring Container (application Context)를 어떻게 넣어줌?
	}

	public static void main(String[] args) {

		// application context -> Spring container 생성
	//	GenericWebApplicationContext applicationContext = new GenericWebApplicationContext(){

		AnnotationConfigWebApplicationContext applicationContext = new AnnotationConfigWebApplicationContext(){
			// 자바 코드를 설정정보로 인식하는 Spring Container
			@Override
			protected void onRefresh() {
                super.onRefresh();
                // 	TomcatServletWebServerFactory serverFactory = new TomcatServletWebServerFactory(8081);
                ServletWebServerFactory serverFactory = this.getBean(ServletWebServerFactory.class) ;
                DispatcherServlet dispatcherServlet = this.getBean(DispatcherServlet.class) ;
				// dispatcherServlet.setApplicationContext(this); 
                // => 이거 지정 안해줘도 동작 잘함  : Bean의 생명주기 메서드 때문! 
                
                
                WebServer webServer = serverFactory.getWebServer(servletContext -> {
              //servletContext.addServlet("dispatcherServlet",new DispatcherServlet(this)).addMapping("/*"); 
                    servletContext.addServlet("dispatcherServlet", dispatcherServlet).addMapping("/*");  
                }) ; // getWebServer
                webServer.start();
            } // onRefresh
		};
		// applicationContext.registerBean(HelloController.class);
		// applicationContext.registerBean(SimpleHelloService.class);
		// 이거 대신 java 코드 구성정보 담은 클래스를 등록해줘야함
		applicationContext.register(HellobootApplication.class);
		applicationContext.refresh(); // 템플릿 메서드  => 훅 메서드: onRefresh


	} // main
} // HellobootApplication
```



 ㄴ *DispatcherServlet 에  Spring Container (application Context)를 어떻게 넣어줌?*

자동으로 넣어짐 ! By  **lifecycle method**

* DispatcherServlet : ApplicationContextAware interface를 구현함 

  => setApplicationContext()

```java
public interface ApplicationContextAware extends Aware {

	/**
	 * Set the ApplicationContext that this object runs in.
	 * Normally this call will be used to initialize the object.
	 * <p>Invoked after population of normal bean properties but before an init callback such
	 * as {@link org.springframework.beans.factory.InitializingBean#afterPropertiesSet()}
	 * or a custom init-method. Invoked after {@link ResourceLoaderAware#setResourceLoader},
	 * {@link ApplicationEventPublisherAware#setApplicationEventPublisher} and
	 * {@link MessageSourceAware}, if applicable.
	 * @param applicationContext the ApplicationContext object to be used by this object
	 * @throws ApplicationContextException in case of context initialization errors
	 * @throws BeansException if thrown by application context methods
	 * @see org.springframework.beans.factory.BeanInitializationException
	 */
	void setApplicationContext(ApplicationContext applicationContext) throws BeansException;

}
```



setApplicationContext()

: lifecycle method' - Bean을 Container가 등록하고 관리하는 중 Container가 관리하는 Object를 bean 에 주입  

=> 이 interface를 구현한 클래스(dispatcherServlet)가 빈으로 등록되면, 어떻게 등록되던 스프링 컨테이너는 해당 인터페이스의 setter 메서드로 applicationContext를 알아서 주입해준거암 

=> 이런 종류의 interface를 구현해놓으면 Spring Container는 이 객체가 등록되는 시점에 setter 메서드로 넣어줘야겠구나! (이 setter 메서드 자체를 Spring Container 가 호출하는 거임! 

(명시적으로 setter 메서드 사용해서 주입 안시켜줘도 됨)





```java
package tobyspring.helloboot;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

\
@RestController
public class HelloController implements ApplicationContextAware {

  
    private final HelloService helloService;
    private ApplicationContext applicationContext ; // 자기 자신이지만 Bean 처럼 등록해서 관리
    // 생성자를 통해 HelloController가 초기화될 때 final 이면 그 필드도 초기화 되어야하는데
    // ApplicationContextAware 는 객체가 생성된 후 메서드 호출(setter)을 통해 주입(조립)되기 때문에


    public HelloController(HelloService helloService) {  // 주입
    this.helloService = helloService;
    }
    
    // setter 주입 
    @Override
    public void setApplicationContext(ApplicationContext applicationContext) throws BeansException {
        // 스프링 컨테이너가 초기화 되는 시점에 이거 실행됨
        System.out.println(applicationContext);
        this.applicationContext=applicationContext;
    }

	// 생성자 주입 
   /*
   private final ApplicationContext applicationContext
  
  	public HelloController(HelloService helloService, ApplicationContext applicationContext) {  // 주입
        this.helloService = helloService;
        this.applicationContext=applicationContext;

        System.out.println(applicationContext);
    }
*/
    @GetMapping("/hello")
   // @ResponseBody
    // 이거 추가해줘야함 : return 된 String 값을 그대로 응답의 Body에 추가해주는 애노테이션 => @RestController 로 대체
    public String hello(String name){
        if(name == null || name.trim().length() == 0)throw new IllegalArgumentException();
        return helloService.sayHello(name); // 이렇게만 하면 에러! Controller에서 반환하는 String return 값을 스프링은 기본적으로 view 페이지 이름으로 인식
        // null 이면 예외를 던지고 아니면 값을 그대로 넘김 (null이 아닌경우에만 사용되도록)
    } // hello
}

```





### 10. Application 과 해당 Application을 실행시켜주는 코드 분리 

* application - visible(웹 마다 다른 어플리케이션 실행부) 

**HelloBootApplication**

```java
@MySpringBootApplication
public class HellobootApplication {
	@Bean
	public ServletWebServerFactory serverFactory (){
		return new TomcatServletWebServerFactory(8081);
	}
	@Bean
	public DispatcherServlet dispatcherServlet(){
		return new DispatcherServlet() ; // 여기선 Spring Container (application Context)를 어떻게 넣어줌?
	}

	public static void main(String[] args) {
		// MySpringApplication.run(HellobootApplication.class, args);
		SpringApplication.run(HellobootApplication.class, args); // refactoring 
	} // main => 스프링 부트 최종코드 !
} // HellobootApplication

```

=> 각기 다른 설정정보를 갖고 있는 클래스 

: @Configuration 과 @ComponentScan 을 갖고있는 클래스 



* application 실행시키는 클래스 - invisible (spring boot가 내부적으로 웹 어플리케이션을 다루는 공통 코드)

**MySpringBootApplication**

* 실행 메서드 분리 : run() 

```java
private static void run(Class<?> applicationClass, String... args) {
    // 설정정보 클래스 (어플리케이션 실행부) 클래스 자체를 전달받음 
    
	AnnotationConfigWebApplicationContext applicationContext = new AnnotationConfigWebApplicationContext(){
			@Override
			protected void onRefresh() {
				super.onRefresh();
				ServletWebServerFactory serverFactory = this.getBean(ServletWebServerFactory.class) ;
				DispatcherServlet dispatcherServlet = this.getBean(DispatcherServlet.class) ;

				WebServer webServer = serverFactory
						.getWebServer(servletContext -> {
							servletContext.addServlet("dispatcherServlet", dispatcherServlet).addMapping("/*");  // addServlet
						}) ; // getWebServer
				webServer.start();
			} // onRefresh
		};
		applicationContext.register(applicationClass);
		applicationContext.refresh(); // 템플릿 메서드  => 훅 메서드: onRefresh
	}
```



*  재사용해야함 

1. 스프링 컨테이너 + 서블릿 컨테이너 초기화해  

2. 어플리케이션의 설정정보를 스프링 컨테이너로부터 넘겨받아 기본적으로 웹을 실행시켜줌 

=> 다른 main이 되는 클래스에서도 재사용 가능

 :Spring Boot에선 형식에 맞는 각기다른 웹 어플리케이션을 넘겨받으면, 이렇게 동작시켜줌 



<-> 추가적인 설정정보만 application에 적어주면, 

1. 우선 ComponentScan및 설정정보에 작성된 정보들로 Spring Bean 객체 등록해주고

2. 이 메서드에서 서블릿 컨테이너와 스프링 컨테이너를 생성해 등록된 빈들로 구성해 실행시켜주고

3. 요청을 받아 바인딩해 설정된 매핑 핸들러로 넘겨주고, 

4. 전달받은 응답 바디(Response)에 부가적인 정보를 붙여 클라이언트로 전달 

=> 이걸 스프링 부트가 우리 안보이게 대신해줌 



### 섹션 5. DI와 테스트, 디자인 패턴

* 개발자 테스트 : 개발자가 자기가 만든 코드가 잘 돌아가는지 테스트

![image-20230810194105952](D:\Programming\github.io\images\2023-07-11-SpringBoot\image-20230810194105952.png)





1. API 테스트 : 어플리케이션이 돌아가는지 (요청과 응답을 정상적으로 주고받는지) 

* 수행속도 down (요청과 응답을 다 parsing 해야해서)

* 서버 미리 시작해야함 

  ㄴ 테스트 하고자 하는 어플리케이션 실행 후 검증 실행해야 함 안그럼 예외 발생( I/O error on GET request for "http://localhost:8081/hello": Connection refused)

```java
package tobyString.helloboot;

import org.assertj.core.api.Assertions;
import org.junit.jupiter.api.Test;
import org.springframework.boot.test.web.client.TestRestTemplate;
import org.springframework.http.HttpHeaders;
import org.springframework.http.HttpStatus;
import org.springframework.http.MediaType;
import org.springframework.http.ResponseEntity;
import org.springframework.web.client.RestTemplate;

public class HelloAPITest {

    @Test
    void helloApi(){
        TestRestTemplate rest = new TestRestTemplate();
        // RestTemplate : API 요청을 호출해 응답을 가져와 수행할 수 있음 (요청과 응답 객체를 갖고 있는 클래스)
        // => 이걸 Test 목적으로 응답값 그대로를 가져오는 클래스 : TestRestTemplate (테스트 용 -> 요청과 응답에 대한 정보가 더 자세히 나타나있음)

        // ResponseEntity : 웹 응답의 모든 요소를 담고 있는 객체 => String : Body 부분이 String임(->Json, Dto..)
        // => 따로 cast 안해줘도 String 으로 돌려줌
        ResponseEntity<String> res
                = rest.getForEntity("http://localhost:8081/hello?name={name}", String.class, "Spring") ;



        // 응답 검증 (상태코드, 컨텐츠 타입,응답 바디)
        // => 테스트 하고자 하는 어플리케이션 실행 후 검증 실행해야 함 안그럼 예외 발생 ( I/O error on GET request for "http://localhost:8081/hello": Connection refused)
        Assertions.assertThat(res.getStatusCode()).isEqualTo(HttpStatus.OK) ;

      //  Assertions.assertThat(res.getHeaders().getFirst(HttpHeaders.CONTENT_TYPE)).isEqualTo(MediaType.TEXT_PLAIN_VALUE) ;
        Assertions.assertThat(res.getHeaders().getFirst(HttpHeaders.CONTENT_TYPE)).startsWith(MediaType.TEXT_PLAIN_VALUE) ;

        Assertions.assertThat(res.getBody()).isEqualTo("HelloSpring");
    } // helloApi

    @Test
    void failsHelloApi(){
        TestRestTemplate rest = new TestRestTemplate();

        ResponseEntity<String> res
                = rest.getForEntity("http://localhost:8081/hello?name=",String.class) ;

        Assertions.assertThat(res.getStatusCode()).isEqualTo(HttpStatus.INTERNAL_SERVER_ERROR) ; // 500

    } // failsHelloApi

}

```



2. 단위테스트(유닛 테스트) : 자바 코드 검증 

* 수행속도 up (자바 실행)
* 서버를 동작하지 않고 (배포 x) 컨테이너 없이 (환경에 의존하지 않고) 간결하게 하나의 대상 (주로 하나의 클래스 단위)를 테스트하도록 만드는 코드 테스트 
* 예외 상황 테스트 코드

```java
package tobyString.helloboot;

import org.assertj.core.api.Assertions;
import org.junit.jupiter.api.Test;

public class HelloControllerTest {

    @Test
    void helloController(){
        // 생성자에 객체 어떻게 주입?
        HelloController helloController = new HelloController(name -> name);

        String ret = helloController.hello("Test") ;
        Assertions.assertThat(ret).isEqualTo("Test") ;
    }

    // 들어오는 요청 (API 동작) 이 제대로 된다는 보장이 없음 ex) Parameter 없어서 exeption
    // => 각 실패에 대한 Test 도 마련해놓아야한다!
    @Test
    void failsHelloController(){ // Exeption이 발생하면 test 성공
        HelloController helloController = new HelloController(name -> name);
        // 이것도 DI ! (스프링 대신 테스트 코드가 assembler 역할!)

        Assertions.assertThatThrownBy(()->{
           helloController.hello(null);
        }).isInstanceOf(IllegalArgumentException.class) ;

        Assertions.assertThatThrownBy(()->{
            helloController.hello("");
        }).isInstanceOf(IllegalArgumentException.class) ;

    } // assertThatThrownBy
}
```





#### [Decorator & Proxy 패턴]

![image-20230811103136267](D:\Programming\github.io\images\2023-07-11-SpringBoot\image-20230811103136267.png)

![image-20230811103107995](D:\Programming\github.io\images\2023-07-11-SpringBoot\image-20230811103107995.png)

=> HelloController를 건들이지 않고 HelloService에 적용되는 클래스를 바꿔주는게 가능 

* 스프링 컨테이너가 뜰때 스프링이 assembler 역할을 하면서 클래스 레벨 의존관계 맞춰줌

  : Assembler한테 참조하는 클래스가 어떤건지 정보만 주면 됨! 



![image-20230811103535263](D:\Programming\github.io\images\2023-07-11-SpringBoot\image-20230811103535263.png)

* SimpleHelloService의 핵심 기능을 수정하지 않고(기존 object를 수정하지 않고) 추가하고자 하는 기능을 클래스로 동적으로 삽입 





* HelloService를 구현하는 Service 클래스를 두개 만들어줘도 오류나지 않음. 어째서?

```java
public class HelloDecorator implements HelloService{
    
}
```

=> 빈 객체로 등록하지 않았기 때문! 

HelloDecorator 가 빈 객체로 등록되지 않았으니 자연히 HelloService를 구현한 애는 SimpleHelloService 밖에 없음 => 단일 객체(single bean)니 자동으로 주입됨 (@Autowired)

​			↓

```java
@Service
public class HelloDecorator implements HelloService{
    
}
```

@Service (@Component 계승) 어노테이션을 붙여줘 빈 객체로 등록 ! 

=> error 정상적으로 발생 ! 

```bash
Parameter 0 of constructor in tobyString.helloboot.HelloController required a single bean, but 2 were found:
```





해결방법 

1. 설정 파일 만들어 주입 관계 직접 지정 

2. @Primary : 후보가 두개면 어떤 것이 @Autowired인지 선택될까

   => 얘가 붙은 애가 먼저 주입되고, 그다음 차례에 HelloSevice 호출하는 부분에서 나머지 걔를 주입 

```java
@Service
@Primary
public class HelloDecorator implements HelloService{
  // HelloService를 의존함과 동시에 다른 클래스(HelloService)를 의존
}
```



3. 자바 코드 





4. Proxy 패턴 

: 실체 대신 그 앞에 대신할 수 있는걸 갖다 놓는다 

![image-20230811110900669](D:\Programming\github.io\images\2023-07-11-SpringBoot\image-20230811110900669.png)

=> HelloController는 원래 내가 사용한 로직있는 클래스로 보지만 실제론  HelloProxy가 대리해서 여러가지 부가적인 기능 

( vs Decorator 패턴 : 기존 오브젝트에 동적으로 새로운 기능 객체를 추가해주는 것 ) 

ex) 실제 로직코드가 복잡해 on demand 로 만들어야하는 경우 (lazy loading)

ex) local 이 아니라 API 를 타고 멀리있는 서버의 객체를 호출하는 경우 

=> 개발하는 쪽에선 Proxy를 실행시켜 local에 있는 객체 다루듯이 하고 실행때 원격 객체 호출은 Proxy 가 담당 





# 자동구성기반 어플리케이션





## 자동 구성 (@AutoConfiguarion)

* Spring 에 있는 기능을 더 잘 활용하도록 ! 





#### [Meta Annotaion]

: 애노테이션 위에 또 다시 애노테이션을 달아주는 것 

![image-20230811111350171](D:\Programming\github.io\images\2023-07-11-SpringBoot\image-20230811111350171.png)

=> @Controller, @Service는 @Component의 기능을 그대로 활용할 수 있음 

+부가적인 효과 + 역할 파악 (이건 WEB mvc에서 어떤 역할을 하는구나 ! 를 알수있음)

@Controller => 아 이거 Controller 구나 mapping 정보 찾아봐야지 



※ 상속 개념은 아님 

Retention 과 Target





2. 합성 애노테이션 : 둘 이상의 Meta Annotation을 가지는 Meta 애노테이션 

 @RestController = @Controller (Meta: @Component) + @ResponseBody 

=> 여러개의 annotation 효과를 한개의 annotation으로 대체 가능 





#### [빈 오브젝트의 역할과 구분] 

스프링 컨테이너 <-- 여러가지 빈 => 성격이 다르고 각각 구성정보 작성이 달라질 수 있음 

​	* 독립 실행형 구현 위한 bean : TomcatServletWebServerFactory, DispatcherServlet

![image-20230811113452679](D:\Programming\github.io\images\2023-07-11-SpringBoot\image-20230811113452679.png)

*  애플리케이션 빈 : 개발자의 로직코드들을 담고 있는 객체들 =>  개발자가 사용하기 위해 명시적으로 구성정보를 제공

* 컨테이너 인프라스트럭처 빈 : 스프링 컨테이너 자체와 관련됨 (컨테이너 스스로 동작하기 위해 등록하는 빈들)  ex) 자기자신, 구성정보, Bean 처리자

​			=> 이런 빈들을 생성해달라고 요청하지 않아도 자동으로 등록되어 사용되고 있음 

​		<-> 구성정보를 작성할 필요 없음 

 

![image-20230811113737945](D:\Programming\github.io\images\2023-07-11-SpringBoot\image-20230811113737945.png)

* 애플리케이션 로직 빈 : 비즈니스 로직을 담고 있는 (우리가 개발하는 부분) 기능을 담당 

  => '사용자 구성정보' ! 이용해 등록  

  : ComponentScan 을 이용해 자동으로 자바코드 및 에노테이션으로부터 읽어옴

- 애플리케이션  인프라스트럭처 빈: 로직 동작 위해 만들어져있음(코드가 작성되어있음) -> 사용하겠다고 구성정보를 작성해줘야 사용됨 

  =>  '자동 구성정보' ! 이용해 등록 

​		: AutoConfiguration (자동구성원리) 매커니즘 통해 등록 됨 ! 

​	<-> 구성정보는 작성해줘야함 ! BUT 그걸 우리가 하는게 아닌 Spring Boot가 대신 작성해줌 (vs 컨테이너 : 작성해줄 필요 없음)





### [실습 : TomcatServletWebServerFactory ,  DispatcherServlet 를 자동구성 매커니즘으로 빈 등록 ]

Q. TomcatServletWebServerFactory ,  DispatcherServlet ? 

이전에는 Servlet Container를 직접 등록해 사용했으니 빈으로 등록할 필요 x 

=> Embedded 방식인 Spring boot에선 반드시 빈으로 명시적으로 선언해야함 

<->  애플리케이션  인프라스트럭처 빈 ! 



#### 1. Component Scan 대상에서 제외 : @Import

Q. AutoConfiguration  ? 

![image-20230811114427499](D:\Programming\github.io\images\2023-07-11-SpringBoot\image-20230811114427499.png)

* 애플리케이션 동작시 사용될 수 있는 인프라스트럭처 빈을 클래스로 등록해놓음 

  => Spring Boot가 Application 실행시키면서 필요한 configuration을 생성해서 가져다 씀

  =>  Spring Boot에선 유저구성정보에 포함 시키지 않아야함  직접 빈으로 등록하지 않아도 뭔가 알아서 등록되도록 만들어야함 (Component Scan 대상에서 제외해야함)



![image-20230811120749026](D:\Programming\github.io\images\2023-07-11-SpringBoot\image-20230811120749026.png)

1. Component 스캔대상이 있는 패키지 x  (@ComponentScan 이 붙은 클래스와 다른 패키지에 있어야함)

   

2. @ComponentScan 대상이 아니더라도 어쨌던 구성정보에 포함해야함

    => **@Import**

    @Import({TomcatWebServerConfig.class,DispatcherServletConfig.class}) : 하드코딩

   

3. Annotaion 분리  : @EnableMyAutoConfig

   => @Import만 따로 관리하고 싶음! 

```java
package tobyspring.config;

@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
@Import(MyAutoConfigImportSelector.class) // 다른 패키지의 Config 빈들을 등록하기 위함 (설정 정보로 불러옴)
public @interface EnableMyAutoConfig {
}
```









#### 2. Import 동적으로 : selectImports()

* Import 동적으로 ! 매번 @EnableMyAutoConfiguarion의 Import 구문 코딩을 수정하는게 아닌 자동으로 ! 

  : 모든 스프링부트 어플리케이션에서 저걸 쓰는게 아니면 직접 쓰는게 아닌 설정 파일로 동적으로 작성되어야함 

ImportSelector  (interface)

: return String[] -> import할 Config 데이터(@Configuration 붙은 클래스들)를 String 으로 변환해 반환하면 이 데이터를 컨테이너가 구성정보로 사용

=> 어떤 설정파일을 사용할지 코드에 의해 그때그때 결정됨 

* 한번 확장해서 DeferredImportSelector (구현)

  : 다른 @Configuartion 붙은 클래스의 구성정보 생성작업이 끝난 후 Import Selector 수행하도록 순서를 좀 바꿈 함

=> 그 읽어오는 클래스 목록은 db에서 읽어올수도, 외부 파일에 적어놓을 수도 있겠지?



1. MyAutoConfigImportSelector 로 DeferredImportSelector  inferface 구현 

```java
public class MyAutoConfigImportSelector implements DeferredImportSelector {
    
	@Override
    public String[] selectImports(AnnotationMetadata importingClassMetadata) {
        
        return new String[]{
                // 사용할 configuration 클래스를 Stiring 으로 반환
                // => Spring 이 이걸 읽어 빈 생성 + 사용
                // 소스코드로 직접
                "tobyspring.config.autoConfig.DispatcherServletConfig",
                "tobyspring.config.autoConfig.TomcatWebServerConfig"
        };
    } 
```

이 반환된 String 배열의 클래스 이름으로 빈 객체 생성 



2. @EnableMyAutoConfig 에 @Import(MyAutoConfigImportSelector.class) 

생성된 빈 객체들을 클래스 MyAutoConfigImportSelector 통째로 Import  

<-> 이 클래스에 목록 작성해주어 빈 객체 생성하고, 이 클래스를 Import 하는 방식으로 바꿔 이 클래스만 빈 객체 목록 수정하면 자동으로 실제 application에 import 



#### 3. 파일 읽어서 하게끔 : @MyAutoConfiguration 



   외부 설정파일로 읽기: txt(구성정보 후보들)로 빼서 자바의 파일 읽기 기능



1. 애노테이션 생성 @MyAutoConfiguration 생성

```java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
@Configuration
public @interface MyAutoConfiguration {
}
```



2. 그렇다면, 이 load는 어디 파일에서 읽어오는가 ?  

   :이 애노테이션 이름으로 된 설정파일 생성해 자동구성에 사용할 클래스 목록 집어넣음

   =>  파일생성해서, 풀네임의 애노테이션 이름으로 파일 생성 시 해당 파일안에 있는 클래스 이름을 읽어와 import한다 

=> 형식 : META-INF/spring/full-qualified-annotation-name.imports

```bash
# tobyspring.config.MyAutoConfiguration.imports 
tobyspring.config.autoConfig.DispatcherServletConfig
tobyspring.config.autoConfig.TomcatWebServerConfig
```





3. MyAutoConfigImportSelector - selectImports

```java
public class MyAutoConfigImportSelector implements DeferredImportSelector {

    private final ClassLoader classloader;

    public MyAutoConfigImportSelector(ClassLoader classloader){
        this.classloader=classloader;  // 이렇게 하면 스프링이 class load할때 사용하는 빈 넣어줌
    }
    @Override
    public String[] selectImports(AnnotationMetadata importingClassMetadata) {

        List<String> autoConfigs = new ArrayList<>();

        // 애노테이션 클래스 정보
        /* classLoader : 애플리케이션에서 resource(파일 등)을 읽어올 땐 classLoader 사용
                    => 스프링이 빈을 생성할 떄 사용할 구성정보 파일을 읽어올때 사용하도록 같이 넣어줘야함
                    => 자동구성정보로 쓸 것이기 때문에 규격화된 방식으로 일어와야함
                    => 작성되어있는 클래스파일을 모두 사용하는게 아니라 후보들을 넣어놓고 스마트하게 고름 ~
         */
        ImportCandidates.load(MyAutoConfiguration.class,classloader).forEach(autoConfigs::add);
        // META-INF/spring/full-qualified-annotation-name.imports on the classpath

     //    return StreamSupport.stream(candidates.spliterator(),false).toArray(String[]::new) ;
        // return autoConfigs.toArray(String[]::new);
        // Arrays.copyOf(autoConfigs.toArray(), autoConfigs.size(), String[].class) ;
        return autoConfigs.toArray(new String[0]); // 빈 String array를 넣어줌 => 컬렉션의 사이즈보다 작은 array가 반환되면 새로운 배열 생성


    }
}

```





4. 실제 자동구성 클래스 들에도 @MyAutoConfiguration 로 바꿔줌 

설정파일 이름을 애노테이션을 하나 만들고, 거기에 imports 붙여줘 이름 만듬  

=> 얘 자체가 기능이 있다기보단 의미를 명확히 함 

:  @MyAutoConfiguration 파일에 적힌 목록에 의해 자동 구성되는 클래스 들이다~ 라는 의미를 명확히 함 









#### 4. @Configuration(proxyBeanMethods = false) 

=> Proxy로 만들어지지 않고 생성됨 

```java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
@Configuration(proxyBeanMethods = false) // proxyBeanMethods = false로 바꾼 configuration이 적용된다
public @interface MyAutoConfiguration {
    // 이 애노테이션 이름으로 된 설정파일을 만들어 거기에 config 으로 쓸 클래스 목록을 작성해줄거임
}

```



* proxyBeanmethods 실습 



+) Test Code 

*  학습 테스트  : ConfigurationTest우리가 만든 기능을 테스트 하는게 아니라 남이 만든 코드로 간단한 샘플을 만들어 사용법을 이해하고 공부하기 위한 목적으로 좋음 



## 조건부 자동구성 : @Conditional

이전까지 자동구성 : 컨스트록춰 빈 기술 종류별로 클래스를 만들고, 외부 파일 목록에서 읽어와서 사용할 수 있도록

--> But! Spring Boot에 들어있는 Configuration 종류에는 어떤게 있을까? 

```bash
org.springframework.boot.autoconfigure.admin.SpringApplicationAdminJmxAutoConfiguration
org.springframework.boot.autoconfigure.aop.AopAutoConfiguration
org.springframework.boot.autoconfigure.amqp.RabbitAutoConfiguration
org.springframework.boot.autoconfigure.batch.BatchAutoConfiguration
									:
									:
org.springframework.boot.autoconfigure.websocket.reactive.WebSocketReactiveAutoConfiguration
org.springframework.boot.autoconfigure.websocket.servlet.WebSocketServletAutoConfiguration
org.springframework.boot.autoconfigure.websocket.servlet.WebSocketMessagingAutoConfiguration
org.springframework.boot.autoconfigure.webservices.WebServicesAutoConfiguration
org.springframework.boot.autoconfigure.webservices.client.WebServiceTemplateAutoConfiguration

=> 144개 
```

=> SpringBoot (Opinionated) : 선택할 수 있는거 제외하고, 스프링 부트를 사용하려면 자동적으로 적용하려는 애들을 자동구성정보 목록으로 작성 



지금까지 방식이면 Configuration 클래스들 + 그 안에 빈들 까지 몇백개의 빈을 다 생성하는 방식 (실제론 몇가지 안되는데... 말도 안됨)

=> 각각의 Configuration 클래스 + 빈 팩토리 메서드들 => 여기에 각각 조건을 걸어 이걸 적용할까? 말까?를 결정해 포함여부 결정 

: 즉 , 일반 파일에 적힌 클래스 목록들은 실제 다 생성되는 빈 목록이 아닌 후보들, 그렇다면 어떤 조건으로 이중 사용할 애들을 결정? 

=> 이게 어떻게 일어남? ? 



### [실습 : 어떤 조건을 걸어 Webserver로 Tomcat을 쓸지, Jetty를 쓸지 선택해보자] 



#### 1. Jetty 자동구성 목록에 추가  

* Tomcat Library : spring initializer가 포함시킴 

  

```bash
# build.gradle 

dependencies {
	implementation 'org.springframework.boot:spring-boot-starter-web'
	testImplementation 'org.springframework.boot:spring-boot-starter-test'
}
```

spring-boot-starter-web 이 라이브러리 하나 작동시키는데 어떻게 모든게 동작할까? 

=> 이 라이브러리는 굉장히 많은 라이브러리 모듈을 포함하기 때문 (그래들의 트리구조 라이브러리 참고)

=> 스프링이 스프링 버전에 맞춰 라이브러리 버전도 다 맞춰서 넣어줌 

 (jsp에선 일일히 다운받아 추가시켜줌........+ 맨땅에서 하나하나 조사해서 호환되는 라이브러리들의 버전까지 맞춰줌 ) 



ㄴ spring-boot-starter-tomcat  <=> tomcat은 스프링 프로젝트 만들면 자동으로 추가해줌 

=> But ! Jetty는 따로 추가해줘야함 ! 

**1. 의존 추가 **

```bash
dependencies {
	implementation 'org.springframework.boot:spring-boot-starter-web'
	implementation 'org.springframework.boot:spring-boot-starter-jetty'
	testImplementation 'org.springframework.boot:spring-boot-starter-test'
}

```



**2. Jetty 와 관련된 ServierConfig 추가 : JettyWebServerConfig**

=> 어떤 조건이 만족되면 Jetty를 , 어떤 조건에선 Tomcat을 자동으로 선택하도록 만드는게 목표 ! 

```java
@MyAutoConfiguration
public class JettyWebServerConfig {
    @Bean("jettyWebServerFactory")
    public ServletWebServerFactory serverFactory (){ // 얜 수정 안해도 됨 (interface형 참조변수의 이유)
        return new JettyServletWebServerFactory(8081);
    }
}
```

```bash
tobyspring.config.autoConfig.DispatcherServletConfig
tobyspring.config.autoConfig.TomcatWebServerConfig
tobyspring.config.autoConfig.JettyWebServerConfig
```



**3. 각 Bean 이름 지정** 

* 원래 spring framework에선 xml에서 생성된 bean이름 지정해줬어야함 

  => boot에선 자동으로 factory 메서드 이름 따라감 

  => 수동 지정 가능   : @Bean("jettyWebServerFactory") 

  (tomcat도 같이 생성되면 같은 factory메서드다보니 이름 충돌되므로 )



=> 이 상태로 서버 띄우면 에러 : 서블릿 컨테이너가 2개 등록되었다 

```bash
Unable to start ServletWebServerApplicationContext due to multiple ServletWebServerFactory beans : tomcatWebServerFactory,jettyWebServerFactory
```



ㄴ 어떻게 둘 중 하나를 선택해서 이 애플리케이션의 서블릿 컨테이너로 사용되도록 할 수 있을까? 

: @Conditional 



#### 2. @Conditional 적용 

```java

@MyAutoConfiguration
@Conditional(JettyWebServerConfig.JettyCondition.class) 
public class JettyWebServerConfig {
    @Bean("jettyWebServerFactory")
    public ServletWebServerFactory serverFactory (){ // 얜 수정 안해도 됨 (interface형 참조변수의 이유)
        return new JettyServletWebServerFactory(8081);
    }

    static class JettyCondition implements Condition {
        @Override
        public boolean matches(ConditionContext context, AnnotatedTypeMetadata metadata) {
            // ConditionContext : 현재 스프링 컨테이너 환경에 대한 정보를 얻을 수 잇는 클래스
            // + AnnotatedTypeMetadata : Meta 애노테이션으로 사용하고 있는 애노테이션들에 대한 metadata 반환
            // return false; // bean으로 등록할건지, 무시할건지 boolean 으로 반환
            return true;
        } // matches
    } // JettyCondition
} // JettyWebServerConfig


```







* TroubleShooting : Jetty 쪽을 true로 지정해줬음에도 ServerFactory로 찾지 못하는 문제 
* 해결 

1. 톰캣쪽을 true로 바꿔 실행 => 정상적으로 동작 : 코드는 틀리지 않았으나 Jetty 빈 생성에 문제가 있음을 발견 
2. 의존 목록 확인 => gradle 목록에 추가했음에도 의존목록에 없음 (오타 확인했는데 틀리지 않음)
3. 원인 : Intellij 오류 - 의존 목록 반영 x 
4. 구글링 keyword : gradle jetty 의존 추가 안됨
5. 해결 : file - invalidates caches - 프로젝트 rebuild (jdk 재가동) => 의존 잘 반영 ~ 

참고 : [gradle 에 추가한 의존성을 IntelliJ 에서 못 찾을 때 (lesstif.com)](https://www.lesstif.com/spring/gradle-intellij-113345573.html)

=> Load 다시해야함 ?? 



* class level 말고 method 레벨 (@bean) 에도 @conditional 할 수 있음 

=> 빈 메서드에 붙은 conditional 조건이 true인 경우에만 해당 빈 등록 / 아님 무시 (클래스는 전체 빈 메서드의 생성 여부를 판단)

class level true -> method level true 여야 빈으로 등록됨 





* testcode 작성 
  * troubleShooting : static keyword를 잘 붙이자...
  * MetaAnnotation : trueConditional 생성 





#### 3. @Conditional 의 Value : ClassUtils.isPresent

* 실제로 자동구성정보 true, fale 를 저런식으로 일일히 지정하지 않음 

  => Spring boot : 해당 라이브러리의 존재 유무를 보고, 그 라이브러리가 있으면 조건구성의 true 값을 주고 그 서블릿을 띄우고, 없으면 안띄움 (즉 , 조건 형식 : 라이브러리 존재 유무)

  => 라이브러리 존재 유무 : 특정 클래스 존재유뮤 검사 (Tomcat -> Tomcat.class , Jetty -> Server.class )  

  => 이 클래스 검사 어떻게 ? Spring 이 제공하는 Utility class 활용 : ClassUtils

  ```java
     static class TomcatCondition implements Condition {
  
          @Override
          public boolean matches(ConditionContext context, AnnotatedTypeMetadata metadata) {
             //  return false;
              return ClassUtils.isPresent("org.apache.catalina.startup.Tomcat", context.getClassLoader()) ;
          } // matches
      } // TomcatCondition
  ```

  



* Conditional에 직접 Condition 클래스를 추가하는 것이 아닌 커스텀 메타 애노테이션을 추가해서 그 안에 조건에 해당하는 값을 넣어주고 그걸 Condition에서 읽어와 사용 (Test Code에서 사용한거랑 같음)

*//* *클래스 이름을 읽어와서 해당 클래스가 존재하는지 유무를* *boolean* *값으로 반환하면 그* *boolean* *값으로 해당 클래스 빈 객체 생성 여부 결정 



#### 4. 자동 구성정보 대체하기 (커스톰 @Bean) : @ConditionaOnMissingBean

* 포함되는 자동 구성 정보 (Object)를 무시하고 내가 만든 custom  configuration을 사용하게 해줘 (스프링 부트 개발시 이런경우 반드시 발생)

=> 사용자 구성정보에다가 자동 구성정보와 등록되는것과 같은 인프라 빈을 직접 작성하면, 그 전의 후보였던 인프라 빈들 대신 내거가 사용되도록 만들어줄 수 있음 (overide와 비슷)



: @Conditional & Condition.class 이용 



​                                                                                                                                                                                                                                                                      **1. ComponentScan 대상 (유저빈)으로 톰캣 추가**

```java
package tobyspring.helloboot;

import org.springframework.boot.web.embedded.tomcat.TomcatServletWebServerFactory;
import org.springframework.boot.web.servlet.server.ServletWebServerFactory;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import tobyspring.config.autoConfig.TomcatWebServerConfig;

@Configuration(proxyBeanMethods = false) // 얜 Component Scan에 의해 추가 
public class WebServerConfiguration {

    @Bean
    ServletWebServerFactory custormerWebServerFactory(){
        TomcatServletWebServerFactory serverFactory = new TomcatServletWebServerFactory() ;
        serverFactory.setPort(9090);
        return serverFactory ;
    }
}

```



=> 에러 : 자동구성에서 톰캣있는데 여기서 또 못띄웡,,! 

=> 따로 다시 지정해야함 





**2. method level Conditional**



=> 클래스 레벨은 그대로 둠 <->  *일단*  *TomcatWebServerConfig* 얜 생성이됨 

=> TomcatServlet에 @ConditionaOnMissingBean 을 넣어서 만약 같은 유저정보 빈이 있으면 유저정보빈으로 사용 

ㄴ 이걸 위해서 DeferredImportSelector 를 implement 

 ```java
 @MyAutoConfiguration
 @Conditional(JettyWebServerConfig.JettyCondition.class)
 public class JettyWebServerConfig {
     @Bean("jettyWebServerFactory")
     @ConditionalOnMissingBean
     public ServletWebServerFactory servletWebServerFactory(){ // 얜 수정 안해도 됨 (interface형 참조변수의 이유)
         return new JettyServletWebServerFactory(8081);
     }
 }
 ```



* 이렇게 잘못 유저정보 빈 잘못 집어넣으면 자동구성정보 대체하면서 앱 실행 문제 발생 ! 

  => 그럼 어케 ? 이걸 쓰면, Spring Boot 의 내부적인 인프라 빈 어떤걸 대체하고 기능이 어떻게 달라지는지 잘 확인해야함 



![image-20230902005630344](D:\Programming\github.io\images\2023-07-11-SpringBoot\image-20230902005630344.png)







#### * Conditional 확장 애노테이션

1. Spring Boot 가 제공하는 자동구성정보를 알고있어야하는 경우 有 : 사용 or 대체 
2. 제 3의 기술을 사용하기 위해 자동구성정보로 등록해서 사용하려면 알아야함 ! 



**@Profile** 

**Class Conditions** - @ConditionakOnClass ** , @ConditionalOnMissingClass  For 조건부 자동 구성  

 => 주의 : 메서드에도 사용되지만 그럼 불필요하게 클래스 빈이 생성되므로 클래스 먼저 고려해야함 

**Bean Conditions** - @ConditionalOnBean , @ConditionalOnMissigBean ** For 자동구성정보 대체 

=> 주의 : 처리 순서 중요 (유저 구성정보먼저 진행되므로 ㄱㅊ But 자동구성 여러개가 여러개고 , factory Method가 여러개인경우 이걸 걸때 순서를 고려해야함 -> 우선 순위 정해서 작성해야 뒤에 검사 안했는데...! 문제 방지 )

**Property Conditions** - @ConditionalOnProperty => 환경 프로퍼지 정보 중 지정된 프로퍼티 값이 있는지 

ex) 라이브러리를 다 로드해도 됨 근데 난 property 파일에 서블릿 톰캣 쓸지, 재키 쓸지 결정할래 => 이런 경우 이거 사용 



**Resource Conditions** - @ConditionalOnResource -> 지정된 리소스(파일)의 존재를 확인 



**Web Application Conditions**  - @ConditionalOnWebApplication, @ConditionalOnNotWebApplication 

=> 웹 애플리케이션 여부를 확인. 모든 스프링 부트 프로젝트가 웹 기술을 사용해야하는 것은 아님 



**SpEL Expression Conditions** 

@ConditionalOnExpression 은 동적으로 설정정보를 문자열 형태로 만들어내는 스프링 표현식(언어)의 처리결과를 기준으로 판단(매우 상세한 조건 설정 가능)





#### 5. 분리 : @ConditionalMyOnClass & MyOnClassCondition.class



TomcalServletConfig와 JettyWebServerConfig의 matches는 완전 중복코딩 

=> 하나의 @Conditional 어노테이션과 Condition.java로 뺀 어노테이션을 만들어 집어넣어줌 

```java
@MyAutoConfiguration
// @Conditional(JettyWebServerConfig.JettyCondition.class)
@ConditionalMyOnClass("org.eclipse.jetty.server.Server")
public class JettyWebServerConfig {
    @Bean("jettyWebServerFactory")
    @ConditionalOnMissingBean
    public ServletWebServerFactory servletWebServerFactory(){ // 얜 수정 안해도 됨 (interface형 참조변수의 이유)
        return new JettyServletWebServerFactory(8081);
    }
}
```





```java
@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.TYPE,ElementType.ANNOTATION_TYPE})
@Conditional(MyOnClassCondition.class)
public @interface ConditionalMyOnClass {
    String value();
}
```



```java
// 클래스 이름을 읽어와서 해당 클래스가 존재하는지 유무를 boolean 값으로 반환하면 그 boolean 값으로 해당 클래스 빈 객체 생성 여부 결정
public class MyOnClassCondition implements Condition {
    @Override
    public boolean matches(ConditionContext context, AnnotatedTypeMetadata metadata) {
        Map<String,Object> attrs =  metadata.getAnnotationAttributes(ConditionalMyOnClass.class.getName()) ;  // 이 애노테이션에 붙은 모든 attribute를 map 형태로 반환
        String value = (String)attrs.get("value") ;

        return ClassUtils.isPresent(value,context.getClassLoader()) ;

    }
}
```





![image-20230902005914446](D:\Programming\github.io\images\2023-07-11-SpringBoot\image-20230902005914446.png)



* Starter, Dependency 로 등록해두면, 해당 어플리케이션의? Classpath로 등록이됨 ! 

---

![image-20230902160847188](D:\Programming\github.io\images\2023-07-11-SpringBoot\image-20230902160847188.png)





## Properties : 외부 설정을 이용한 자동구성 

빈 오브젝트를 아예 다른걸 등록하는게 아니라 자동등록 빈의 내용을 세부적으로 수정 

ex) jdbc 비밀번호는 어디 ? 

=> 외부 설정은 자동 구성 중 어떤 타이밍에 왜 필요한가? 

: 물론 이전엔 유저구성정보 작성으로 했지만... 그 기술이 굉장히 복잡할 경우 아예 그 빈을 새로 구현하는건 너무 힘드니, 

만들어져 적용되는 자동 구성정보의 정보를 약간 바꾸는 방식이 더 효율적임 ! 



=> 앞선 과정에 한 가지 더 있음 

![image-20230902160915499](D:\Programming\github.io\images\2023-07-11-SpringBoot\image-20230902160915499.png)

: 외부 설정정보를 이용해 생성된 빈 오브젝트의 property 값을 '수정'하는 것 (자동구성정보는 디폴트로 되어있는데, 이걸 필요한 경우 바꿔줘야할 때 있음  ) 

ex) 톰캣 포트번호 변경 

ㄴ 자동구성정보의 다양한 프로퍼티를 바꿀수 잇는 방법 => Environment Properties ! 



#### [Environment Abstraction? ]

코드를 매번 직접 수정하지 않고도  어플리케이션의 구성을 수정할 수 있도록 해줌 ==> 외부 설정을 통한 Property! 

- @Profile 모델 : 특정 조건을 만족한 profile일때만  어떤 빈들을 사용할 것인

- Property값 읽어오기 

  외부에 Property 값을 설정해두고, 애플리케이션이 실행될 때 해당 값을 읽어오도록 동작 ex) DB 을 연결할 때 username 

  => 자바, 서블릿에선 다양한 방법으로 해당 Property 값에 접근, 

  But! Spring 은 Environment라는 이름으로 단일화된 방식으로 access할 수 있도록 추상화 ("서비스 추상화")	





**추상화 종류에 따른 5가지 프로퍼티**  

* StandardEnvironment  => 주로 o ! 미리 지정해줄 일 있으면 여기서 지정 

  * System Properties  : 자바에서 기본적으로 다루는 프퍼티
  * System Environment  : OS 자체에 환경 변수를 세팅하고 읽어오도록

* StandardServletEnvironment (서블릿 사용시 활용가능한 프로퍼티 설정) => 거의 x : boot에선 서블릿 직접 안다뤄주니까 ! 

  * ServletConfig Parameter : xml 같이 서블릿을 초기화하는 코드에 서블릿 파라미터 지정 
  * ServletContext Parameter : " " => But Servlet Context level 에 지정 
  * (JNDI) 

  +) Spring Boot은 추가적으로 이러한 Environment의 프로퍼티를 읽어온다  

  1. @PropertSource : 애노테이션을 붙여 커스텀으로 프로퍼티를 추가하는 방식도 제공함 ! 
  2. application.properties, xml, yml : 이런 타입의 프로퍼티 정보를 이 Environment 추상화를 통해 읽어옴 

  

=> Environment 타입으로 가져오면 , getProperty로 값 가져옴 (우선순위가 있음) 

: Environment.getProperty("property.name")     (--> property.name, property_name, PROPERTY.NAME, PROPERTY_NAME :  이 이름들의 프로퍼티 있는 지 확인 



**EX) ApplicationRunner  Bean** 

어플리케이션을 실행할 때 특정 기능이 실행되도록 바꾸겠음 ! 

: main에 코드를 추가해도 되지만 그것보단 스프링 부트가 제공하는 ApplicationRunner Interface 사용 ! 

=> ApplicationRunner  :  컨테이너 초기화 작업이 끝난 후 실행될때 빈으로 실행되는 클래스  

```java
	@Bean
	ApplicationRunner applicationRunner(Environment env){
		// 스프링 안에 있는 환경정보를 추상해놓은 Object인 Environment를 주입받음(실행될때 자동으로 )
		return args ->{
			String name = env.getProperty("my.name");
			System.out.println("my.name:" + name); 
            // my.name:null --> my.name:ApplicationProperties --> my.name:EnvironmentVariable (System Environment ) --> my.name:SystemProperty
		} ;
}
```

ㄴ Environment는 스프링이 자체적으로 빈으로 등록해 사용하는 클래스 => 이걸 자동 DI로 주입받아서 사용 가능  



* 환경변수 설정시 이 Properties 파일보다 우선해서 적용 

  * System Environment 

  ![image-20230902131914452](D:\Programming\github.io\images\2023-07-11-SpringBoot\image-20230902131914452.png)

  *환경변수 이름은 보통 대문자 + 언더바 

  

  * System Properties 

    ![image-20230902132216826](D:\Programming\github.io\images\2023-07-11-SpringBoot\image-20230902132216826.png)

  * properties 파일 변경 

  ```properties
  #configuring port
  my.name = ApplicationProperties
  
  server.port = 8081   
  # 헐 난 이미 해봄!! 포트번호 바꿀때 이 파일에 작성해 초기 실행할때 embedded Tomcat이 포트번호 8081하도록 설정함 헐 !! 
  ```

  





### [실습: 자동구성 톰캣의 포트번호, contextPath를 수정해보자]

* property는 (key,value) 의 형태로 저장된다 --> 그래서 Map<> 으로 받는 경우가 많았음 



#### 1. Enviroment.getProperty(저장된 properties 파일 설정값) 

1. helloboot 패키지(ComponentScan)의 커스텀 빈 제거 (톰캣 유저정보 제거 ) 
2. application.properties 파일에 contextPath 속성 지정 

```properties
#configuring port
server.port = 8081
my.name = ApplicationProperties
contextPath=/app
```



3. Envioronment를 활용해 해당 속성 읽어와 Tomcat contextPath로 지정해주기 ! 

```java
@MyAutoConfiguration
@ConditionalMyOnClass("org.apache.catalina.startup.Tomcat")
public class TomcatWebServerConfig {

    @Bean("tomcatWebServerFactory") 
    @ConditionalOnMissingBean
    public ServletWebServerFactory servletWebServerFactory(Environment env){
        TomcatServletWebServerFactory factory =  new TomcatServletWebServerFactory(8081);
        // factory.setContextPath("/app");
        // 이걸 정해주면 모든 서블릿의 Mapping앞에 contextPath 추가 => 그냥 /hello 면 에러 ! /app/hello 로 요청 보내줘야함
        // 이걸 코드로 박아넣지 않고 Enviomnet를 통해 Property 값 지정
        factory.setContextPath(env.getProperty("contextPath"));
        return factory ;
    }
}
```



4. 개선 : 필드에 주입 **=> placeholder 사용** 

Environment 에서 직접 값을 읽어올 수 있지만, Spring 에선 Property에서 읽어온 값을 필드에 주입해주는 방법도 있음

=> 매번 읽어오기보다 필드로 선언해두면 재사용 용이 (getProperty 코드 생략) 

=> placeholder:  **@Value("${contextPath}")** 

```java
package tobyspring.config.autoConfig;


@MyAutoConfiguration
@ConditionalMyOnClass("org.apache.catalina.startup.Tomcat")
public class TomcatWebServerConfig {

    @Value("${contextPath:}") 
    String contextPath;

    @Value("${port:8081}")
    int port; // 만약 이 값을 지정하지 않으면(properties에 안쓰면) 띄우면 에러 => default값을 지정해줘야함 ':8081'

    @Bean("tomcatWebServerFactory") // factory 메서드 실행 -> 얘네도 생성
    @ConditionalOnMissingBean
    public ServletWebServerFactory servletWebServerFactory( /*Environment env */ ServerProperties properties){
        TomcatServletWebServerFactory factory =  new TomcatServletWebServerFactory(8081);

        // factory.setContextPath(env.getProperty("contextPath"));
        factory.setContextPath(this.contextPath);

        return factory ;
    }
}
```



*=>* *에러* *!* *문자열 그대로 추가됨* *:* *이 치환 기능은 스프링의 기본기능이 아니기 때문에 후처리 기능으로 추가해줘야함* *=>

=> **PropertyPlaceholderConfig.java** 추가 ! 

​	ㄴ Bean : PropertySourcesPlaceholderConfigurer 를 등록시켜주는 Config.java 

​	: Environment로 추상화된 각종 PropertySource로 부터 ${} (placeholder) 에서 적어준 값을 지정해준 도구 

=> 자동구성으로 지정 : 클래스 목록에 추가 

```properties
tobyspring.config.autoConfig.PropertyPlaceholderConfig 
```

=> 치환자 적용 ㄱㄴ ! 





#### 2. ServerProperties: 프로퍼티를 담고있는 정보를 독립적인 클래스

* 근데 프로퍼티가 굉장이 많다면,,,,? 필드 몇백개 쓸거야? 조금 더 구조적으로 다룰 순 없을까? 

=> 프로퍼티를 담고있는 정보를 독립적인 클래스로 추출, 분리  : **ServerProperties.java** --> autoConfig 

--> 해당 클래스로 객체를 생성해 여러값을 세팅한 후 Bean으로 등록하기위한 클래스 : **ServerPropertiesConfig.java** 

--> 톰캣Config는 그 세팅된 객체를 주입받음: 톰캣 자체의 필드로 정의하는게 아닌 Property  객체에 저장된 값을 읽어옴 

​		 Tomcat(ServerProperties) => 그 클래스 주입받음 



1. ServerProperties.java 로 프로퍼티 목록 정의 

```java
@MyConfigurationProperties(prefix="server") // 이 아래 property들에 대한 namespace 역할 (파키지 같은 역할)
public class ServerProperties {

    String contextPath;
    int port;

    public String getContextPath() {
        return contextPath;
    }
    public int getPort() {
        return port;
    }
    public void setContextPath(String contextPath) {
        this.contextPath = contextPath;
    }
    public void setPort(int port) {
        this.port = port;
    }
}
```



2. 해당 클래스 값 세팅해주는 클래스 ServerPropertiesConfig 생성 

*  원리 : 스프링부트는 어떤 프로퍼티값읽어 사용할땐 프로퍼티값들을 정의해둔 클래스가 있고, 이걸 자동구성 빈에서 주입받아 사용하니, 이 프로퍼티값들을 정의해둔 클래스를 빈으로 등록해주는 작업이 필요

```java
@MyAutoConfiguration
public class ServerPropertiesConfig {
    @Bean
    public ServerProperties serverProperties(Environment env){
        ServerProperties properties = new ServerProperties();

        properties.setContextPath(env.getProperty("contextPath"));
        properties.setPort(Integer.parseInt(env.getProperty("port")));

        return properties;
    }
}
```



=>근데, 이렇게 Environment에서 일일히 꺼내오는 과정 불편! 개선해보자 ~ 

: **Binder.java**

```java
@MyAutoConfiguration
public class ServerPropertiesConfig {
    @Bean
    public ServerProperties serverProperties(Environment env){

        return  Binder.get(env)   // Environment로부터 Property값들을 가져와서 
            .bind("", ServerProperties.class).get(); // ServerProperties 필드들과 Binding해서 값 넣어줌  
        // * Binding 이렇게 하면 Properties 클래스에 있는 필드 이름과 일치하는 property 값들을 자동으로 넣어줌
        // 
    }
}
```

=> 이러면 프로퍼티 파일과 Properties 클래스만 맞춰 수정해주면, 이 Config 파일은 수정해줄 필요 없음 

(Tip: 어떤 propery 값을 수정, 다룰땐 ServerPropertis.java 확인해서 목록 확인할 수있음 )

(+prefix)



4.  톰캣Config은 그 Property 값으로 세팅된 객체 주입받음

```java
@MyAutoConfiguration
@ConditionalMyOnClass("org.apache.catalina.startup.Tomcat")
@Import(ServerProperties.class)
public class TomcatWebServerConfig {
/*
    @Value("${contextPath:}") 
    String contextPath;

    @Value("${port:8081}")
    int port; 
*/
    @Bean("tomcatWebServerFactory") 
    @ConditionalOnMissingBean
    public ServletWebServerFactory servletWebServerFactory( /*Environment env */ ServerProperties properties){
        TomcatServletWebServerFactory factory =  new TomcatServletWebServerFactory(8081);

        factory.setContextPath(/*this.contextPath*/ properties.getContextPath());
        factory.setPort(/*port*/ properties.getPort());

        return factory ;
    }
}
```

=> 톰캣 자체의 필드로 정의하는게 아닌 Property  객체에 저장된 값을 읽어옴 





#### 3.  ServerPropertiesConfig 없이 빈 객체 생성  



위의 방식  : Config가 객체를 생성하고, 값 설정해서, 빈으로 등록  (객체생성 --> 값 세팅 --> 빈 등록 --> 빈 주입)

=> **문제**

1. 사용하는 기술이 늘어날때마다 이렇게 Properties 클래스가 늘어날거고, 그에 맞춰 Config도 계속 생성해주면서 늘어날거임 

2. 해당 자동구성 사용할경우에만 활성화해야하기 때문에 또 각 Config 파일에도 Conditional 만들어서 빈 생성할지 조건줘야함 (톰캣이야? 제티야? )

=> ServerProperties 를 세팅+ 빈 등록하는 방식 바꿔야함 ! 

: 클래스 자체에 표식을 줘 컨테이너가 빈으로 등록하고, 그 값을 설정해야함 (객체 생성 -- 빈 등록 --> 빈 주입 --> 값 세팅) 



1. ServerPropertiesConfig 제거 (파일에서도 제거해야함)
2. @Component : ServerProperties 클래스 자체를 빈 후보로 등록 !  

(VS 위에 : ServerProperties 객체를 생성해서, 그 값을 세팅한 객체 자체를 빈으로 등록 )

```java
@Component
public class ServerProperties {
    String contextPath;
    int port;

    public String getContextPath() {
        return contextPath;
    }

    public int getPort() {
        return port;
    }

    public void setContextPath(String contextPath) {
        this.contextPath = contextPath;
    }

    public void setPort(int port) {
        this.port = port;
    }
}

```





3. @Import(ServerProperties .class)

=> 그럼 ServerProperties 어떻게 빈으로 등록해? @Import하면 빈으로 등록됨 

(@Import : @ComponentScan 대체 가능! )





#### 4. BeanPostProcessor  : 생성된 빈 값 설정



그래도 에러 ! contextPath, port값 지정 안해줬으니까 

<-> 빈으로 생성 한후, Properties 파일에서 읽어온 값으로 값 설정해줘야함 (객체 생성 -- 빈 등록 --> 빈 주입 --> 값 세팅) 

(이전  : Config가 객체를 생성하고, 값 설정해서, 빈으로 등록)

 => 이거 어떻게지정?  **빈의 후처리기**

 : 빈을 다 만들고 주입한 다음에 , 그 빈을 가공할 수 있는 기회가 주어지는 것 



1. @ MyConfigurationProperties 제작 

```java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
@Component
public @interface MyConfigurationProperties {
    
}
```

=> 이 애노테이션이 붙은 클래스는 빈으로 등록된 후 값을 설정가능

 

2. 후처리 할 클래스에 만든 애노테이션 붙이기 

```java
@MyConfigurationProperties
public class ServerProperties {

    String contextPath;
    int port;

    public String getContextPath() {
        return contextPath;
    }

    public int getPort() {
        return port;
    }

    public void setContextPath(String contextPath) {
        this.contextPath = contextPath;
    }

    public void setPort(int port) {
        this.port = port;
    }
}

```





3. 후처리기  **PropertyPostProcessorConfig.java**  생성 

: 빈으로 등록한 클래스에 MyConfigurationProperties 애노테이션이 붙어있는 경우엔

, 해당 빈(ServerProperties)의 클래스를 가져와 

, Environment의 Property 값을 바인딩하거나, 해당 프로퍼티 목록으로 클래스를 새로 만들어 값을 세팅해 반환하고 

, 빈으로 다시 등록한다 (ServerProperties)

```java
@MyAutoConfiguration // 자동 구성정보로 빈 생성 
public class PropertyPostProcessorConfig { // 후처리기 생성하는 Config 클래스 

    @Bean // 후처리기
    BeanPostProcessor propertyPostProcessor(Environment env) // Environment 환경설정 값 주입 받아서 
    	{ return new BeanPostProcessor() { // 익명 클래스
           
        @Override
        public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
                // Bean Object 초기화가 끝난 다음에, 이 빈 오브젝트 프로세서를 실행해줘 !
                // <-> 이 후처리기는 모든 빈이 생성될때마다 각각 실행됨 !
            	// => Object : 생성된 빈 

                MyConfigurationProperties annotation = AnnotationUtils.findAnnotation(bean.getClass(),MyConfigurationProperties.class);
                // 생성된 빈의 클래스에, MyConfigurationProperties가 붙은 애노테이션이 있으면 그 어노테이션을 반환해라
            	// return : 일치하는게 있으면 @MyConfigurationProperties 자체 반환 
            
                if(annotation == null) return bean ; // 어노테이션이 없으면, 원래 하던대로 빈 생성 

                return Binder.get(env).bindOrCreate("", bean.getClass()); 
            // 있으면? 값 세팅해야함 (여기선 아직 어노테이션 MyConfigurationProperties 자체는 안씀 그냥 값 있냐없냐)
            // : Environment에서 Property 값 가져와, bind를 시작했는데 없으면 Create해서 return 
            }
        } ;
    }
}
```

=> ServerProperties 클래스 파일이 추가된다 하더라도 그에 맞춰 빈생성기 클래스를 따로 만들어줄 필요 없고 

해당 프로퍼티 목록으로 클래스를 자동으로 만들어줌 



4. 그렇게 설정값이 세팅된 빈을 Tomcat은 Import 

: 톰캣Config은 그 Property 값으로 세팅된 객체 주입받음

```java
@MyAutoConfiguration
@ConditionalMyOnClass("org.apache.catalina.startup.Tomcat")
@Import(ServerProperties.class)
public class TomcatWebServerConfig {

    @Bean("tomcatWebServerFactory") 
    @ConditionalOnMissingBean
    public ServletWebServerFactory servletWebServerFactory( /*Environment env */ ServerProperties properties){
        TomcatServletWebServerFactory factory =  new TomcatServletWebServerFactory(8081);

        factory.setContextPath(/*this.contextPath*/ properties.getContextPath());
        factory.setPort(/*port*/ properties.getPort());

        return factory ;
    }
}
```





#### 5. prefix 붙이는 작업



Property 는 너무 많다... Property만 쓰면 자동구성정보에 등록된 클래스의 모든 환경 변수가 포함되는격  

ㄴ 이름이 중복될 경우가 많음 ex) 'port'  --> 어디의 ? 서버의? 



*  prefix :  property들에 대한 namespace 역할 *(패키지 같은 역할)* 

: binding(값 세팅) 을 할 때 그 prefix 를 추가해줘야함 ! - binding : server의 port ! 

; Environment에서 읽어온 프로퍼티값으로 port는 port인데 서버의 port 값을 세팅해줌 ! 

=> 실제로 server 폴더가 있는게 아니라 구분을 위해 달아준 prefix 타고가는거임



1. @MyConfigurationProperties에 매개변수로 prefix를 입력받음 

```java
@MyConfigurationProperties(prefix="server")
public class ServerProperties {
    String contextPath;
    int port;

    public String getContextPath() {
        return contextPath;
    }

    public int getPort() {
        return port;
    }

    public void setContextPath(String contextPath) {
        this.contextPath = contextPath;
    }

    public void setPort(int port) {
        this.port = port;
    }
}

```



* MyConfigurationProperties

```java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
@Component
public @interface MyConfigurationProperties {
    String prefix(); // server
}

```



2. 해당 어노테이션이 붙은 빈 등록 
3. 후처리기 발동 

: postProcessor에서 prefix를 뭐로했는지 알아서 binding을 할 때 그 prefix 를 추가해줘야함 ! 

```java
@MyAutoConfiguration
public class PropertyPostProcessorConfig {

    @Bean
    BeanPostProcessor propertyPostProcessor(Environment env){
        return new BeanPostProcessor() { // 익명 클래스
            @Override
           public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException{
				// Object bean : ServerProperties
               
                MyConfigurationProperties annotation = AnnotationUtils.findAnnotation(bean.getClass(),MyConfigurationProperties.class);

                if(annotation == null) return bean ;

               // 반환된 어노테이션 MyConfigurationProperties의 모든 attr(매개변수 값)을 가져와 <이름, 객체>로 저장
               // <prefix, "server"> < ~ , ~ > ... 
                Map<String,Object> attrs= AnnotationUtils.getAnnotationAttributes(annotation);
                String prefix = (String) attrs.get("prefix"); 

                return Binder.get(env).bindOrCreate(prefix, bean.getClass()); 
               //binding(값 세팅) 을 할 때 그 prefix 를 추가해줘야함 ! - binding : server의 port ! 
               // <-> Environment에서 읽어온 프로퍼티값으로 port는 port인데 서버의 port 값을 세팅해줌 ! 
              // (실제로 server 폴더가 있는게 아니라 구분을 위해 달아준 prefix 타고가는거임)

            }
        } ;
    }
}
```





#### 6. 복습 : @Import(하드코딩)은 좋지 않다 



* 자동구성정보 값 수정을 많이할 수록 하나의 클래스만 다루지 않음 

  => 그에 맞게 프로퍼티 목록파일 xxProperties.java 파일도 많아질거임 

  => 위와 같은 방식에선 그럼 

  @Import({ServerProperties.class, MyProperties.class, WaterProperties.class....} ) 로 쫙 나열해야함 

=> 어딘가에서 수정할 프로퍼티 목록 읽어서 Import 동적으로 해줘야함 : **selectImports()**



1. @Import --> @EnableMyConfigurationProperties

: 목적성을 분명히 함 

```java
@MyAutoConfiguration
@ConditionalMyOnClass("org.apache.catalina.startup.Tomcat")
// @Import(ServerProperties.class)
@EnableMyConfigurationProperties(ServerProperties.class , WaterProperties.class)
public class TomcatWebServerConfig {

```



* @Enable- 애노테이션의 대부분의 목적 

: 이 안에 @Import을 다시 넣어 기능을 가진 Configuration 클래스나 Selector을 가져오는 목적 

```java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
@Import(MyConfigurationPropertiesImportSelector.class) // @Enable- 애노테이션의 대부분의 목적: 이 안에 @Import을 다시 넣어 기능을 가진 Configuration 클래스나 Selector을 가져오는 목적
public @interface EnableMyConfigurationProperties {
    Class<?> value(); // <ServerProperties.class , WaterProperties.class>
}
```



2. ImportSelector 사용 

```java
public class MyConfigurationPropertiesImportSelector implements DeferredImportSelector {
    @Override
    public String[] selectImports(AnnotationMetadata importingClassMetadata) {

        MultiValueMap<String, Object> attr = importingClassMetadata.getAllAnnotationAttributes(EnableMyConfigurationProperties.class.getName());
        // EnableMyConfigurationProperties 이 애노테이션에 붙은 모든 애노테이션의 attribute를 가져와 Map 형태로 저장 
//         => 이 Map:프로퍼티 클래스 이름들의 목록  <value, ServerProperties.class,WaterProperties.class >
        Class propertyClass = (Class) attr.getFirst("value"); 
        // 그냥 get은 List 형태로 반환되니 임의로 첫번째것만! - getFirst() -> ServerProperties.class

        return new String[]{propertyClass.getName()}; // ServerProperties.class
        // Import로 직접 ServerProperties 클래스를 가져오는 대신 @EnableMyConfigurationProperties의 element 값으로 프로퍼티값을 대신 읽어오도록 만듬
    }
}
```





* Inmemory(Embeded) DB --> H2 : 애플리케이션이 실행될때만 존재하는 DB 





Test 순서 

1. HelloRepository test 
2. Helloervice TEst 
