---
published: true
title: "Greedy-큰 수의 법칙" 
categories: Algorithm 
tag: [algorithm, 프로그래머스, greedy] 
toc: true
author_profile: false 

---



##### 문제 

* 책, '이것이 취업을 위한 코딩 테스트다' / ch3 Greedy / 예제 3-2 (p.92)

N :  숫자 종류의 갯수 

M :  더하는 횟수

K : 연속해서 등장할 수 있는 최대 횟수  

=> 해당 조건 하에 주어진 수들들을 M번 더하여 가장 큰 수를 만드시오.

 

##### Step 1. 문제 접근 

최종적으로 숫자들을 M번 더한 값이 가장 큰 값이 되려면, 후보 숫자들 중 최대한 큰 숫자들을 더해나가야한다 



##### Step 2. 선택한 알고리즘 

Greedy 알고리즘 

* Greedy (탐욕법)

  : 현재 상황(고려하는 turn)에서 당장 좋은 것만 고르는 방법 (이후는 고려하지 X)



##### Step 3. 선택 이유 

이 문제는 가장 큰 수들을 가능한 횟수만큼 더하면 최적의 해가 나오는 문제이다(가능한 선에서 계속 큰 숫자만 선택한다.)

즉, 다른 숫자를 더하는 경우는 생각하지 않고 가능한 숫자중 가장 큰 숫자만 매 횟수마다 더한다(M번) . 

=> 가장 큰 숫자를 계속 더해주면서 최대 연속횟수를 초과하지 않게끔만 두번째로 큰 숫자를 중간중간 넣어준다  



##### Step 4. 구현 (코드)

* 해당 원리를 이용하여, 수학적 아이디어를 도출한다 : **반복되는 수열 파악** 

* 가장 큰 수와 두 번째로 크ㅡㄴ수가 더해질 때는 특정한 수열 형태로 일정하게 반복해서 더해짐

  ex) {6, 6, 6, 5} , {6, 6, 6, 5}

  * 반복되는 수열 길이 : K+1

    ​			 ; 연속해서 나오는 가장 큰 수의 횟수 (<-> 연속가능한 최대 등장 횟수) + 두번째 숫자 등장 횟수 (1회)

  * 수열이 반복되는 횟수 : M / K+1 

  * 가장 큰 수가 등장하는 횟수 (수열에 포함) : (M / K+1 ) X K  

  * 수열을 완성시키지 못한 숫자들은 자동적으로 가장 큰 수 : m % (k+1) ;

  

```java
import java.util.* ; 

public class 가장큰수 {

	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in) ; 
		int n = sc.nextInt() ;  // 숫자 종류의 갯수 
	 	int m = sc.nextInt(); // 더하는 횟수 
		int k = sc.nextInt(); // 연속해서 등장할 수 있는 최대 횟수  
		
		// part 1. 숫자 종류 입력 받기 
		int[] arr = new int[n] ; 
		for (int i = 0; i < n ; i++) {
			arr[i] = sc.nextInt(); 
		}
		
		// part 2. 더할 숫자 준비 
		/*
		 * 사용한 아이디어: 가장 큰 숫자를 계속 더해주면서 최대 연속횟수를 초과하지 않게끔만 두번째로 큰 숫자를 중간중간 넣어준다  
		 */
		Arrays.sort(arr); 
		
		int first = arr[n-1] ; 
		int second = arr[n-2] ; 
		
		// part 3. 수학적 아이디어로 변환 - 해당 두 숫자의 반복되는 횟수 구하기 
		int cnt = (m / (k+1)) * k ; // 가장 큰 숫자가 반복되는 횟수 
		cnt += m % (k+1) ; // 수열을 완성시키지 못한 숫자들은 자동적으로 가장 큰 수 -> 가장 큰 수 등장 횟수에 포함 
		
		// part 4. 정답 구하기 - 횟수만큼 숫자 더하기 
		int result = 0 ; 
		result += cnt * first ; // 첫번째로 큰 숫자 * 등장 횟수 
		result += (m - cnt) * second ;  // 두번째로 큰 숫자 * 등장 횟수 (최종 등장 횟수 - 첫번째 수수자 등장 횟수) 
		
		System.out.println(result) ;	
	}
}
```

