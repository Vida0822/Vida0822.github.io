---
published: true
title: "Sort-인사고과" 
categories: Algorithm 
tag: [algorithm, Sort, Binary, Octal, Hex] 
toc: true
author_profile: false 
  
---



##### ❔ 문제

카카오 lv3. 인사고과 

[코딩테스트 연습 - 인사고과 | 프로그래머스 스쿨 (programmers.co.kr)](https://school.programmers.co.kr/learn/courses/30/lessons/152995)

<br>

<br>



##### 💡 핵심 아이디어

> 정렬 문제 => 그 정렬 조건이 까다로운 

1. 두 점수의 합이 높은 순으로 정렬 
2. 단, 두 점수가 다른 임의의 사원보다 모두 낮은 경우 석차에 포함 X 
3. 동점일 경우 같은 등수로 계산되며 건너뛰고 다음 등수 인덱싱 
4. scorest<=100000 -> O(N) 또는 O(NlogN)으로 처리해야함 

<br>

<br>



##### ❌ 내 코드

```java
import java.util.* ; 

class Employee implements Comparable<Employee>{
    int index ; 
    int a ; 
    int b ; 
    
    public Employee(int index, int a, int b){
        this.index = index ;
        this.a = a; 
        this.b = b;
    }
    
    @Override
    public int compareTo(Employee other){
        return (other.a + other.b) - (this.a + this.b) ; 
    }
}

class Solution {
    public int solution(int[][] scores) {
        
        PriorityQueue<Employee> pq = new PriorityQueue<>() ; 
        int a_min = 0  , b_min = 0 ; 
        
        for(int i = 0 ; i < scores.length ; i++){
            pq.add(new Employee(i, scores[i][0], scores[i][1])) ; 
            a_min = Math.min(a_min, scores[i][0]) ; 
            b_min = Math.min(b_min, scores[i][1]) ; 
        }
        
        int count = 1 ; 
        int rank = 0 ; 
        int prev = 0 ; 
        while(!pq.isEmpty()){
            Employee cur = pq.poll() ; 
            // 두 점수 모두 높은 사람이 있는지 확인 --> 제외 (이때 순차탐색은 X) 
            
            if(cur.a < a_min && cur.b < b_min)
                continue; 
            if(prev != cur.a + cur.b){
                rank += count ; 
                count = 1 ;
            }else{
                count++; 
            }
            prev = cur.a + cur.b ; 
            
            if(cur.index == 0)
                return rank ; 
    
        }
        return -1;  
    }
}
```

<br>

<br>





##### 🌊 로직 : 두번 정렬

>  Step 1. 인센티브를 못받는 사람 걸러내기 (정렬 대상에서 제외)

1. a점수 내림차순으로 정렬
2. 이전 비교대상(prevs)들의 b점수 중 최댓값이 현재 검사대상(cur)의 b점수보다 높으면 검사대상에서 제외
3. 만약 제외되는 대상 중 완호 점수가 있으면 -1 반환 후 종료

<br> 

>  Step 2. 점수합으로 점수 구하기 

1. a점수 + b점수 내림차순으로 정렬 
2. 완호의 등수 구하기 

<br>

<br>



##### 🎈Point :

1. 구현 : 두 점수 모두 높은 사람이 있는지 확인

'두 점수가 모두 높다' <-> a점수도 높고, b점수도 높다 + 그런 사람이 1명이라도 있다 

**<-> 나보다 a점수와 b점수가 모두 높으면 안된다! **

<br>

=> a점수 내림차순으로 정렬 + 이전 비교대상(prevs)들의 b점수 중 최댓값이 현재 검사대상(cur)의 b점수보다 높은지 확인한다.

a점수가 내림차순이므로 cur의 a점수는 자연히 이전 비교대상들의 a점수보다 낮다 (prevs'a > cur.a) 만족

만약 b점수가 이전 검사대상들의 b점수보다 낮으면(some of prevs'b > cur.b) 

현재 검사 대상보다 두 점수 모두 높은 직원이 존재한다. 

<br>

※ 현재 검사 대상의 뒷 직원들과 비교할 필요 없다. 

a점수로 내림차순 했기때문에 무족권 cur.a > nexts'a 이기 때문이다. 

<br> 

2. 동일 점수는 동일 석차로 출력 

완호보다 점수가 높은 대상들은 같은 등수가 몇명이 있어도 어쨌든 그 인원수 만큼 넘어간 후 완호의 등수가 다시 인덱싱되기 때문에

결과적으로 직원수를 counting한 값이 등수 그 자체이다. 

<br>

만약 완호와 같은 등수가 여러명이라면 따로 등수를 구하지 않고 현재까지 계산한 counting을 반환한다.  

같은 점수는 동일 등수로 처리되기 때문에 그 다음 등수를 고려해줄 필요가 없기 때문이다. 

ex) 현재까지 카운팅된 등수 : 3 -> 완호 등수 2명 : 모두 4등 -> 그냥 4등 반환

<br>

<br>



##### ✅ 코드 

>  최악의 경우 O(N)  : O(scores.length) = O(100000)

정렬 후 전체검사 x 2 

```java
import java.util.Arrays;

class Solution {
    public int solution(int[][] scores) {
        int answer = 0;
        int size = scores.length;
        int n = scores[0][0];
        int m = scores[0][1];
        
        // step 1. 못받는 사람 걸러내기 
        
        // a점수 내림차순, b점수 오름차순 
        Arrays.sort(scores, (o1, o2) -> { // O(logN)
            if (o1[0] == o2[0]) {
                return o1[1] - o2[1];
            }
            return o2[0] - o1[0];
        });
        
        // 어차피 a점수는 내림차순으로 정렬되어 있으므로(prevs'a > cur.a), b점수만 이전 사람 중 최대와 비교해 작으면 제외 (prevs'b > cur.b ? )
        int max_b = scores[0][1];
        
        for(int i = 1; i<size; i++) { // O(N)
           if (scores[i][1] < max_b) { // 인센티브를 받지 못하는 경우
               if (scores[i][0] == n && scores[i][1] == m) // 완호 점수인 경우
                   return -1;
               
               // 정렬 대상에서 제외 (좋은 트릭**)
               scores[i][0] = -1;
               scores[i][1] = -1;
           } else {
                max_b = scores[i][1];
           }
        }
        
        // Step 2. 점수합으로 등수 구하기 
        Arrays.sort(scores, (o1, o2) -> (o2[0] + o2[1]) - (o1[0] + o1[1])); // O(logN)
        
        answer = 1;
        // counting
        for(int i = 0; i<size; i++) { // O(N)
            if (scores[i][0] + scores[i][1] > n + m) {
                // 같은 등수가 있어도 어쨌든 완호보다는 등수가 높고 그 사람들을 다 counting한 뒤의 등수가 완호등수기 때문에 그냥 count  
                answer ++;
            } else {
                // 완호와 같은 등수가 여러명이라면 count하지 않고 바로 현재까지의 등수 반환 
                break;
            }
        }    
        return answer;
    }
}
```

<br>

<br>

