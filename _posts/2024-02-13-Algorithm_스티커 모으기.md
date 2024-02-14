---
published: true
title: "DP-스티커 모으기" 
categories: Algorithm 
tag: [algorithm, Programmers, DP] 
toc: true
author_profile: false 


---

##### 원형 dp

 : 첫번째 요소 선택 여부로 case 나누는게 중요 
   - case 1 : 첫번째 스티커 제외 나머지를 use 
   - case 2 : 마지막 스티커 제외 나머지를 use



##### 점화식 

dp[i] = Math.max(dp[i-1] , dp[i-2] + sticker[i] ) 

 ※ dp[i] 는 i번째 스티커를 선택하지 않은 경우, 그저 i번째까지 고려했을 때 최대 스티커 합을 의미함 

```java
class Solution {
    public int solution(int sticker[]) {
           
        if(sticker.length == 1)
            return sticker[0]; 
        else if(sticker.length == 2)
            return Math.max(sticker[0],sticker[1]); 
            
        // case 1 
        int[] dp1 = new int[sticker.length] ;     
        dp1[0] = 0 ; 
        dp1[1] = sticker[1] ; 
        
        for(int i = 2 ; i < sticker.length ; i++){
            dp1[i] = Math.max(dp1[i-1] , dp1[i-2] + sticker[i]) ; 
        }

        // case 2 
        int[] dp2 = new int[sticker.length-1] ; 
        dp2[0] = sticker[0] ; 
        dp2[1] = Math.max(dp2[0], sticker[1]) ; 
        
        for(int i = 2 ; i < sticker.length -1 ; i++){
            dp2[i] = Math.max(dp2[i-1] , dp2[i-2] + sticker[i]) ; 
        }
        // 각 dp의 마지막 요소 비교해서 큰 값이 답 
        return Math.max(dp1[dp1.length -1] ,dp2[dp2.length -1]  ) ;   
    } // main 
} // class 
```

