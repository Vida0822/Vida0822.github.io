---
published: true
title: "Shortest Path-BFS로 최단경로 구하기" 
categories: Algorithm 
tag: [algorithm, Programmers, Shortest Path, BFS] 
toc: true
author_profile: false 
  
---



##### 핵심 아이디어 : BFS  

> 시간 복잡도 : O(N+M)

모든 간선의 비용이 동일할 때는 너비 우선 탐색을 이용해 간단히 구할 수 있다 

* 우선순위 큐 X : 모든 간선의 비용이 1이므로, 최소 비용 간선을 골라내는 작업이 불필요하다 (그냥 인접되는 순서대로 꺼냄)

* 기존 dp값과 대소 비교 불필요 : BFS의 원리가 가까운 노드부터 방문이므로 차례로 접하는 인접노드가 곧 x 로부터 최단 거리다.

  => 방문한적 없는지만 체크한다  

* 즉, 해당 조건하에선 A노드에서 B노드로의 최소 간선 수를 counting한것이 최단거리이다 ! 

<br>





##### 문제 

특정 도시 x에서 다른 도시로의 최단거리가 k인 도시를 출력하시오 

<br>





##### 코드 (Java)

```java
import java.util.* ; 

public class 특정거리의도시찾기 {
	public static void main(String[] args) {
		
		Scanner sc = new Scanner(System.in) ;
		int n = sc.nextInt() ; 
		int m = sc.nextInt() ;
		int k = sc.nextInt() ; 
		int x = sc.nextInt() ;
		
		// 그래프 만들기 
		ArrayList<ArrayList<Integer>> graph = new ArrayList<>() ;
		for(int i = 0 ; i <= n ; i++) {
			graph.add(new ArrayList<>()) ; 
		}
		
		for(int i = 0 ; i < m ; i++) {
			int a = sc.nextInt() ;
			int b = sc.nextInt() ; 
			graph.get(a).add(b) ; 
		}
		
		// x로부터 최단거리 갱신 
		int[] dp = new int[n+1] ; 
		Arrays.fill(dp, -1);
		
		Queue<Integer> q = new LinkedList<Integer>();
		q.offer(x) ; 
		dp[x] = 0 ;
		
		while(!q.isEmpty()) {
			int cur = q.poll() ; 
			for(int i = 0 ; i < graph.get(cur).size(); i++) {
				int next = graph.get(cur).get(i) ; 

				//if(dp[cur] + 1 < dp[next]) {
				if(dp[cur] == -1) { // 아직 방문하지 않은 도시라면
					dp[next] = dp[cur] + 1 ; // 최단 거리 갱신 
					q.offer(next); 
				}		
			}
		}
		
		// 답 출력
		for(int i = 1 ; i <= n ; i++) {
			if(k == dp[i])
				System.out.println(i);
		}
	}
}
```

<br>





