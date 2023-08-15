### 섹션 1. 스프링 부트 시작하기

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





### 섹션2. 개발환경 세팅

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
									, new DispatcherServlet(this)).addMapping("/*");  // addServlet
							// 매핑(요청 넘기기), 응답 body 작성 등 직접 코드 안해줘도 알아서 해줌 (매핑 정보만 빈객체에 작성해주면 됨)
						}) ; // getWebServer
				webServer.start();
			} // onRefresh
		};
		applicationContext.registerBean(HelloController.class);
		applicationContext.registerBean(SimpleHelloService.class);
		applicationContext.refresh(); // 템플릿 메서드  => 훅 메서드: onRefresh
```





코드를 Spring container의 Compenent 를 등록하고, 의존하고 있다면 어느 시점에 어떻게 주입해 줄 것인가를 스프링 컨테이너에 '구성정보'로 제공 (이전 : xml) 

* 이전 : 클래스 정보를 registerBean 

1. java 코드로 ! 

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
				// 두번째 작업인 Servlet Container 를 만들고 초기화 하는 과정을 Spring Container 초기화 과정 중 일어나도록 수정 : Spring boot의 방식
				TomcatServletWebServerFactory serverFactory = new TomcatServletWebServerFactory(8081);
				WebServer webServer = serverFactory
						.getWebServer(servletContext -> {
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
									, new DispatcherServlet(this)).addMapping("/*");  // addServlet
							// 매핑(요청 넘기기), 응답 body 작성 등 직접 코드 안해줘도 알아서 해줌 (매핑 정보만 빈객체에 작성해주면 됨)
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





2. 스캐너 : 클래스 위에 직접 붙여줌 

빈객체로 등록하고 싶은 클래스 위에 직접 사용해줌 (위: 호출하는데서 작성)

=> @Component : "나는 Spring Container 에 들어가는 Component(빈객체)야"



1) 빈객체로 등록하고자 하는 클래스 위에 @Component 

2) @ComponentScan : @Component 이 붙은 클래스를 찾아 빈객체로 생성+조립해라 !

    (@Configuration: Container 구성정보  아래에 작성)



=> 간단,편리하지만 Bean으로 어떤 객체들이 등록되는지 찾지 못하는 단점

: 패키지 구성 잘하고 모듈 잘 나눠서 개발하면 ㄱㅊ 



* MetaAnnotation : Annotaion (코드)위에 붙은 Annotaion 

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







* TomcatServletWebServerFactory, DispatcherServlet  => Application이 시작되려면 필요한 두 object

  => Spring Bean으로 등록해 Spring Container 가 관리 

* ApplicationContextAware => setApplicationContext()

  => 'lifecycle method' : Bean을 Container가 등록하고 관리하는 중 Container가 관리하는 Object를 bean 에 주입  

  => 이런 종류의 interface를 구현해놓으면 Spring Container는 이 객체가 등록되는 시점에 setter 메서드로 넣어줘야겠구나! (이 setter 메서드 자체를 Spring Container 가 호출하는 거임! )





* 실행 메서드 분리 : run()

```java
private static void run(Class<?> applicationClass, String... args) {
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



=> 스프링 컨테이너 + 서블릿 컨테이너 자동으로 만들어 스프링 컨테이너로부터 정보ㅡㄹ 넘겨받아 기본적으로 웹을 실행시켜줌 

=> 다른 main이 되는 클래스에서도 재사용 가능 





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







### 섹션 6. 자동 구성 (@AutoConfiguarion)

* Spring 에 있는 기능을 더 잘 활용하도록 ! 

  

1. Meta Annotaion 

![image-20230811111350171](D:\Programming\github.io\images\2023-07-11-SpringBoot\image-20230811111350171.png)

=> @Controller, @Service는 @Component의 기능을 그대로 활용할 수 있음 

+부가적인 효과 + 역할 파악 (이건 WEB mvc에서 어떤 역할을 하는구나 ! 를 알수있음)

@Controller => 아 이거 Controller 구나 mapping 정보 찾아봐야지 



※ 상속 개념은 아님 

Retention 과 Target





2. 합성 애노테이션 : 하나 이상의 Annotation을 활용해 만든 애노테이션 

 @RestController = @Controller (Meta: @Component) + @ResponseBody 

=> 여러개의 annotation 효과를 한개의 annotation으로 대체 가능 





* 빈 오브젝트의 역할과 구분 

스프링 컨테이너 <-- 여러가지 빈 => 성격이 다르고 각각 구성정보 작성이 달라질 수 있음 

​	* 독립 실행형 구현 위한 bean : TomcatServletWebServerFactory, DispatcherServlet

![image-20230811113452679](D:\Programming\github.io\images\2023-07-11-SpringBoot\image-20230811113452679.png)

*  애플리케이션 빈 : 개발자가 사용하기 위해 명시적으로 구성정보를 제공

* 컨테이너 인프라스트럭처 빈 : 스프링 컨테이너 자체와 관련됨 (컨테이너 스스로 동작하기 위해 등록하는 빈들)  ex) 자기자신, 구성정보, Bean 처리자

