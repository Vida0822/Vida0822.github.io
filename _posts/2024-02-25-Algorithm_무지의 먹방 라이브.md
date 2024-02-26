---
published: true
title: "Greedy-무지의 먹방라이브" 
categories: Algorithm 
tag: [algorithm, Programmers, Greedy, PriorityQueue] 
toc: true
author_profile: false 
  
---



##### 문제

각 작업에 대해 [작업이 요청되는 시점, 작업의 소요시간]을 담은 2차원 배열 jobs가 매개변수로 주어질 때, 

작업의 요청부터 종료까지 걸린 시간의 평균을 가장 줄이는 방법으로 처리하면 평균이 얼마가 되는지 return 

이때, 하드디스크가 작업을 수행하고 있지 않을 때에는 먼저 요청이 들어온 작업부터 처리한다.

<br>





##### 내 코드 : ❌ (선형탐색) 

> 시간 복잡도 : O(K)

* 일반 Queue 사용 -->  효율성 테스트 미통과
* k는 무려 20조까지 커질 수 있다. food_times 배열을 k번 만큼 ‘순환적으로’ 순회하면서 1씩 감소시켜서 답을 구하면 (즉, O(k)이면) 효율성 테스트는 절대 통과할 수 없다.
* food_times.length 자체는 데이터가 크지 않기 때문에 상관 x 애초에 k번 순차탐색을 하지 않아야 함

```java 
import java.util.* ; 
class Food{
    int index; 
    int lefts ; 
    
    public Food(int index, int lefts){
        this.index = index; 
        this.lefts = lefts ; 
    }
    public int compareTo(Food other){
        return this.index - other.index ; 
    }
}

class Solution {
    public int solution(int[] food_times, long k) {
        // k초 후의 먹고 있는 음식 번호? 
        // k가 굉장히 크다 -> 단순히 k번 순차탐색하면 food_times.length 만큼 너무 반복하게 된다 --> 다 먹은 음식은 음식 목록에서 제외하고 순차탐색해야 효율적이므로 다른 자료구조를 이용해야한다. --> Queue: 먹으면 다음 순서로 삽입, key(몇번음식) - value(남은 갯수) --> Food 객체 선언 
        Queue<Food> foods = new LinkedList<>(); 
        for(int i = 0 ; i < food_times.length ; i++){
            foods.offer(new Food(i+1, food_times[i])) ; 
        }
        
        long time = 0 ; // 방송 경과 시간
        a: while(true){
            for(int i = 0 ; i < foods.size() ; i++){
                time ++ ; 
                
                // k초가 되면 네트워크 장애 발생 (먹방 중단)
                if(time > k)
                    break a ; 
                
                // 먹기
                Food food = foods.poll() ; 
                if(food.lefts > 1)
                    foods.offer(new Food(food.index, food.lefts-1)) ; 
                if(foods.isEmpty())
                    return -1 ; 
            }
        }
        Food next = foods.poll() ; 
        return next.index ; 
    }
}
```

<br> 



##### 핵심 아이디어 

> Greedy 

* 애초에 먹방 총시간이 k보다 작으면 비교할 필요 없다 (초기에 검사해 문제 종료시키기) 
* 단순히 k번 반복하며 1씩 빼주는게 아닌 시간이 적게 걸리는 음식부터 꺼내서 해당 시간에 음식 종류를 곱한만큼을 총방송시간에 누적한다 (누적할 때, 이미 흐른시간은 제외 시켜줘야한다 )
* 더 이상 다 먹을 수 있는 음식이 없을 때, 음식번호 순서대로 정렬한 후 중단시점에 해당하는 음식번호를 도출한다

<br>



##### 시간 복잡도 

> O(NlogN)

