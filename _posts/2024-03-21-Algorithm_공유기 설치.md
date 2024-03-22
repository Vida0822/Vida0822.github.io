---
published: true
title: "Binary Search-공유기 설치" 
categories: Algorithm 
tag: [algorithm, Programmers, DP] 
toc: true
author_profile: false 
  
---



##### 문제

도현이의 집 N개가 수직선 위에 있다. 각각의 집의 좌표는 x1, ..., xN!

도현이는 집에 공유기 C개를 가장 인접한 두 공유기 사이의 거리를 가능한 크게 하여 설치하려고 한다.

C개의 공유기를 N개의 집에 적당히 설치해서, 가장 인접한 두 공유기 사이의 거리를 최대로 하는 프로그램을 작성하시오.

첫째 줄에 집의 개수 N (2 ≤ N ≤ 200,000)과 공유기의 개수 C (2 ≤ C ≤ N)이 하나 이상의 빈 칸을 사이에 두고 주어진다. 둘째 줄부터 N개의 줄에는 집의 좌표를 나타내는 xi (0 ≤ xi ≤ 1,000,000,000)가 한 줄에 하나씩 주어진다.

<br>



##### 핵심 아이디어

> 이분 탐색

- n (집의 갯수)가 200000, 좌표가 10000000… 이기 때문에 좌표내 순차탐색으로 해결시 시간초과

- 어떤 값을 mid로 설정할지가 관건 : 가장 인접한 공유기 사이의 거리!

  즉 공유기가 전부 설치되었을 때 각 공유기 사이의 거리는 mid 이상이어야함

<br>



##### 비즈니스 로직

> ‘canInstall() ‘

C개의 공유기를 설치했을 때 가장 인접한 공유기간 거리가 mid 이상인가?

- 첫번째 집엔 무조건 공유기를 설치한다 (count=1) —> 초기 비교기준 공유기
- 두 집간의 거리가 mid 이상이면 공유기 설치 (count++) & 비교기준 공유기 갱신
- mid 이하면 비교기준 공유기 유지

<br>

⇒ 답 도출

- 최종적으로 설치한 공유기가 c 이상이면 true(그중 몇개 안설치해도 성립하니까)

  → 최소 인접 거리를 더 벌려서 다시 설치 (left 옮기기)

- 최종적으로 설치한 공유기가 c 이하면 false

  → 최소 인접 거리를 더 좁혀서 다시 설치 (right 옮기기)

<br>



##### 코드

```java
import java.util.* ;
public class Main{
    public static void main(String[] args){
        // n = 200000 sooobig -> binary Search 
        
        // sort 
        Scanner sc = new Scanner(System.in) ;
        int n = sc.nextInt() ;
        int c = sc.nextInt() ;
        
        int[] houses = new int[n] ; 
        for(int i = 0 ; i < n ; i++){
            houses[i] = sc.nextInt() ; 
        }
        Arrays.sort(houses) ; 
            
        // binary Search ? 
        // mid : the smallest distance between two houses 
        int left = 0 , right = houses[n-1] - houses[0]; 
        int answer = 0 ; 
        while(left <= right){
            int mid = (left+right)/2 ;
            
            if(canInstall(mid, houses, c)){
                answer = mid ;
                left = mid+1 ; 
            }else{
                right = mid-1 ; 
            }
        }
        
        // print answer 
        System.out.println(answer);
    }
    public static boolean canInstall(int mid, int[] houses, int c){
        // can install c machines with maintaining 
        // every distance of two closest houses over mid 
        int count = 1 ;
        int left = houses[0] ; 
        for(int i = 1 ; i < houses.length ; i++){
            if(houses[i] - left >= mid){
                count++ ; // install 
                left = houses[i] ; 
            }
            if(count >= c)
                return true; 
        }
        return false ; 
    }
}
```

<br>
