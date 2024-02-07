---
published: true
title:  "에러 Premature end of file  해결 "
categories: Troubleshooting 
tag: [java, Spring] 
toc: true
author_profile: false 
---



문제 상황

: Spring Project 진행 중 WEB-INF 폴더 밖에 있는 jsp 파일을 실행해도 404 에러로 페이지가 실행되지 않음



#### 1. console 확인해보니 다음과 같은 에러 발생 

``` java
Cause by: org.xml.sax.SAXParseException; lineNumber: 1; columnNumber: 39; Premature end of file
```

point )  1- SAXParseException 	2 - Premature end of file



#### 2. 에러 종류 

XML을 파싱하는 방식 중 하나로 XML문서를 순차적으로 읽어 들이면서 오류를 발생시키는 방식, 

xml 파일을 읽어들이는 과정에서 문제가 있으면 발생하는 오류이다. 쉽게 말하면 xml 문법 오류! 

( ??????????서버를 갔다오지 않은 publicWeb 실행시점에서 오류가 나는 이유?????????????) 



*자주 하는 실수 

1. 선언부에 공백이 있거나
2. 잘못된 위치에 주석이 있거나 
3. 부등호를 <> 로 인식했거나 (해결책 : CDATA!!!  -> 데이터 자체를 그냥 문자 그대로 받아들이는것 ) 
4. xml 구성요소의 순서 및 위치가 잘못되었거나

등 정말 다양한 이유로 발생하는 에러 

 

#### 3. 해결단계 

정말 다양한 이유로 발생하는 문제인만큼 단순 구글링, 코드 추가로 해결하기 힘든 문제

→ 원인을 정확히 파악해야함 



* 진행과정

<li> 다행히 프로젝트 세팅 자체가 원본 파일에서 많이 변경되지 않음 </li>

 1. 세팅 base 파일에서 세팅 단계 하나씩 진행하면서 index.jsp 파일 실행 

 2. 순차적으로 되다가 

    src/main/resource 	

    ​	ㄴ org.doit.ik.mapper 에

    Mybatis 활용위한 Mapper.xml 파일들 추가 이후 404 에러 발생 

    

 3. ##### 원인 발견

    

    : 해당 xml 파일에 

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    ```

    만 작성되있어 어떤 문서타입인지? 몰라서 생기는 문제인지 

    매핑이 안되서 생기는 문제인지 정확히 파악은 안됨 (나중에 수정)

    

    4. ##### 해결

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "https://mybatis.org/dtd/mybatis-3-mapper.dtd">
    
    <mapper namespace="org.doit.ik.mapper.MemberMapper">
    
    </mapper>
    ```

    

    로 수정해 각 매퍼파일들 매핑 시켜주니 정상적으로 실행됨

    

    * test 
    *  MemberMapper 를 ProjectMapper 로 바꾸니 또다시 404 에러
    * Mapper 파일 중 하나 지워도 정상적으로 실행

    

    #### 4. Learned...

    이렇게 정말 사소한, 다양한 원인으로 발생할 수 있는 문제는 구글링으로 명확한 원인을 파악하기 어려움

    → 코딩이 얼마되지 않은 시점에선 정상적으로 실행되는 시점부터 하나씩 진행해나가면서 실행 

    → 안되는 부분 캐치해 문제 해결 

    → 애초에 코딩할 때 부터 계속 테스트 -> 코딩 -> 테스트 -> 코딩을 반복 (중간중간 계속 테스트)

    →  만약 코딩이 많이 진행된 상태라면 ?   ??? ?

    

    

    

    

    

    

     





