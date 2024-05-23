---
published: true
title: "DP-퇴사" 
categories: Algorithm 
tag: [algorithm, DP, Samsung, 역순DP] 
toc: true
author_profile: false 
  
---



##### ❔ 문제

백준 14501번 - 퇴사 (삼성)

[14501번: 퇴사 (acmicpc.net)](https://www.acmicpc.net/problem/14501)

<br>

<br>



##### 💡 핵심 아이디어

이전의 계산 결과 값을 재활용하는 dp 문제이다.  

<br>

<br>



##### ❌ 내 코드 : 정방향 DP 

```java
import java.util.*; 

public class 퇴사{
    public static void main(String[] args){
        
        // dp 초기조건
        int[] dp = new int[N] ; 
        dp[0] = P[0] ; 
        
        // dp 점화식 
        for(int i = 1 ; i < N ; i++){
            
            // 'possible' 구현 
            int j = i ; 
            while(j > 0){
                j--;
                
                if(i-j >= T[j]){     
                	dp[i] = Math.max(dp[i], dp[j]) ; 
                }                
            } // while 
            
            // 조건 1 : 오늘 진행하는 상담이 N일을 넘어가면 진행 불가 
            // 조건 2 : 오늘것만 진행하는게 더 낫다면 이전 진행 x 
            if(i+T[i] <= N) {
            	dp[i] += P[i] ; 	
            } 
            	         
        } // for 
        
        // print answer 
        int answer = 0; 
        for(int i = 0 ; i < N ; i++) {
        	answer = Math.max(dp[i], answer);
        }
        System.out.println(answer) ; 
    }
}

```

<br>

<br>



##### 🌊 로직 : 거꾸로 DP 

> dp[i] = i번째 날부터 마지막 날까지 낼 수 있는 최대 이익 

현재 날짜의 최대 이윤 

= 현재 상담 날짜의 이윤 : P[i] 

➕ 현재 상담을 마친 일자로부터의 최대 이윤 : dp[ i + T[i] ]

<br>

<br>





##### 🎈Point 

> 1. 별도의 DP 배열

재참조 여부가 아리까리 할 때나 반복적으로 뒤쪽의 배열을 재참조 하는 경우 별도의 dp배열을 선언해서 사용한다. 

기존의 P배열은 수정하지 않는다 

<br>

> 2. 점화식 수행하면서 답 갱신 

이러한 dp 같은 경우 해당 날짜에 상담을 진행하지 않는 것이 더 이득일 경우 

해당 날짜의 dp값은 '상담을 진행하지 않았을 때의' 최댓값으로 갱신해야 한다. 

따라서 현재까지 검사한 이익 중 '최댓값'을 계속 갱신하며 비교, dp값 갱신해야 한다. 

따라서 최종적으로 dp[0] = answer가 된다. 

<br>

<br>





##### ✅ 코드 

>  최악의 경우 O(N) 

 : 뒤쪽으로 계속 타고올라갔으면 O(N^2)이지만 (해당 날짜 + T[i]) 의 'dp 값'을 바로 비교하면 되기 때문에 뒤쪽을 다시 검사할 필요가 없다 

ex) 1일 -> 3일 -> 8일 -> 11일 (X)  , 1일 -> 3일의 dp값 

따라서 시간 복잡도는 i = N-1 ~> 0 인 O(N)이다. 

```java
import java.util.*; 

public class 퇴사{
    public static void main(String[] args){
    	
        // N+1일 후 퇴사 --> 남은 N일동안 최대한 많은 상담 
        Scanner sc = new Scanner(System.in) ; 
        int N = sc.nextInt() ; 
        
        int[] T = new int[N] ; 
        int[] P = new int[N] ; 
        for(int i = 0 ; i < N ; i++){ // O(N)
            T[i] = sc.nextInt() ; 
            P[i] = sc.nextInt() ; 
        }
        
        // dp 초기조건
        int[] dp = new int[N+1] ; 
        int answer = 0; 
        
        // dp 점화식 
        for(int i = N-1 ; i >= 0 ; i--) { // O(N)
        	int time = T[i] + i ; // i일에 배정된 상담 완료 날짜 
        	
        	if(time <= N) {
        		dp[i] = Math.max(P[i]+dp[time], answer) ; 
        		answer = dp[i] ;
        	}else
        		dp[i] = answer ; 
        		
        }
        
        // print answer 
        System.out.println(dp[0]) ; 
    }
}

```

<br>

<br>

