---
published: true
title: "Greedy-프림 알고리즘" 
categories: Algorithm 
tag: [algorithm, Prim, MST, Greedy] 
toc: true
author_profile: false 

---



##### 프림 알고리즘이란? 

최소 신장 트리를 구하기 위한 알고리즘.

특정 노드의 이어진 간선들 중 최소 가중치인 것을 반복적으로 선택한다.

즉, 크루스칼과 달리 간선을 정렬하지 않고 하나의 정점에서 시작해 트리를 확장해 나가는 알고리즘

<br>





##### 알고리즘 로직 

> "그리디 알고리즘의 일종"

1. 시작 정점 선택
1. 정점에 부속된 모든 간선 중 가중치가 가장 작은 간선 연결(트리 확장)
1. 해당 집합에 부속된 간선 중 최소 가중치 간선 선택 (이때, 사이클을 형성하지 않는 간선이어야한다)
1. 그래프에 n-1개의 간선을 삽입할 때까지 반복

<br>



##### 우선순위 큐 사용

* List형 그래프 사용 
* 방문 여부 배열 사용 
* 특정 노드에 인접한 노드들을 우선순위 큐에 삽입하고, 가중치가 가장 작은 애를 뽑아줌으로써 해당 노드로의 집합 확대

<br>



##### 코드(구현)

```java
import java.util.* ; 

public class 프림알고리즘 {
	public class Node_Prim implements Comparable<Node_Prim>{
		int index ; 
		int cost ; 
		
		public Node_Prim(int index, int cost) {
			this.index = index ; 
			this.cost = cost ; 
		}
		@Override
		public int compareTo(Node_Prim other) {
			return this.cost - other.cost; 
		}	
	}
	
	public List<List<Node_Prim>> graph = new ArrayList<>() ; 
	
	public int solution(int n, int[][] costs) {
		
		// 초기화 (그래프 만들기) 
		for(int i = 0 ; i < n ; i++) {
			graph.add(new ArrayList<>()) ; 
		}
		for(int i = 0 ; i < costs.length ; i++) {
			int from = costs[i][0] ; 
			int to = costs[i][1] ; 
			int val = costs[i][2] ; 
			
			graph.get(from).add(new Node_Prim(to, val)); 
			graph.get(to).add(new Node_Prim(to, val)) ; 
		}
		
		// 프림 알고리즘 
		boolean[] visited = new boolean[n] ; // 각 노드만큼 크기의 방문여부배열
		PriorityQueue<Node_Prim> q = new PriorityQueue<>() ; 
		q.add(new Node_Prim(0,0)) ; // 검사 시작 노드 
		
		int answer = 0 ; 
		while(!q.isEmpty()) {
			Node_Prim cur = q.poll() ; // 가중치가 가장 작은 노드 반환 
			if(visited[cur.index]) continue ; 
			
			visited[cur.index] = true ; 
			answer += cur.cost ;  
			// a 노드에 부속된 간선 중 가능 작은 값이므로 answer에 누적 
			// b 노드로 확대 
			
			// 인접 노드 큐에 추가  
			for(int i = 0 ; i < graph.get(cur.index).size(); i++) {
				int next = graph.get(cur.index).get(i).index ; 
				int cost = graph.get(cur.index).get(i).cost ; 
				if(visited[next]) continue ; // 해당 인접 노드는 큐에 추가 X 
				q.add(new Node_Prim(next, cost)) ; 
			} // for 
		} // while
		return answer ; 
	} 
}

```

<br>



