---
published: true
title: "DP-병사 배치하기" 
categories: Algorithm 
tag: [algorithm, 핵심유형, DP, 최장부분증가수열] 
toc: true
author_profile: false 
  
---



##### ❔ 문제

[18353번: 병사 배치하기 (acmicpc.net)](https://www.acmicpc.net/problem/18353)<br>

<br>



##### 💡 핵심 아이디어

> 최장부분증가수열 (LIS)

 EX. {6,2,5,1,7,4,8,3} 이라는 배열이 있을 때, LIS는 {2,5,7,8} 이 된다. 

 즉,  증가하는 부분 수열 중 가장 길이가 긴 것

**=> 도출 방법 : DP 이용** 

dp[i] :  i번째 인덱스에서 끝나는 최장 증가 부분 수열의 길이(최대 값) 

<br>

<br>

##### 🌊 로직 

1. 주어진 배열에서 인덱스를 한 칸씩 늘려가면서

2. 내부 반복문으로 k보다 작은 인덱스들을 하나씩 살펴봄 (soilders[i] < soilders[k]) 

3. dp[i] 업데이트 : Max 비교

   1) 추가하지 않고, 즉 그 smaller 값을 포함시키지 않고 기존의 사용 --> dp[i]

      ex) 그 smaller 값을 포함시키면 그 사이의 값들이 더 많이 못 오는 경우

   2) 그 smaller 값을 추가해 현재 dp[i]+1 (길이 증가 ) --> dp[j] + 1 

      

      <br>

<br>



##### ✅ 코드 

>  O(N^2) 

* 바깥쪽 i : N 
* 안쪽 j  :  평균적으로 N-1/2



```java
package dp ; 

import java.util.* ; 

class 병사배치하기_1_이중for{
    public static int N ; 
    public static ArrayList<Integer> soilders = new ArrayList<>() ; 
    public static int[] dp ; 
    
    public static void main(String[] args){    
     
        // 입력 
        Scanner sc = new Scanner(System.in) ; 
        N = sc.nextInt() ;
        dp = new int[N] ; 
        
        for(int i = 0 ; i < N ; i++){
            soilders.add(sc.nextInt()) ;
        }
        
        // 오름차순으로 변환 
        Collections.reverse(soilders); 
        
        /*dp */        
        // 초기조건 
        for(int i = 0 ; i < N ; i++){
            dp[i] = 1 ; 
        }
  
        // 점화식 
        for(int i = 1 ; i < N ; i++){
            for(int j = 0 ; j < i ; j++){
                if(soilders.get(j) < soilders.get(i))
                    dp[i] = Math.max(dp[i], dp[j]+1) ; 
            }
        }
        
        // 답 반환
        int max = 0 ; 
        for(int i = 0 ; i < N ; i++){
            max = Math.max(max, dp[i]) ; 
        }
        System.out.println(N - max) ; 
    }
}
```

<br>



##### 🤔 문제점

이중 for문 DP : 한 원소의 최장 길이를 잴 때마다 앞의 모든 원소를 순차 탐색 

--> O(N^2) 

--> 이 과정을 LogN으로 줄이는 과정 필요 

**==> 이분 탐색 ** 

<br>



##### 🌊 로직 

1. 이분 탐색으로 검사 원소의 lowerbound를 찾고 : obj 값보다 작은 값들 중 가장 큰 값
2. 해당 원소 값을 교체 
3. 만약 lowerbound가 없다면 뒤에 원소 추가하며 길이 증가 (최장 목표)

<br>



##### 📌 Point

> Greedy : 각각의 원소를 최대한 작은 값으로 교체 

가장 작은 증가 폭을 갖는 부분 수열이 되게끔 시시각각 갱신하는 원리가 길이를 늘리는 원리

-->  '교체'이기 때문에 해당 dp원소의 최종 길이는 변하지 않는다.

--> BUT LIS를 구성하는 정보는 다음 검사 대상의 원소가 포함될 수 있게끔(길이를 늘릴 수 있는) 최상의 정보 제공 : 그 뒤의 원소들은 더 작은 비교 대상을 가지게 되며 수열에 포함될 확률이 더욱 커지기 때문

```tex
arr : 10 50 20 30 15 16 20 
Step 1. dp : 10 50  --> 이대로였으면 30을 끝 원소와 비교했을 때 포함되지 못함 
Step 2. dp : 10 20 --> 50을 20으로 교체함으로써 30이 원소로 포함될 수 있게끔 최적화 
Step 3. dp : 10 20 30  --> 성공적으로 30 포함 
Step 4. dp : 10 15 30  --> 20을 15로 대체 
Step 5. dp : 10 15 16 --> 30을 16으로 대체 --> 30이었으면 뒤의 원소인 20이 수열에 포함되지 했음, 만약 그 이전의 20이 15로 교체되지 않았다면 10 16 30으로 뒤 원소 20이 포함되지 못했음
Step 6. dp : 10 15 16 20 --> 최장 증가 길이 수열 
```

<br>

**※ dp 배열 자체가 최장증가부분수열을 의미 X --> 가능성 및 동일한 길이를 의미 **

<br>

+)  

upperbound : 찾고자 하는 값(obj) 이하가 처음 나타나는 위치 <-> obj값보다 큰 값들 중 가장 작은 값 
lowerbound : 찾고자 하는 값(obj) 이상이 처음 나타나는 위치 <-> obj 값보다 작은 값들 중 가장 큰 값 
	

<br>

##### ✅ 코드 

>  O(N^2) 

* 바깥쪽 i : N 
* 안쪽 j  : log N 

```java
package dp;
import java.util.* ; 

public class 병사배치하기_2_이분탐색 { // O(N log N) 
	
	// 이중 for문' dp : 앞의 모든 원소를 탐색 --> 이 과정을 log(N) 으로 줄여보자 
	// 핵심 원리 : dp 배열에 채워진 원소의 값들이 LIS 배열 자체를 의미하지는 않더라도
	// 			dp 배열의 길이 자체는 LIS 길이와 일치한다. 
	
	public static int[] dp ; 
	public static void main(String[] args) {
		
		Scanner sc = new Scanner(System.in) ; 
		int n = sc.nextInt(); 
		
		int[] soilders = new int[n] ; 
		for(int i = 0 ; i < n ; i++) {
			soilders[i] = sc.nextInt(); 
		}
		
		dp = new int[n] ; 
		
		int LIS = 0 ; 
		
		for(int i = 0 ; i < n ; i++) {
			
			if(soilders[i] > dp[LIS]) 
				dp[LIS++] = soilders[i] ; 
			else {
				int idx = upperbound(soilders[i] , 0, LIS) ;
				dp[idx] = soilders[i] ; 				
			}
		}
		System.out.println(LIS);		 
	}
	

	public static int lowerbound(int num, int start, int end) {
		int res = 0 ; 
		
		while(start <= end) { 
			int mid = (start+end)/2 ; 
			
			if(num <= dp[mid]) {
				res = mid ; 
				end = mid -1 ; 
			}else
				start = mid+1 ; 	 
		}
		return res ;
	}
}
```





