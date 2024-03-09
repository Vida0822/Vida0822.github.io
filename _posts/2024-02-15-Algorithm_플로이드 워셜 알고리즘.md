---
published: true
title: "Shortest Path-플로이드 워셜 알고리즘" 
categories: Algorithm 
tag: [algorithm, Programmers, Shortest Path, FloydWarshall] 
toc: true
author_profile: false 
  
---



##### 플로이드 워셜 알고리즘?

모든 노드에서 다른 모든 노드로의 최단 경로를 구한다. 

* 최단거리 테이블 : 특정 노드에서 특정 노드로의 최단 거리  

* 다이나믹 프로그래밍 이용: **점화식**에 맞게 최단거리 테이블을 갱신 

<br>





##### 핵심 아이디어

A에서 B로 가는 최소 비용과 A에서 K를 거쳐 B로 가는 비을 비교하여 더 작은 값으로 갱신한다. 

 *점화식: DP(a,b) = min <-- DP(a,b) , DP(a, k)+DP(k,b)* 

<br>



##### Point 

> 2차원 리스트

행렬 형태 그래프를 사용하며, dp 배열을 또 다시 생성하지 않고 해당 그래프 자체를 갱신한다.  

즉 그 자체가 모든 노드에 대해 다른 모든 노드로 가는 최단 거리 정보.

<br>



##### 시간 복잡도 

> O(V^3)

2차원 리스트 갱신 : O(V^2)     **x**    모든 노드에 대해 반복 : O(V) 

<br>



##### 코드

```java 
import java.util.* ; 
public class FloydWarshall {
	public static final int INF = (int) 1e9 ; 
	public static int n, m ; 
	public static int[][] graph = new int[501][501] ; // 행렬형태 그래프
	
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in) ; 
		n = sc.nextInt() ; 
		m = sc.nextInt() ; 
		
		// 초기 조건 : 그래프 생성 (dp 초기조건)    
		for(int i = 0 ; i < 501; i++) {
			Arrays.fill(graph[i], INF); // 무한으로 채우고 
		}	
		for(int a = 1 ; a <= n ; a++) {
			for(int b = 1; b <= n ; b++) {
				if(a==b) graph[a][b] = 0;  // 자기자신으로는 갈 수 X 
			}
		}	
		for(int i = 0 ; i < m ; i++) { // 갈수 있는 곳은 거리 입력받음
			int a = sc.nextInt() ; 
			int b = sc.nextInt() ; 
			int c = sc.nextInt() ;
			graph[a][b] = c; 
		}
		
		// 점화식 (알고리즘 수행)
		for(int k = 1 ; k <= n ; k++) { // 각각의 노드에 대해 
			for(int i = 1 ; i <= n ; i++) {
				for(int j = 1 ; j <= n ; j++) {
					graph[i][j] = Math.min(graph[i][j], graph[i][k]+graph[k][j]) ; 
				}
			}
		} // for		
	} // main 
} // class 
```

<br> 

##### 다익스트라 vs 플로이드 

| 단순 Ver                              | 복잡 Ver                                    |
| ------------------------------------- | ------------------------------------------- |
| 한 노드에서 다른 노드들로의 최단 경로 | 모든 노드에서 다른 모든 노드로의  전체 경로 |
| 경로보다는 거리를 구하는데 사용       | 구체적인 경로도 특정할 수 있다              |

<br>



##### 구현(Python)

```python
INF = int(1e9)

n = int(input()) 
m = int(input()) 

graph = [[INF]*(n+1) for _ in range(n+1)]

# 자기자신에서 자기자신으로 가는 비용은 0으로 초기화 
for a in range(1, n+1):
    for b in range(1, n+1) : 
        if a == b : 
            graph[a][b] = 0 

# 각 간선에 대한 정보를 입력받아 초기화 
for _ in range(m):
    a, b, c = map(int, input().split()) 
    graph[a][b] = c 

# 점화식 (플로이드 워셜 알고리즘 수행)
for k in range(1, n+1):
    for a in range(1, n+1):
        for b in range(1, n+1):
            graph[a][b] = min(graph[a][b] , graph[a][k]+ graph[k][b])
```

