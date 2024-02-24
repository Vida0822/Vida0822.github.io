---
published: true
title: "BinarySearch-입국 심사" 
categories: Algorithm 
tag: [algorithm, 프로그래머스, BinarySearch ] 
toc: true
author_profile: false 
---



##### 문제 : 입국 심사 (LEVEL 3)

* 목적 : 모든 사람이 심사를 받는데 걸리는 시간의 최솟값 return  

<br> 



##### 이분 탐색 

1. 탐색의 정의와 범위 지정 중요 (어려운 문제일수록) 

   : **'모든 사람이 심사를 받는데 걸리는 시간'**

2. 내가 찾아야 할 정답의 범위를 left ~ right로 정한다. ** 

   : 0 ~ 가장 오래 걸린 심사 시간(최악의 경우 심사시간)

  * left : 0 
  * rigth : times[0] * (long) n  ; // 모든 사람이 가장 빠른 심사대에서만 검사  

3. 정답을 mid로 간주한 후, 해당 정답이 유효한지 살펴본다.

--> 정답 : 모든 사람이 심사를 받을 수 있는 시간 중 최솟값 

4. 2번 여부를 따지며 left와 right의 범위를 좁힌다.

5. left > right가 되면 반복을 끝내고 답을 반환한다

<br> 



##### 비즈니스 로직 

이분탐색 문제에선 탐색 범위 지정 다음으로 해당 mid 값이 유효한지를 검사하는 **실질적인 핵심로직**이 중요하다

> Point : 사람 입장이 아닌 '심사대 입장'에서 생각

대기 최적화(남은시간+대기시간 고려)는 사람들이 알아서 하니 각 심사대는 최대한의 효율로 알아서 돌아간다고 가정 

--> 각 심사대별로 주어진 심사시간(mid)안에 검사 가능한 모든 인원수를 누적하면 됨 

 ex. mid = 10, times = {2, 3, 4}인 경우,  10 / 2 + 10 / 3 + 10 / 4로 총 5+3+2=10명 가능

<br> 

##### 주의 사항 

사용되는 자료형이 long이라 잘 맞춰주지 않으면 테스트 케이스 오류가 난다 

<br>



##### 구현(코드)

```java
import java.util.* ; 
class Solution {
    public long solution(int n, int[] times) {
        
        Arrays.sort(times) ; 
       
        long answer = 0 ; 
        long left = 0 , right = times[0] * (long) n  ; 
        while(left <= right){
            long mid = (left + right) / 2 ;             
            if(isPossible(n,times,mid)){
                answer = mid ; 
                right = mid -1 ; 
            }else
                left = mid + 1; 
        }
        return answer ; 
    }
    
    // ** 비즈니스 로직 ** : 해당 시간안에 n 명 모두 심사 가능 ? 
    public boolean isPossible(int n, int[] times, long maxMinutes){
        long complete = 0 ; 
        for(int i = 0 ; i < times.length ; i ++) {
			complete += maxMinutes/ times[i] ; // ** 
		}
        if(complete < n)
            return false ;
        return true ; 
    }
}
```

