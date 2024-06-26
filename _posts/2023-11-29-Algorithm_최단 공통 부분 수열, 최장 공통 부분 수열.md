---
published: true
title: "DP-최단공통부분수열, 최장공통부분수열"
categories: Algorithm 
tag: [algorithm, CodeTree, dp, 점화식 , 최단공통부분수열, 최장공통부분수열 ] 
toc: true
author_profile: false 
---



### 1. 최단 공통 부분 수열

: 두 문자열을 모두 부분 문자열로 하는 문자열 중 길이가 가장 짧은 문자열  



**구하는 방법** 

* 공통되는 부분을 뽑아내어 한번만 표시하고
* 각 문자열의 공통되지 않는 부분을 적절한 위치에 덧붙인다 



**동등한 상황** 

* A 문자열 중 지금까지 고려한 문자의 위치  ex) 5번째 문자까지 고려함 
* B 문자열 중 지금까지 고려한 문자의 위치 ex) 8번째 문자까지 고려함 
* 지금까지 고려된 공통 supersequence의 길이

=> 두 개의 공통 supersequence 가 있을 때 더 나은 상황 

: A 문자열에서의 위치와 B 문자열에서의 위치가 같다면, 지금까지 고려된 '두 부분 문자열의 공통 supersequence의 길이'는 짧을 수록 좋다 



**dp[i] [j]**

: A 문자열의 i번째 문자까지 고려하고, B 문자열의 j번째 문자까지 고려했을 때 가능한 공통 supersequence의 최단 길이 



**점화식** 

1. A 문자열의 i번째 문자와, B 문자열의 j번째 문자가 일치하는 경우

: A 문자열에서 i-1번째 문자까지 고려하고, B 문자열의 j-1번째 문자까지 고려했을 때 가능한 최대 공통 supersequence 뒤에 문자 하나를 새로 추가 

```java
DP[i][j] = DP[i-1][j-1] + 1   // if A[i] == B[j]
```

​	

2.  A 문자열의 i번째 문자와, B 문자열의 j번째 문자가 일치하지 않는 경우 

* A 문자열의 i-1번째 문자와 B문자열의 j번째 문자까지의 공통 최단 부분 수열과 

* A 문자열의 i 번째 문자와 B 문자열의 j-1번째 문자까지 공통 부분 수열의 최소값 중   
* 작은 값에 포함되지 않던 새로운 문자(i번째 문자 or j번째 문자) 를 더한다 (늘어난 개수인 1을 더함)  

```java
DP[i][j] = min(DP[i-1][j] , dp[i][j-1])  + 1  // if A[i] != B[j]
```



**초기조건**

* dp[1] [1] : 두 문자가 일치하는 경우엔 1, 아니면 2 
* dp[1] [j] (1행)  

 : A 문자열의 첫 번째 문자와 B 문자열의 j번째 문자까지 고려했을 때 가능한 supersequence의 최단길이 

```java
DP[1][j] = j 				// if A[1] == B[j] 
    	|| DP[1][j-1] + 1 	// if A[1] != B[j]
```

(만약 A문자열 첫 번째 문자가 B문자열과 일치하는 게 없으면 죽죽 누적되어 B문자열 길이보다 1많아짐, 하나라도 겹치는게 있으면 최종적으로 B문자열 길이만큼 수열 만들어짐!) 



* dp[i] [1] (1열) 

: A문자열의 i번째 문자와 B문자열의 첫 번째 문자까지 고려했을 때 가능한 supersequence의 최단길이 

```java
DP[i][1] = j 				// if A[1] == B[j] 
    	|| DP[1][j-1] + 1 	// if A[1] != B[j]
```



**answer**

: 첫 번째 문자열의 끝까지 고려하고, 두 번째 문자열의 끝까지 고려했을 때 가능한 공통 supersequence의 최단 길이 

```java
DP[len_A][len_B]
```



**좋은 모범 로직**

````java
public class SuperSubSequence {

	public static final int lenA = 4 , lenB = 3 ; 

	public static String A = " ABAB" ;  // 0 ~ 5 (0은 빈 문자열 )  
	public static String B = " BBA" ;  // 0 ~ 4  (0은 빈 문자열) 
	public static int[][] dp = new int[lenA + 1][lenB + 1 ]; 

	public static void initialize() {
		dp[1][1] = (A.charAt(1) == B.charAt(1))? 1: 2 ; 

		// 1행 
		for(int i = 2 ; i <= lenA ; i++) {
			if(A.charAt(i) == B.charAt(1))  
                dp[i][1] = i ; 
			else 	
                dp[i][1] = dp[i-1][1] + 1 ; 	
		}
		// 1열 
		for(int j = 2 ; j <= lenB ; j++) {
			if(A.charAt(1) == B.charAt(j)) 
				dp[1][j] = j ; 
			else 
				dp[1][j] = dp[1][j-1] + 1 ; 
		}
	} // initialize
	
	public static void main(String[] args) {
		
		initialize(); 
		
		for(int i = 2 ; i <= lenA ; i++) {
			for(int j = 2 ; j <= lenB ; j++) {
				if(A.charAt(i) == B.charAt(j))
					dp[i][j] = dp[i-1][j-1] + 1 ; 
				else 
					dp[i][j] = Math.min(dp[i-1][j], dp[i][j-1]) + 1  ; 
			} // j 
		} // i 
		System.out.println(dp[lenA][lenB]) ; 
	} // main 
} // class 
````





