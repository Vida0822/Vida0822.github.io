---
published: true
title: "Greedy-숫자카드게임" 
categories: Algorithm 
tag: [algorithm, 프로그래머스, greedy] 
toc: true
author_profile: false 


---



##### 문제 

- 책, '이것이 취업을 위한 코딩 테스트다' / ch3 Greedy / 예제 3-3 (p.96)

각 행에서 가장 작은 숫자카드들을 뽑은 후, 그 중 가장 큰 수를 도출하는 프로그램을 작성하시오 



##### Step 1. 문제 접근 

각 행을 검사할때마다 가장 작은 수를 뽑고, 해당 값들을 비교해서 가장 큰 값을 도출한다 



##### Step 2. 선택한 알고리즘 

Greedy 알고리즘 

- Greedy (탐욕법)

  : 현재 상황(고려하는 turn)에서 당장 좋은 것만 고르는 방법 (이후는 고려하지 X )



##### Step 3. 선택 이유 

매 행을 검사할 때마다 다음 행의 최솟값을 생각하지 않고 지금 당장의 최솟값을 뽑고 최댓값으로 갱신한다. 

즉, 지역적 최적답을 반복적으로 구하면 결국 전체적 최적의 해가 되기에 greedy 알고리즘이 효과적이다. 

시간 복잡도는 각 행마다 동일한 연산을 수행하므로 O(N)이다. 



##### Step 4. 구현 (코드)

```java
package greedy;

import java.util.Scanner;

public class 숫자카드게임 {

	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in); 
		
		// 숫자카드 행, 열 입력  
		int n = sc.nextInt() ; 
		int m = sc.nextInt() ;
		int result = 0 ; 
		
		for(int i = 0 ; i < n ; i++) {
			int min_value = 10001 ; 
			for(int j = 0 ; j < m ; j++) {
				/*
				 * 아이디어 주목 : 숫자카드를 한번에 완성시킨 후 비교하는게 아닌 입려 받으면서 최솟값 갱신   
				 	=> 각 줄의 최솟값 도출 
				 */
				int x = sc.nextInt(); 
				min_value = Math.min(min_value, x); 
			} // j 
			// 행의 최솟값을 도출한 후 바로 기존 최댓값과 비교하여 갱신 
			result = Math.max(result, min_value) ; 
		} // i 
		System.out.println(result) ; 
	} // main 
} // class 

```

