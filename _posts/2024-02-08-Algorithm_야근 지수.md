---
published: true
title: "Priority Queue-야근지수" 
categories: Algorithm 
tag: [algorithm, Programmers, Priority-Queue] 
toc: true
author_profile: false 


---



##### 문제

야근 피로도는 야근을 시작한 시점에서 남은 일의 작업량을 제곱하여 더한 값입니다. 

Demi는 N시간 동안 야근 피로도를 최소화하도록 일할 겁니다.

Demi가 1시간 동안 작업량 1만큼을 처리할 수 있다고 할 때, 

퇴근까지 남은 N 시간과 각 일에 대한 작업량 works에 대해 야근 피로도를 최소화한 값을 리턴

<br>



##### 핵심 아이디어 

  : 야근 피로도를 최소화하려면 작업을 마쳤을 때 남은 작업량들 각각의 크기가 작아야 한다 

​    <-> 작업량이 큰 작업을 최우선으로 처리한다 

<br>



#####  우선순위 큐를 활용한 풀이

```java
import java.util.* ; 
class Solution {
    public long solution(int n, int[] works) {
       
      
       PriorityQueue<Integer> q 
           = new PriorityQueue<>(Comparator.reverseOrder()); // 내림차순 우선순위 큐 생성 
        
        for(int work : works){       
           q.add(work) ; // 작업량 크기순으로 정렬
        }
        for(int i = 0 ; i < n ; i++){ // 부여된 작업시간 동안
            if(!q.isEmpty()){ // 작업이 남아있다면 
                int left = q.poll() -1 ;  // 가장 작업량이 큰걸을 꺼내와 작업한 후 
                if(left != 0) // 작업이 완료되지 않았다면
                    q.add(left) ; // 다시 작업 큐에 넣어준다
            }         
        } 
        // 작업 마친 후 ,
        long sum = 0 ; 
        while(!q.isEmpty()){ 
            int temp =q.poll() ; // 큐에 있는 요소를 하나씩 꺼내아 
            sum += temp*temp ; // 제곱해서 누적 (야근지수 계산)
            /*
            잘못된 코드 
            - sum += ( q.poll() * q.poll() ) ; 
            - temp^2  -- 이건 해당 숫자를 거듭제곱 한다는 뜻 --> 문제는 2제곱 해야함
            */
        }
        return sum ; 
    }
}

```

<br>
