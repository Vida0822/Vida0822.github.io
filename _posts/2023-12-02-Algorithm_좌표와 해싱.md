---
published: true
title: "BinarySearch-평행한 두 직선의 조건"
categories: 알고리즘 
tag: [algorithm, 프로그래머스, BinarySearch, Hash ] 
toc: true
author_profile: false 
---



### 7번. 평행한 두 직선의 조건 

---

##### 문제 : 평행 (LEVEL 2)

* 입력 : 점 네 개의 좌표를 담은 이차원 배열 

➡ 두 개씩 짝지어서 점을 이었을 때,

   직선 두개가 평행한 게 한번이라도 나올 경우 1 , 아니면 0을 반환 

※ 이은 두 점의 x좌표, y좌표는 각각 다른 값을 가짐 



##### 내 풀이

* '평행하다' 

​	: 점 A,B / C, D 가 이어졌다고 했을 때 

​	점 A, C의 x좌표, y 좌표 차이가 점 B, D의 x 좌표, y 좌표 차이와 같으면 평행 



* 해싱 --> 모든 점의 조합 자체를 key 값으로, 평행 여부 (1, 0) 을 value 값으로 저장
  *  key : int { {좌표1} , {좌표2} }   (int[] [])
  * value  : int (0, 1 )



* 반복문을 돌면서 두 점조합 도출 

  1. new int[] []
  2. 평행 여부 확인 --> value 값 도출 
  3. hashMap.put(key, value )

   

* .values.contains(1) --> return 1 , return 0



=> ❌ Hashing으로 풀 수 없음 (수정) ❌ 



---

##### 해설 풀이 ✔

##### logic : 평행은 '기울기' 로 풀어야한다 

(not 두 좌표값 차이 --> 절대값 등 고려해야할 것 多)

=> 입력되는 좌표 개수가 정해져 있고, 많지 않으니 각 좌표에 대한 기울기를 다 구하고 비교해 닾 도출 

```java
class Solution {
    public int solution(int[][] dots) {
        int x1 = dots[0][0];
        int y1 = dots[0][1];
        int x2 = dots[1][0];
        int y2 = dots[1][1];
        int x3 = dots[2][0];
        int y3 = dots[2][1];
        int x4 = dots[3][0];
        int y4 = dots[3][1];
        int answer = 0;
        
        double slope1 = (double) (y2 - y1) / (x2 - x1);
        double slope2 = (double) (y4 - y3) / (x4 - x3);
        if (slope1 == slope2) answer = 1;
        
        slope1 = (double) (y3 - y1) / (x3 - x1);
        slope2 = (double) (y2 - y4) / (x2 - x4);
        if (slope1 == slope2) answer = 1;
        
        slope1 = (double) (y4 - y1) / (x4 - x1);
        slope2 = (double) (y2 - y3) / (x2 - x3);
        if (slope1 == slope2) answer = 1;
        
        return answer;
    }
}
```

