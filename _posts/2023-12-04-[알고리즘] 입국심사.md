---
published: true
title: [알고리즘] 이분 탐색 - 탐색의 범위와 정답의 정의 지정 중요 
categories: 알고리즘 
tag: [algorithm, 프로그래머스, BinarySearch ] 
toc: true
author_profile: false 
---



### 9번. 이분 탐색으로 시간의 최솟값 구하기 

---

##### 문제 : 입국 심사 (LEVEL 3)

* 목적 : 모든 사람이 심사를 받는데 걸리는 시간의 최솟값 return  



##### 나의 풀이

1. 심사중 여부를 나타내는 배열 생성 

2. 심사대 배열의 요소 값 정렬 

3. for문 : 1분씩 늘어나는? 

4.  요소값 중 n번째 index에 위치한 심사원의 심사 시간을 첫번째 심사시간으로 fix 

   (한번 쫙 채우기)

5. 심사 중이 아닌 배열 요소 값 중 가장 작은 값 

   vs 심사 중인 배열 요소 중 가장 작은 값 + 남은 심사 시간 



```java
import java.util.Arrays;

public class 입국심사 {
	public long solution(int n, int[] times) {
        
		long answer = 0;        
		Arrays.sort(times);

		int[] checking = new int[times.length] ; 
		for (int i = 1; i <= n ; i++) {
			checking[i] = 1 ; 
		} 
		if(n < times.length) {
			answer = times[n] ; 
		}else {
			answer = times[times.length -1] ; 
		}
       
        while(n>= 0) {
        	answer ++ ;
        	int emptyIndex = Arrays.binarySearch(checking, 0) ; 
        	int checkingIndex = Arrays.binarySearch(checking, 1) ;        
        } // while     
        return answer;
    }
}
```











---

##### 해설 풀이 ✔

##### logic : 정답이 포함된 시간의 범위는 0 ~ 가장 오래 걸린 심사 시간

모든 사람이 심사를 받는데 걸리는 시간의 최솟값

 = 0 (left) ~ 모든 사람이 가장 오래걸리는 심사대에서만 심사를 받은 시간 (right)



[이분 탐색으로 푸는 법] 

1. 내가 찾아야 할 정답의 범위를 left ~ right로 정한다. ** 
  * 탐색의 정의와 범위 지정 중요 (어려운 문제일수록)

    --> **'모든 사람이 심사를 받는데 걸리는 시간'**

    |--------------------------------|-------------------------------------|

2. 정답을 mid로 간주한 후, 해당 정답이 유효한지 살펴본다.

   --> 정답 : 모든 사람이 심사를 받을 수 있는 시간 중 최솟값 

3. 2번 여부를 따지며 left와 right의 범위를 좁힌다.

4. left > right가 되면 반복을 끝내고 답을 반환한다



```java
import java.util.*;

public class 입국심사_해설 {
	public long solution(int n , int[] times) {
		
		long answer = 0 ; 
		Arrays.sort(times); // 이분탐색은 정렬이 필수 
		
		long left = 0 ; 
		long right = times[times.length - 1] * (long) n ; // 모든 사람이 가장 느리게 심사 
		
		while(left <= right) {
			long mid = (left+right)/2 ; 
			long complete = 0 ; //  mid 시간 동안 검사받을 수 있는 사람 수 
			
			for(int i = 0 ; i < times.length ; i ++) {
				complete += mid/ times[i] ; 
                // ex. mid = 10, times = {2, 3, 4}인 경우,  10 / 2 + 10 / 3 + 10 / 4로 총 5+3+2=10명 가능
			}
		
			if(complete < n ) { // 검사를 다 받지 못하면 
				left = mid +1 ; // 오른쪽 범위에서 이분 탐색 
			}else {  // 검사를 다 받을 수 있으면 
				answer = mid ;  // 일단 그 값을 answer에 담아 두고
				right = mid - 1 ; // 왼쪽 범위에서 더 짧은 시간안에 가능한지 검사 	
			}
		}
		return answer  ;  // 더 이상 최대값 안나오면 저장해둔 answer가 답	
	} // solution
} // class 
```

