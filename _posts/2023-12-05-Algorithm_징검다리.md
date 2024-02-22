---
published: true
title: "BinarySearch-징검 다리" 
categories: Algorithm 
tag: [algorithm, 프로그래머스, BinarySearch ] 
toc: true
author_profile: false 
---



##### 문제 : 징검 다리 (LEVEL 4)

바위를 n 개 제거한 뒤 각 지점 사이 거리의 최솟값을 구한다.

그 최솟값 중에 가장 큰 값을 return   

<br>



##### 파라메트릭 서치 

최적화 문제를 결정문제로 바꾸어 해결하는 기법 

되냐/안되냐?를 범위를 좁혀가며 탐색함으로써 최적의 해를 구함

=> 조건 : 현재 이 높이로 자르면 떡 최소 조건 만족 ? --> 만족 여부(예, 아니오) 도출 

=> 범위를 좁힐 때 '이진 탐색' 

<br>



##### 핵심 아이디어

: 지점간 최소 거리를 탐색의 범위로 해서 그 안의 최댓값을 찾음 

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

지점 간 최소거리를 13으로 가정하면, 총 6개의 바위가 제거 된다.

우리가 제거 해야할 바위는 2개이므로 최소 거리를 줄여서 재탐색 해야 한다. 

<br>



##### 구현 (코드)

```java
import java.util.* ; 
class Solution {
    public int solution(int distance, int[] rocks, int n) {
        int answer = 0 ; 
        Arrays.sort(rocks) ;  // 돌이 놓여진 위치 정렬
        
        // 이분 탐색 : 바위를 n개 제거한 뒤 각 지점 사이 거리의 최솟값
				//  ㄴ 그 최솟값이 될 수 있는 값 중 최댓값이 최종적으로 구하는 값 
        int left = 1 ;  
        int right = distance ; 
        while(left <= right){ 
            int mid = (left+right)/2 ; 
            if(getRemovedRockCnt(rocks ,mid, distance)<=n){ // 해당 mid가 답이 되는 돌 제거 수가 조건보다 적으면 
                left = mid + 1 ; // 최소 거리를 더 크게 잡아서 돌을 더 제거시켜주고  
                 answer = mid ; // 다음 반복에서 해가 안나올 수 있기 때문에 허용 가능 범위일 때 일단 answer에 mid값을 저장해준다 **  
            }else{ // 해당 mid가 답이 되기 위해 돌이 더 많이 제거 되면 
                right = mid -1; // 돌간 최소 거리를 작게 잡아서 돌을 덜 제거해야한다 
            }
        }
        return answer ; 
    }
    public int getRemovedRockCnt(int[] rocks, int mid, int distance){
        // mid 가 바위 지점 간 최소거리가 되기 위해 제거해야할 바위의 수를 return한다 
        // mid가 바위 지점간 최소 거리가 되려면 mid보다 작은 숫자의 거리가 나오면 해당 돌을 제거해야한다 
        // 실제로 돌을 제거하지 않고 제거해야'할' 돌의 갯수를 세는게 포인트 
        int before = 0 ; // 출발지 
        int end = distance ; 
        
        int removeCnt = 0 ; 
        for(int i = 0 ; i < rocks.length ; i++){ // 돌 하나하나 검사하면서 
            if(rocks[i] - before < mid){ // 이전 돌과의 거리가 mid보다 작으면 
                removeCnt++ ; // mid가 최솟값이 아닌게 되므로 돌제거 (제거할 돌 수 ++)  
                continue ; 
            }
            before = rocks[i] ;  // '이전 돌'을 검사한 돌로 갱신
        }
        if(end - before < mid) removeCnt++ ; // 마지막 돌과 도착지 사이의 거리(mid보다 짧으면 마지막 돌 제거)
        return removeCnt ; 
    }
}
```

<br>





