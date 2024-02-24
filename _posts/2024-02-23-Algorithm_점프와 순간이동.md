---
published: true
title: "Greedy-점프와 순간이동" 
categories: Algorithm 
tag: [algorithm, Programmers, Greedy] 
toc: true
author_profile: false 
  
---



##### 문제

한 번에 K 칸을 앞으로 점프하거나, (현재까지 온 거리) x 2 에 해당하는 위치로 순간이동을 할 수 있음

순간이동은 건전지가 안들지만 점프는 1칸마다 건전지 1개 소모.

이동하려는 거리 N이 주어졌을 때, 사용해야 하는 건전지 사용량의 최솟값을 return

<br>



##### 핵심 아이디어 

> Greedy 

무조건 순간이동을 할 수 있으면 하는게 좋으므로 최종 거리에서 역순으로 계산하면서 2의 배수면 2로 나눠주기 (위치 이동) 

불가능하면 어쩔수 없이 배터리 사용해 위치 재갱신

--> 위치가 0이 될때까지 매순간 최선의 선택(순간이동)을 하려고 노력하며 위치값 줄여주면 결국 그 답이 최적의 답(최소 배터리 사용)

<br>



##### Point 

최종거리에서 '역순으로' 빼주면서 위치 갱신

<br>





##### 내 코드 : 처리하지 못한 경우 재검사 

```java 
public class Solution {
    public int solution(int n) {

        int k = 0 ; 
        while(n > 0){
            if(n%2 == 0)
                n /= 2 ; 
            else{
                k++ ;
                n-- ;     
            }
        }// while 
        return k ; 
    }
}
```

<br> 


