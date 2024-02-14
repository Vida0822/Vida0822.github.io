---
published: true
title: "Greedy-단속 카메라" 
categories: Algorithm 
tag: [algorithm, Programmers, Greedy] 
toc: true
author_profile: false 


---



##### 핵심 아이디어 

진출/진입 가로 그래프가 이어져있지 않은 부분을 기준으로 그룹을 나눠 각각 감시 카메라를 설치한다

 => 가장 먼저 나가는 차량부터 진출 지점을 기준으로 카메라를 설치한 후, 이후에 나가는 차량들을 검사



##### 코드 

```java
import java.util.* ; 
class Solution {
    public int solution(int[][] routes) {
        // 1. 차들을 '나간지점'을 기준으로 오름차순 
        // 	[-18, -16], [-20, -15], [-14, -5], [-5, -3]
        ArrayList<Car> list = new ArrayList<>() ; 
        for(int i = 0 ; i < routes.length ; i++){
            Car car = new Car(routes[i][0], routes[i][1]) ; 
            list.add(car) ; 
        } // for 
        Collections.sort(list, (a,b) -> {  // 비교하는 두 Car 
            return Integer.compare(a.out, b.out) ; // 진출 시점으로 정렬 
        }) ; 
            
        // 2. 감시 카메라 설치 --> 해당 감시카메라로 찍을 수 있는 차 확인 
        boolean[] included = new boolean[list.size()] ; 
        // 해당 차가 감시카메라 영역에 포함되었는지 여부 
        
        int camera = 0 ; 
        for(int i = 0 ; i < list.size() ; i++){
            if(included[i]) 
                continue ; 
            
            camera++ ; // 감시 카메라에 포함되지 않으면, 새로운 감시카메라 설치
            // 해당 감시 카메라 영역에 포함되는 차들 검사 
            for(int j = i+1 ; j < list.size(); j++){
                // 검사 대상 차들은 감시카메라 지점보다 더 늦게 나감  
                if(list.get(i).out >= list.get(j).in){
                    // 진출 시점이 감시카메라 설치 위치보다 이전이면  
                    included[j] = true ; // 해당 차는 감시카메라 지점보다 이전에 들어오고, 이후에 나가는 것 ==> 해당 감시카메라 영역에 포함
                } // if 
            } // j             
        } // i
        // 3. 최종적인 감시카메라 갯수 반환 
        return camera ; 
        
    } // solution 
} // class 

class Car{
    int in ; 
    int out ; 
    
    public Car(int in, int out){
        this.in = in ; 
        this.out = out ; 
    }
}
```