* 총 먹방시간을 구하는 반복문 : O(N)
* 모든 음식을 우선순위큐에 집어 넣는다 : O(NlogN)
* 최소소요시간 음식을 빼주면서 총 방송시간에 누적 : 최악의 경우 n-1개의 음식만큼 최소시간 음식을 빼줄 수 있음 --> O(NlogN)
* 큐에서 하나씩 빼서 ArrayList 구성 : O(NlogN)
* ArrayList 을 음식번호 순으로 정렬 : O(log N)

=> 최종 시간 복잡도 : O(NlogN) , 이때 N(음식 종류)는 2,000이하로 적은 숫자이다!

<br> 





##### 구현(코드)

```java
import java.util.* ; 

class Food implements Comparable<Food>{
    private int index ; // 음식 번호 
    private int time ;  // 먹방시간
    
    public Food(int index, int time){
        this.index = index ; 
        this.time = time ; 
    }
    public int getIndex(){return this.index ; }
    public int getTime() {return this.time ; }
    
    @Override
    public int compareTo(Food other){
        return Integer.compare(this.time, other.getTime()) ; 
    }
}
class Solution{
    public int solution(int[] food_times, long k){
        
        /*
        1. 총 먹방 시간이 k보다 적은 경우 
        */
        long sumTime = 0 ; 
        for(int i = 0 ; i < food_times.length ; i++){ // O(N)
            sumTime+= food_times[i] ; 
        }
        if(sumTime <= k)
            return -1 ; 
        
        /*
        2. 적은 소요시간을 가진 음식을 다 먹은 경우를 반복적으로 도출 --> 총 방송시간에 반영 
        */
        // 소요시간순으로 정렬된 우선순위큐 준비
        PriorityQueue<Food> pq = new PriorityQueue<>() ; 
        for(int i = 0 ; i < food_times.length; i++){ // O(NlogN)
            pq.offer(new Food(i+1 , food_times[i])) ; 
        }
        
        sumTime = 0 ; // 최종적인 누적 먹방 진행 시간 
        long previous = 0 ;  // 이미 흐른 시간  
        long length = food_times.length ; // 음식 갯수 
        while(sumTime + (pq.peek().getTime()-previous)*length <= k){ // 만약 최소시간음식을 기준으로 모든 음식을 먹었을 때 total 누적 방송시간이 방송 중단시간 k를 초과한다면 해당 turn은 돌리지 않는다
            int now = pq.poll().getTime() ; // 해당 음식 먹방시간
            sumTime += (now - previous)*length ;  // 이전에 고려해준 먹방시간은 이미 지난것으로 처리해줘야하므로 총 누적먹방시간을 갱신해줄 땐 '(이 음식을 먹는데 걸리는 시간 - 이미 흐른 먹방시간)* 음식 갯수'를 해줘야한다.   
            previous = now ; // 이미 흐른 시간
            length -- ; // 음식 하나는 다 먹어 없앴으므로
        } // 최악의 경우 n-1개의 음식만큼 최소시간 음식을 빼줄 수 있음 --> O(NlogN)
        
        /*
        3. pq에 남은 음식을 순차적으로 먹는 도중 방송이 중단될것 (도중 다 먹는 음식은 없다) 
        --> 중단되는 시점을 구해서 음식번호 반환 
       	*/ 
        // 음식번호순으로 정렬 
        ArrayList<Food> result = new ArrayList<>() ; 
        while(!pq.isEmpty()){ //  O(NlogN)
            result.add(pq.poll()) ;
        }
        result.sort(Comparator.comparing(Food::getIndex)) ; // 암기 
        
        // 중단 시점 구하기 
        long turn = (k-sumTime) // 중단시점까지 남은 시간을 ex) 17 
            % length ; // 음식 종류로 나눈 나머지는 중단되는 시점 : 음식 종류수는 변하지 않으므로 이게 가능  ex) 8 
        
        // 중단 시점에 먹을 음식의 음식번호 구하기
        int foodNum = result.get((int)turn) // 중단 시점에 먹을 음식의 
            .getIndex() ; // 음식번호 
        return foodNum ;  // 반환
        
        
    }
}
```

<br>



