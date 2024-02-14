---
published: true
title: "DP-Choose One Of Two"
categories: Algorithm 
tag: [algorithm, CodeTree, dp, 점화식 ] 
toc: true
author_profile: false 

---



##### 문제 : Choose One Of Two

* 2n 개의 trial 
  2n개의 red card + 2n개의 blue 카드 
  --> 각 trial에서 red or blue 카드 선택
  --> 궁극적으로 red, blue 카드 각각 n개씩 선택되어야함 

  

##### 세운 로직 

* 각 카드 
  * red[2n] ,  blue[2n]

* dp[2n] [2] : red카드를 i번째까지 j개의 red 카드를 골랐다고 했을 때 얻을 수 있는 최대 숫자
  최대값을 도출하는데 각 카드가 현재까지 몇번 선택되었는지 표시 필요 

* 점화식 ※ red 카드는 n개만 나올 수 있음
  * i번째에 red 카드를 골랐을 때 
    : 1번째까지 j-1 개의 red 카드를 골랐어야함
  * i번째에 blue 카드를 골랐을 때 
    : i-1 번째까지 j개의 red 카드를 골랐어야함

=> dp[i] [j] = Max ( dp[i-1] [j-1] + red[i] , dp[i-1] [j] + blue[i] ) 
ㄴ i 는 2n 까지, j는 n까지 채워짐 



* 초기 조건 

0) dp[0][0] = 0 ; 
1) 1열 (i < n )

​	dp[1,,0] = 첫번째 trial에서 blue 카드 골랐을 때 

​	dp[2,0] = 두번째 trial에서 blue 카드 골랐을 때 
​		: 	

2) 대각선열 (i < n )

​	dp[1][1] = 첫번째 trial 에서 red 카드 골랐을때

​	dp[2][2] = 두번째 trial 에서 red 카드 골랐을때

​		: 



##### 내 풀이  ✔

```java
import java.util.* ; 

public class ChooseOneOfTwo {

	    public static int n ; 
        public static int MAX_VALUE = 100 ; 
	    public static int[] red = new int[2* MAX_VALUE+ 1 ] ;
	    public static int[] blue = new int[2 * MAX_VALUE+1] ;
	    public static int[][] dp = new int[2 * MAX_VALUE+1][MAX_VALUE+1] ; 


	    public static void initialize(){ 
	        for(int i = 0; i <= 2*n; i++){
	          for(int j = 0 ; j <= n ; j++){
	            dp[i][j] = Integer.MIN_VALUE; 
	          }  
	        }
             dp[0][0] = 0 ; 
	    }

	    public static void main(String[] args) {
	        Scanner sc = new Scanner(System.in) ; 
	        n = sc.nextInt() ; 

	        for(int i = 1 ; i <= 2*n; i++){
	            red[i] = sc.nextInt() ; 
	            blue[i] = sc.nextInt() ; 
	        }

	        initialize() ; 

	        for(int i = 1 ; i <= 2*n ; i++){    
                if(i <= n ){
                    dp[i][0] = dp[i-1][0] + blue[i] ; 
                    dp[i][i] = dp[i-1][i-1] + red[i] ;
                }
	            
	            for(int j = 1 ; j <= n ; j++){
	                dp[i][j] 
	                = Math.max ( dp[i][j] , 
                       Math.max(dp[i-1][j-1] + red[i] , dp[i-1][j] + blue[i])) ; 
	            }
	        }
	        System.out.print(dp[2*n][n]) ; 
	    }
	}
```





##### 좋은 모범 로직: blue 카드를 선택할 수 있는 조건도 고려

````java
 for(int j = 0; j <= i; j++) {
                // Case 1
                // i번째 카드 쌍에서 빨간색 카드를 선택하여
                // 최종적으로 빨간색이 j개가 된 경우입니다.
                if(j > 0)
                    dp[i][j] = Math.max(dp[i][j], dp[i - 1][j - 1] + red[i]);

                // Case 2
                // i번째 카드 쌍에서 파란색 카드를 선택하여
                // 최종적으로 빨간색이 j개가 된 경우입니다.
                // 당연히 i - j가 0보다 커야지만이 만들어질 수 있는 경우입니다.
                if(i - j > 0)
                    dp[i][j] = Math.max(dp[i][j], dp[i - 1][j] + blue[i]);
            }
````

