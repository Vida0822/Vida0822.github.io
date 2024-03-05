---
published: true
title: "DP-금광" 
categories: Algorithm 
tag: [algorithm, Programmers, DP] 
toc: true
author_profile: false 
  
---



##### 문제 

n행 m열의 금광이 있다. 

첫번째 열부터 출발해 금을 캐기 시작하며, m번에 걸쳐 매번 오른쪽 위, 오른쪽 아래, 오른쪽, 총 3가지 중 하나의 위치로 이동해야한다. 

결과적으로 채굴자가 얻을 수 있는 금의 최대 크기를 출력하시오 

<br>



##### 핵심 아이디어 

> DP

2차원 배열 각 자리까지의 최대 금 채굴량을 dp 배열에 저장한다. 

이 때, 채굴자는 이전칸의 왼쪽 위, 왼쪽, 왼쪽 아래에서만 이동 가능하므로 세가지 경우를 비교한 최대값

+현재 위치의 금 채굴량을 현재 위치의 dp값으로 설정한다 

즉, 점화식 dp[i] [j] = max(dp[i-1 ~ i+1]) + dp[i] [j]

<br>



##### Points

* 바깥 테두리를 감싸고 0으로 초기화 해 인덱스가 범위 밖으로 벗어나는 경우를 따로 고려하지 않는다 

  즉, n+2 x m+2 크기의 배열을 사용한다 

* 이동 가능 경우수는 3가지로 고정되어있으므로, 반복문을 사용하지 않고 하드코딩 비교한다 (3중 반복문 방지)

* gold 배열을 따로 만들지 않고 dp 배열을 초기화 해 사용한다 

  : 한번 갱신된 값 이전의 초기 값을 다시 조회할 경우가 없을 경우, 메모리 절약을 위해 dp배열을 그대로 사용한다. 



##### 코드 (Java)

```java
import java.util.Scanner;

public class 금광 {
	
	public static void dp() {
		Scanner sc = new Scanner(System.in) ; 
		
		int n = sc.nextInt() ; 
		int m = sc.nextInt() ; 
		int[][] gold = new int[n+2][m+2] ; // 둘레를 한칸씩 비워두고 0으로 초기화하여 index 오류 방지 
		int[][] dp = new int[n+2][m+2] ; 
		
		// 금광 만들기 
		for(int i = 1 ; i <= n ; i++) {
			for(int j = 1 ; j <= m ; j++) {
				gold[i][j] = sc.nextInt() ; 
			}
		}
		
		// dp 수행 - 점화식 : dp[i][j] = dp[i-1 ~ i+1][j-1] + gold[i][j] 
		for(int j = 1 ; j <= m ; j++) {
			for(int i = 1 ; i <= n ; i++) {
				int max = 0 ; 
				max = Math.max( Math.max(dp[i-1][j-1], dp[i][j-1]), dp[i+1][j-1]) ; 
				/*for(int k = i-1 ; k <= i+1 ; k++) {
					max = Math.max(max, dp[k][j-1]) ; 
				}*/
				dp[i][j] = max + gold[i][j] ; 
			}
		}
        
		// 최종 답 출력 
		int answer = 0 ; 
		for(int i = 1 ; i <= n ; i++) {
			answer = Math.max(answer, dp[i][m]) ;
		}
		System.out.println(answer);
	}

	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in) ; 
		int T = sc.nextInt() ; 

		for(int i = 1 ; i <= T ; i++) {
			dp() ; 
		}
	} 
}
```

<br>



##### 코드(Python)

```python
for ic in range(int(input())):  # 테스트 케이스 입력 받은만큼 반복 
    n , m = map(int, input().split())
    array = list(map(int, input().split())) # n*m개 금 리스트 + 초기화 

    # n행 m열 2차원 배열 
    dp = [] 
    index = 0 
    for i in range(n) : # 총 n개 요소로 구성 -> 2차원 배열 
        dp.append(array[index:index+m]) # m개씩 잘라서 (m열) -> 1차원 배열 
        index += m 

    # dp 실행 
    for j in range(1, m):  # 두번째 행부터 
        for i in range(n) : 

            # 맨 윗행 초기화 하는 경우 
            if i == 0 : 
                left_up = 0 
            else :
                left_up = dp[i-1][j-1] 

            # 맨 아랫행 초기화 하는 경우 
            if i == n-1 : 
                left_down = 0 
            else : 
                left_down = dp[i+1][j-1]
            
            left = dp[i][j-1] 

            # 점화식
            dp[i][j] = dp[i][j] + max(left_up, left, left_down) 

    result = 0 
    for i in range(n):
        result = max(result, dp[i][m-1])
    print(result) 
```



 

