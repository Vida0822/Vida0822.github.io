---
published: true
title: "기지국 설치" 
categories: Algorithm 
tag: [algorithm, 프로그래머스, greedy] 
author_profile: false 

---



##### 문제 

기지국이 설치된 아파트를 기준으로 전파를 양쪽으로 W만큼 전달할 수 있으며, 초기엔 모든 아파트에 전파를 전달 할 수 없다. 

아파트의 갯수(N), 설치된 아파트의 번호가 담긴 1차원 배열(stations), 전파의 도달거리 W가 매개변수로 주어질 때, 모든 아파트에 전파를 전달하기 위해 증설해야 할 기지국 개수의 최솟값 구하는 프로그램을 작성하시오. 

  

##### Step 1. 문제 접근 

기지국(으로 전파받는 지역)을 기준으로 그룹을 나누어 전파가 전달되지 않는 구역을 하나의 그룹으로 삼는다. 

해당 그룹의 칸 수를 1+2w으로 나눈 몫+1 만큼 기지국을 설치한다 



##### Step 2. 선택한 알고리즘 

구현 



##### Step 3. 선택 이유 

별다른 알고리즘을 필요로 하지 않고 반복문을 돌며 기지국 전파 X 구역의 크기를 구하고 그 크기를 1+2w로 나눈 몫 만큼 기지국 갯수를 ++ 해준다 



##### Step 4. 구현 (코드)

```java
import java.util.*;
class Solution {
    public static int solution(int n, int[] stations, int w)
    {
        int count = 0;
        int state = 0;
        int left = 1;
        int newleft = 0;

        while(true) {
            if(state<stations.length && left >= stations[state] - w) {
                left = stations[state]+w+1;
                state++;
            }
            else {
                newleft = left+w;
                if( (state <= stations.length-2) && (newleft >= stations[state+1]-w))
                    newleft = stations[state+1]-w-1;

                left=newleft+w+1;
                count++;
            }
            if(left > n)
                break;
        }
        return count;
    }
}
```