### 2. 최장 공통 부분 수열 

: 두 문자열 중 공통적인 부분만 뽑아 만든 부분 수열 중 최대로 길이가 긴 것 



**구하는 방법 **

각 문자열의 공통되는 부분만 뽑아내어 표시한다 



**동등한 상황**

* A 문자열 중 지금까지 고려한 위치  ex) 5번째 문자 
* B 문자열 중 지금까지 고려한 위치  ex) 8번째 문자 
* 지금까지 고려한 공통 부분 수열의 길이 

=> 더 나은 상황 : A 문자열에서의 위치와 B 문자열에서의 고려한 위치가 같다면, 지금까지 고려된 '두 문자열의 공통 부분 수열의 길이'가 가장 긴 것이 좋다 



**dp[i] [j]** 

: A 문자열에서 i번째 문자까지 고려했으며, B 문자열에서 j 번째 문자까지 고려했을 때 ! 

지금까지 고려된 공통 부분 수열 길이의 최댓값  



**점화식**

1. A 문자열의 i번째 문자와, B 문자열의 j번째 문자가 일치하는 경우

: A 문자열에서 i-1번째 문자까지 고려하고, B 문자열의 j-1번째 문자까지 고려했을 때 가능한 최대 공통 common subsequence 뒤에 문자 하나를 새로 추가 

```java
DP[i][j] = DP[i-1][j-1] + 1   // if A[i] == B[j]
```

​	

2. A 문자열의 i번째 문자와, B 문자열의 j 번째 문자가 일치하지 않는경우 

* A 문자열의 i-1번째 문자와 B문자열의 j번째 문자까지의 공통 최장 부분 수열의 최댓값과 (dp[i-1] [j]) 

* A 문자열의 i 번째 문자와 B 문자열의 j-1번째 문자까지 공통 최장 부분 수열의 최댓값 중 (dp[i] [j-1])   
* 더 큰 값 (1을 더하지 않음 : 공통된 문자만 더해야하니까 일치하지 않는 문자는 더하지 않음! )

```java
DP[i][j] = max(DP[i-1][j] , DP[i][j-1] ) 
```



**초기 조건**

* dp[1] [1] : 두 문자가 일치하면 1 , 일치하지 않으면 0 

* 1행 (dp[1] [j]) 

  : A 문자열의 첫번째 문자와 B 문자열의 j번째 문자까지 고려했을 때 가능한 공통 부분 수열의 최장 길이 

  ```java
  DP[1][j] = 1  			// if A[1] == B[j] 
      	|| DP[1][j-1] 	// if A[1] != B[j]
  ```



* 1열 (dp[i] [j]) 

  : A 문자열의 i번째 문자까지와 B 문자열의 첫번째 문자를 고려했을 때 가능한 공통 부분 수열의 최장 길이

  ```java
  DP[i][1] = 1  			// if A[i] == B[1] 
      	|| DP[i-1][1] 	// if A[i] != B[1]
  ```

  ​	

**answer**

: 첫 번째 문자열의 끝까지 고려하고, 두 번째 문자열의 끝까지 고려했을 때 가능한 공통 common subsequence의 최장 길이 

```java
DP[len_A][len_B]
```



**내 풀이 ✔**

```java
import java.util.* ; 

public class LongestCommonSubsequence {

    public static int MAX_LEN = 1000 ; 
    public static String strA ; 
    public static String strB ; 
    public static int lenA, lenB ; 
    public static int[][] dp = new int[MAX_LEN+1][MAX_LEN+1];// 1001 

    public static void initialize(){
        /*
        for(int i = 1 ; i <= lenA ; i++ ){
            for(int j = 1 ; i <= lenB ; j++){
                dp[i][j] = Integer.MIN_VALUE ; 
            }
        } */
        dp[1][1] = (strA.charAt(1) == strB.charAt(1))? 1: 0 ; 

        // 1열 
        for(int i = 2 ; i <= lenA ; i++){
            if(strA.charAt(i) == strB.charAt(1)){
                dp[i][1] = 1 ; 
            }else{
                dp[i][1] = dp[i-1][1] ; 
            }  
        }
        // 1행
        for(int j = 2 ; j <= lenB ; j++){
            if(strA.charAt(1) == strB.charAt(j)){
                dp[1][j] = 1 ; 
            }else{
                dp[1][j] = dp[1][j-1] ; 
            }  
        }
    } // initialize

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in) ; 
        strA = "#"+ sc.next() ; 
        strB = "#"+ sc.next() ; 
        lenA = strA.length() - 1 ; 
        lenB = strB.length() - 1 ;  
    
        initialize() ; 

        for(int i = 2 ; i <= lenA; i++){
            for(int j = 2 ; j <= lenB; j++){
                if(strA.charAt(i) == strB.charAt(j)){
                    dp[i][j] = dp[i-1][j-1] + 1 ; 
                }else{
                    dp[i][j] = Math.max(dp[i-1][j] , dp[i][j-1]) ; 
                }
            } // j 
        } // i 
        System.out.println(dp[lenA][lenB]) ; 
    } // main
} // class
```

