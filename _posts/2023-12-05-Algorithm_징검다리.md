---
published: true
title: "[알고리즘] 이분 탐색 - 탐색의 범위와 정답의 정의 지정 중요" 
categories: 알고리즘 
tag: [algorithm, 프로그래머스, BinarySearch ] 
toc: true
author_profile: false 
---



### 10번. 이분 탐색으로 거리의 최댓값 구하기 

---

##### 문제 : 징검 다리 (LEVEL 4)

* 목적 : 바위를 n 개 제거한 뒤 각 지점 사이 거리의 최솟값 중에 가장 큰 값을 return   



##### logic : 지점간 최소 거리를 탐색의 범위로 해서 그 안의 최댓값을 찾음 

|ㅡㅡㅡㅡㅡㅡㅡㅡㅡ|ㅡㅡㅡㅡㅡㅡㅡㅡㅡ|

* 지점 간 최소 거리의 최소값 : 1 
* 지점 간 최소 거리의 최댓값 : 25 (도착지점이 떨어진 위치)

=> 이분 탐색으로 최댓값을 설정하고, 바위를 없애가면서 그게 최댓값이 맞는지 확인 (바위를 이거저거 없애보면서

<-> 바위 사이 거리의 최솟값 중 최대값을 k로 설정했을 때, 없애야할 바위수? 

​	(이게 문제에 주어진 조건 (n)과 같은지 확인)

ex) 첫번째 탐색 

left : 1, right: 25, mid: 13

시작 ~ 2번 바위의 거리는 2이며, 13 보다 작다. 2번 바위 제거 

시작 ~ 4번 바위의 거리는 4이며, 13 보다 작다. 4번 바위 제거 

시작 ~ 11번 바위의 거리는 11이며, 13보다 작다. 11번 바위 제거 

시작 ~ 14번 바위의 거리는 14이며, 13보다 크다. 유지 

14번 ~ 17번 바위의 거리는 3이며, 13보다 작다. 17번 바위 제거 

14번 ~ 21번 바위의 거리는 7이며, 13보다 작다. 21번 바위 제거 

14번 ~ 도착지까지의 거리는 11이며, 13보다 작다. 14번 바위 제거

 

지점 간 최소거리를 13으로 가정하면, 총 6개의 바위가 제거 된다.

우리가 제거 해야할 바위는 2개이므로 최소 거리를 줄여서 재탐색 해야 한다. 





```java
import java.util.*;
class Solution {
    public int solution(int distance, int[] rocks, int n) {
        int answer = 0;
    
        Arrays.sort(rocks);
        
        int left = 1;
        int right = distance;
        while(left <= right){
            int mid = (left + right)/2;
            if(getRemovedRockCnt(rocks, mid, distance) <= n){
                answer = mid;
                left = mid + 1;
            } else {
                right = mid - 1; 
            }
        }
        
        return answer;
    }
    
    public int getRemovedRockCnt(int[] rocks, int mid, int distance){
        // mid가 바위(지점) 간 최소 거리가 되어야 함
        // 그렇게 하기 위해 제거 해야할 바위의 개수를 리턴한다. 
        int before = 0; 
        int end = distance;
        
        int removeCnt = 0;
        for(int i = 0; i < rocks.length; i++){
            if(rocks[i] - before < mid) {
                removeCnt++;
                continue;
            }
            before = rocks[i];
        }
        if(end - before < mid) removeCnt++;

        return removeCnt;
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