​			=> 자동으로 등록되어 사용되고 있음 

 

![image-20230811113737945](D:\Programming\github.io\images\2023-07-11-SpringBoot\image-20230811113737945.png)

* 애플리케이션 로직 빈 : 비즈니스 로직을 담고 있는 (우리가 개발하는 부분) 기능을 담당 

  => 사용자 구성정보 ! 이용해 등록  

  : ComponentScan 을 이용해 자동으로 자바코드 및 에노테이션으로부터 읽어옴

- 애플리케이션  인프라스트럭처 빈: 로직 동작 위해 만들어져있음 -> 사용하겠다고 구성정보를 작성해줘야 사용됨 

  =>  자동 구성정보 ! 이용해 등록 

​		: AutoConfiguration (자동구성원리) 매커니즘 통해 등록 됨 ! 



Q. TomcatServletWebServerFactory ,  DispatcherServlet ? 

이전에는 Servlet Container를 직접 등록해 사용했으니 빈으로 등록할 필요 x 

=> Embedded 방식인 Spring boot에선 반드시 빈으로 명시적으로 선언해야함 

<->  애플리케이션  인프라스트럭처 빈 ! 



Q. AutoConfiguration  ? 

![image-20230811114427499](D:\Programming\github.io\images\2023-07-11-SpringBoot\image-20230811114427499.png)

* 애플리케이션 동작시 사용될 수 있는 인프라스트럭처 빈을 클래스로 등록해놓음 

  => Spring 이 Application 실행시키면서 필요하면 생성해서 가져다 씀

  =>  Spring Boot에선 유저구성정보에 포함 시키지 않아야함  직접 빈으로 등록하지 않아도 뭔가 알아서 등록되도록 만들어야함 (Component Scan 대상에서 제외해야함)



![image-20230811120749026](D:\Programming\github.io\images\2023-07-11-SpringBoot\image-20230811120749026.png)

1. Component 스캔대상이 있는 패키지 x  (@ComponentScan 이 붙은 클래스와 다른 패키지에 있어야함)
2. @Import({TomcatWebServerConfig.class,DispatcherServletConfig.class})
3. Annotaion 분리  

-- 하드 코딩  --- 

* Import 동적으로 ! 매번 @EnableMyAutoConfiguarion의 Import 구문 코딩을 수정하는게 아닌 자동으로 ! 

* 직접 쓰는게 아닌 설정 파일로 동적으로

ImportSelector  (interface)

: return String[] -> import할 Config 데이터(@Configuration 붙은 클래스들)를 String 으로 반환하면 이 데이터를 컨테이너가 구성정보로 사용

=> 어떤 설정파일을 사용할지 코드에 의해 그때그때 결정됨 

* DeferredImportSelector (구현)

  : @Configuartion 안의 코드가 완료된 후? Import Selector 수행하도록 함



* 파일 읽어서 하게끔

1. @MyAutoConfiguration 생성
2. MyAutoConfigImportSelector 
