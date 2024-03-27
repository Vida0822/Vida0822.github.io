---
published: true
title: "최대공약수(gcd)와 최소공배수(lcm)" 
categories: Algorithm 
tag: [algorithm, Programmers, gcd, lcm] 
toc: true
author_profile: false 
  
---



##### 최대 공약수

1. 모든 수를 동시에 반드시 나눌 수 있는 숫로 나눈다

2. 더이상 동시에 나눌 수 없으면 stop 

   : 남은 몫이 모두 서로소이다

3. 왼쪽 공약수를 모두 곱한다 

<br>



##### 최소 공배수 

1. 서로소가 아닌 수가 2ㅐ라도 있으면 그 수들의 공약수로 나눈다

   : 나누어 떨어지지 않는 수는 그대로 밑에 내려온다

2. 모든 몫이 서로소이면 멈춘다

3. 왼쪽 공약수와 아래 서로소를 모두 곱한다

<br>



##### 유클리드 호제법

자연수 a,b에 대해 a를 b로 나눈 나머지를 r이라 한다면, a,b의 최대 공약수와 b,r의 최대 공약수는 같다

따라서 a를 b로 나눈 나머지 r을 구하고, b를 r로 나눈 나머지 r2를 구한다. 나머지가 0이 될때 나눈 수가 a,b의 최대 공약수가 된다. 

<br>

유클리드 호제법으로 최대 공약수를 구한 다음, 최소 공배수는 다음 식으로 구할 수 있다

> (axb) / 최대 공약수



수가 여러개라면, A, B의 최소 공배수를 구한 뒤 해당 수와 C의 최소 공배수를 구한다. 

<br>



##### 코드

```java
package math;

public class gcd_lcm {
	
	// 두 수의 gcd, lcm
	public static int gcd(int a, int b) {
		if(a % b == 0)
			return b ;
		return gcd(b, a % b) ; 
	}
	
	public static int lcm(int a, int b) {
		return (a*b) / gcd(a,b) ; 
	}
	
	// 여러수의 gcd, lcm 
	public static int gcd(int[] arr) {
		int result = arr[0] ; 
		for(int i = 1 ; i < arr.length ; i++)
			result = gcd(result, arr[i]) ; 
		return result;  
	}
	
	public static int lcm(int[] arr) {
		int result = arr[0] ; 
		for(int i = 1 ; i < arr.length ; i++)
			result = lcm(result, arr[i]) ; 
		return result ; 
	}
}

```







