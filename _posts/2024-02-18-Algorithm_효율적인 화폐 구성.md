---
published: true
title: "DP-효율적인 화폐 구성" 
categories: Algorithm 
tag: [algorithm, Programmers, DP] 
toc: true
author_profile: false 
  
---



##### 문제

N종류의 불규칙한 화폐를 이용해 그 가치의 합이 M원이 되도록 하는 최소 화폐 갯수 

<br>



##### 핵심 아이디어

금액 i를 만들 수 있는 최소한의 화폐 개수를 a(i), 화폐의 단위를 k라고 했을 때 

a(i-k)는 금액 (i-k)를 만들 수 있는 최소한의 화폐 갯수 . 



> 점화식 : a(i) = min: a(i) , a(i-k)+1

만약 (i-k)원을 만드는 방법이 존재하는 경우 해당 최소 동전 갯수 +1 값을 비교해서 최솟값으로 갱신한다  

<br>



##### Point

모든 화폐 단위에 대해 차례대로 tabulation tngod

<br>



##### 코드

```java 
import java.util.* ; 
public class 효율적인화폐구성 {
	public static void main(String[] args) {
		
        // part 1. 초기 상태
		Scanner sc = new Scanner(System.in) ; 
		int n = sc.nextInt() ; 
		int m = sc.nextInt() ; 
		
		int[] coins = new int[n] ; 
		for(int i = 0 ; i < n ; i++) {
			coins[i] = sc.nextInt() ;
		}		
		int[] dp = new int[m+1] ; 
		Arrays.fill(dp, 10001);
		dp[0] = 0 ; 
		
        // part 2. dp 진행
		for(int i = 1 ; i < n ; i ++) { 
			// 모든 화폐 단위에 대해 차례대로 적용
			for(int j = coins[i] ; j <= m ; j++) { 
				// 1개 사용했을때 금액부터 1씩 증가하면서 (3, 4, 7) 
				if(dp[j-coins[i]] != 10001)
					// 만들 금액(j)에서 coin 액수 만큼 뺀 금액을 만들 수 있으면 
					dp[j] = Math.min(dp[j], dp[j-coins[i]]+1) ; 		
                	// 그 금액을 위한 최소 동전 갯수 +1과 기존 j원 구성 최소 갯수와 비교해 갱신
			}
		}
        
        // part 3. 최솟값 출력 (답 출력)
		System.out.println(dp[m] == 100001? -1 : dp[m]);
	}
}
```

<br> 

