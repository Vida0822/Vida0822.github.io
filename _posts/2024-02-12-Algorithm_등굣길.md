---
published: true
title: "DP-등굣길" 
categories: Algorithm 
tag: [algorithm, Programmers, DP] 
toc: true
author_profile: false 


---



```java
import java.util.* ; 
class Solution {   
    public static int count = 0 ; 
    public static int[] dx = {1, 0} ; 
    public static int[] dy = {0, 1} ; 
    public static boolean[][] canGo  ; 
    public static int n, m ; 
    public static int[][] dp ; 
    
    public int solution(int m, int n, int[][] puddles) {
        this.canGo = new boolean[n+2][m+2] ; 
        this.dp = new int[n+2][m+2] ; 
        this.m = m ; 
        this.n = n ; 

        /*
        1. 물에 잠긴 지역 표시 
        */
        for(int i = 1 ; i < canGo.length ; i++){
            for(int j = 1 ; j < canGo[i].length ; j++){
                canGo[i][j] = true ;  
            }
        }
        for(int i = 0 ; i < puddles.length; i++){
            int x = puddles[i][0] ; // 2
            int y = puddles[i][1] ; // 3 
            canGo[y][x] = false ;  
        }
        
        /*
        2. 초기조건 
        */
        dp[1][1] = 1 ; 
        for(int i = 1 ; i <= n ; i++){
            if(!canGo[i][1])
                break;                 
            dp[i][1] = 1 ;            
        }
        for(int i = 1 ; i <= m ; i++){
            if(!canGo[1][i])
                break ; 
            dp[1][i] = 1 ; 
        }
          /*
        3. 점화식 적용 
        (x,y) 좌표에서의 경로 갯수의 최댓값에은 (x-1,y) 좌표에서 경로갯수의 최댓값, (x, y-1) 좌표에서 경로갯수의 최댓값 +1(만약 둘 모두에서 올 수 있는 경우 )  
        */
        for(int i = 2 ; i <= n ; i++){
            for(int j = 2 ; j <= m ; j++){
                if(!canGo[i][j])
                    continue ; 
                
                if(canGo[i-1][j] && canGo[i][j-1])
                    dp[i][j] = (dp[i-1][j] + dp[i][j-1])%1000000007;
                else
                    dp[i][j] = (Math.max(dp[i-1][j] , dp[i][j-1]))%1000000007 ; 
            }  
        } // for 
        /*
        4. 최댓값 반환 
        */
        return dp[n][m] % 1000000007 ; 
    }
}
```

