---
published: true
title: "Java-Scanner와 BufferedReader" 
categories: Java
tag: [Java, Scanner, BufferedReader] 
toc: true
author_profile: false 
  
---



##### 버퍼를 이용한 입출력

> BufferedReader와 BufferedWriter는 버퍼를 사용하여 읽기와 쓰기를 하는 함수이다. 

버퍼를 사용하지 않는 입력은, 키보드의 입력이 키를 누르는 즉시 바로 프로그램에 전달된다. 반면 버퍼를 사용하는 입력은 키보드의 입력이 있을 때마다 버퍼로 전송하다가, 버퍼가 가득차거나 개행문자를 입력하면 버퍼의 내용을 한번에 프로그램에 전달한다. 

입출력 장치와의 데이터 입출력은 느리며, 이 느린 작업을 입력시마다 프로그램에 주면 비효율적이다. 그렇기에 중간에 버퍼를 두어 한번에 묶어 보내는 것이 더 효율적이고 빠른 방법이다. 

쓰레통을 비우는 일이라고 생각하면 이해가 쉽다. 쓰레기가 생길때마다 하나하나 밖에 버리는 것보다, 쓰레기통에 하나하나 모았다가, 꽉 차면 버리는게 훨씬 효율적이다 

<br>



##### Scanner 

Java의 기본적인 입출력 방식이다. 

띄어쓰기와 개행문자를 경계로 입력값을 인식하기 때문에, 데이터를 따로 가공할 필요가 없어 편리하다. 지원해주는 메서드가 많고 사용하기 쉽지만, 버퍼 사이즈가 1024 char이기 때문에 많은 입력을 필요로 할 땐 성능상 좋지 않다.

또한 동기화가 되지 않기 때문에 멀티 쓰레드 환경에서 안전하지 않다. 

<br>





##### BufferedReader 

개행문자만 경계로 인식하며 입력받은 데이터는 String으로 고정된다. 그렇기에 따로 데이터를 가공해야하는 경우가 많다. 하지만 Scanner보다 속도가 빠르며 버퍼 크기도 8192 char로 매우 크다. 

또한 동기화 되기 때문에 멀티 쓰레드 환경에서 안전하다. 

<br>





##### BufferedReader 사용법 

```java
impot java.io.*; 
class BufferReaderTest throws IOException{
    BufferedReader br = new BufferedReader(new InputStreamReader(System.in)) ; 
	String s = br.readLine() ; 
	int i = Integer.parseInt(br.readLine()) ;  
}   
```

* 입력은 readLine() 이라는 메서드를 사용한다

* String으로 return 값이 고정되어있기 때문에, 다른 타입으로 입력을 받고자 하면 반드시 형변환이 필요하다. 

* 예외 처리를 반드시 필요로 한다

  try/catch문으로 감싸주거나 throwsIOException을 통해 예외처리한다. 

<br>



 

##### + 공백단위 데이터 가공

> StringTokenizer 또는 String.split() 

```java
BufferedReader br = new BufferedReader(new InputStreamReader(System.in)) ; 
StringTokenizer st = new StringTokenizer(br.readLine()) ; 
while(st.hasMoreTokens())
    System.out.println(st.nextToken()) ; 

```

<br> 















