---
published: true
title: "Sort-단속 카메라" 
categories: Algorithm 
tag: [algorithm, Programmers, Greedy] 
toc: true
author_profile: false 


---



##### 문제

고속도로를 이동하는 모든 차량이 고속도로를 이용하면서 단속용 카메라를 한 번은 만나도록 카메라를 설치하려고 합니다.

고속도로를 이동하는 차량의 경로 routes가 매개변수로 주어질 때, 모든 차량이 한 번은 단속용 카메라를 만나도록 하려면 

최소 몇 대의 카메라를 설치해야 하는지를 return

<br>



##### 영향 문제 (effect problem) 

특정 task를 진행했을 때 문제 상황에 영향을 주기 때문에 특정 task를 진행할 때 마다 해당 변화를 적용해야한다 

영향 '범위'를 특정하기 때문에 특정 조건으로 정렬하는게 우선되는 경우가 많다

<br>



##### 핵심 아이디어 

진출/진입 가로 그래프가 이어져있지 않은 부분을 기준으로 그룹을 나눠 각각 감시 카메라를 설치한다

 => 가장 먼저 나가는 차량부터 진출 지점을 기준으로 카메라를 설치한 후, 이후에 나가는 차량들을 검사

<br>



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

<br>

