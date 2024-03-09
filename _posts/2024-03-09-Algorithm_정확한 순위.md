---
published: true
title: "Shortest Path-정확한 순위" 
categories: Algorithm 
tag: [algorithm, Programmers, Shortest Path, Floyd-Warshall] 
toc: true
author_profile: false 
  
---



##### 문제 

N명의 학생 간 성적을 비교한 결과 중 일부만 가지고 있다. 

A번 학생이 성적이 B번 학생보다 낮다면, 

화살표가 A에서 B를 가리키도록 할 때 성적 순위를 알 수 있는 학생은 모두 몇 명? 

<br>





##### 핵심 아이디어 

* 화살표로 가리킨다는 점에서 성적 비교 결과는 '방향 그래프'로 볼 수 있다. 

* 핵심은, A에서 B로 도달이 가능하거나 B에서 A로 도달 가능하면 '성적비교 가능'이라는 점이다. 

* 성적 순위를 알 수 있으려면 다른 모든 학생과의 성적 비교가 가능해야한다. 
* 즉, 각 노드를 차례로 검사하며 다른 모든 노드와의 연결된 경로가 있는지 확인한다 

<br>



##### 로직

> 최단경로 : Floyd-Warshall 알고리즘 

 Floyd-Warshall 알고리즘을 사용해 모든 노드들간의 최단 경로를 구하고, 

각 노드에서 다른 노드로, 또는 다른 노드에서 해당 노드로의 경로 존재 여부를 확인하고 

모든 노드에 대해 경로가 존재하면 결과++ 

<br>



##### 시간 복잡도 

> O(500^3)

N의 크기가 500으로 작으므로, O(N^3)인 플로이드 워셜 알고리즘을 사용해도 된다. 

<br>





##### 코드 (Java)

```java
package shortestPath;

import java.util.Arrays;
import java.util.Scanner;

public class 정확한순위 {

	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in) ; 
		int n = sc.nextInt() ; 
		int m = sc.nextInt() ;

		// 1. 방향 그래프로 간주 
		// 2. 한 노드에서 다른 노드 각각으로 올 수 있는, 
		// 	또는 갈 수 있는 경로가 다른 노드들 모두에 존재한다면, 해당 학생은 순위를 알 수 있다 

		int[][] graph = new int[n+1][n+1] ;

		// 그래프 만들기 
		for(int i = 1 ; i <= n ; i++) {
			Arrays.fill(graph, 100000);
		}
		for(int i = 0 ; i < m ; i++) {
			int a = sc.nextInt() ; 
			int b = sc.nextInt() ; 
			graph[a][b] = 1 ; // 경로 존재 
		}

		
		// 알고리즘 수행 (점화식) 
		for(int k = 1 ; k <= n ; k++) { 
			for(int i = 1 ; i <= n ; i++) { 
				for(int j = 1 ; j <= n ; j++) {
					graph[i][j] = Math.min(graph[i][j], graph[i][k]+graph[k][j]) ; 
				}
			}
		}

		// answer 도출 
		int answer = 0 ;
		for(int i = 1 ; i <= n ; i++) {
			int count = 0 ;
			for(int j = 1 ; j <= n ; j++) {
				if(graph[i][j] != 100000 || graph[j][i] != 100000) 
					count ++ ; 
			}
			if(count == n) {
				answer++ ; 
			}
		}
		System.out.println(answer);
	}
}

```

<br>





