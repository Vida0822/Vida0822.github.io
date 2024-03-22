---
published: true
title: "DP-정수 삼각형" 
categories: Algorithm 
tag: [algorithm, Programmers, DP] 
toc: true
author_profile: false 
  
---



##### 문제

정수 삼각형의 한 모습이다.

맨 위층 7부터 시작해서 아래에 있는 수 중 하나를 선택하여 아래층으로 내려올 때, 이제까지 선택된 수의 합이 최대가 되는 경로를 구하는 프로그램을 작성하라. 아래층에 있는 수는 현재 층에서 선택된 수의 대각선 왼쪽 또는 대각선 오른쪽에 있는 것 중에서만 선택할 수 있다.

<br>



##### 핵심 아이디어

> DP

각 행의 특정 열까지의 최대합은 이전 행의 왼쪽, 오른쪽 열까지의 최대합+ 그 행의 값 까지이다

따라서, 점화식은 다음과 같다.

dp[i][j] = Math.max(dp[i-1][j-1] + tri[i][j] , dp[i-1][j] + tri[i][j])

<br>



##### Points

- ‘삼각형‘을 코드로 구현하는 것, 즉 배열의 index를 어떻게 처리해줄지가 관건이다

  문제가 삼각형이라해서 꼭 dp. index도 삼각형으로 해줄필요없다. i 행 첫번째 index부터 채워주되, 이전 행의 열값을 비교할 때 왼쪽위를 j-1로, 오른쪽 위를 j 로 가져와 비교함으로써 삼각형을 구현한다.

- Out Of Range 방지

  열을 채워줄 때 0번부터 채워주면 왼쪽에서 내려올 때 그 열값이 0 이전의 값인지 체크하는 코드가 추가로 필요하다. 이를 방지하기 위해 0행, 0열을 빈배열로 만들어주고 실제 값은 1행, 1열부터 채운다.

  이렇게 하면 왼쪽위의 값을 참조할 때 빈 요소(0)을 참조하므로 최댓값 비교에서 자연스럽게 제외된다.

  이렇게 쿠션을 만들어주는 트릭은 자주 사용되므로 활용하자

<br>



##### 코드

```java
import java.util.* ;
public class Main{
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in) ; 
        int n = sc.nextInt() ; 
        int[][] tri = new int[n+1][n+1] ; 
        
        // input 
        for(int i = 1 ; i <= n ; i++){
            tri[i][0] = 0 ; 
            for(int j = 1 ; j <= i ; j++){
                tri[i][j] = sc.nextInt() ; 
            }
        }
        
				// dp 
				for(int i = 2 ; i <= n ; i++){
            for(int j = 1 ; j <= i ; j++){
                tri[i][j] = Math.max(tri[i-1][j-1]+tri[i][j] 
                                     , tri[i-1][j]+tri[i][j]) ; 
            }    
        }
        
        // print answer
        int answer = 0 ; 
        for(int i = 1 ; i <= n ; i++){
            answer = Math.max(answer, tri[n][i]) ; 
        }
        System.out.println(answer); 
    }
}
```

<br>

