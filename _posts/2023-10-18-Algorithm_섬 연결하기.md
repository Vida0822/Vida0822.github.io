---
published: true
title: "Greedy-섬 연결하기" 
categories: Algorithm 
tag: [algorithm, Kruskal, MST, Greedy] 
toc: true
author_profile: false 

---



##### 최소 신장 트리(Minimum Spanning Tree)

1) 그래프에서 모든 정점을 포함하고, 
2) 정점 간 서로 연결이 되며,
3) 사이클이 존재하지 않는 그래프 

=> 정점의 갯수가 n개일 때, 간선이 n-1개가 된다 

=> 이런 신장 트리 중 가중치의 합이 최소가 되는 신장 트리 : **최소 신장 트리 **

<br>





##### 1) 크루스칼 알고리즘 

```java
import java.util.* ; 
class Solution {
    
    public int[] parent ; 
    public int find(int i){
        if(parent[i] == i)
            return i ; 
        else return parent[i] = find(parent[i]) ; 
    } 
    
    public void union(int a, int b){
        int parent_a = find(a) ; // a의 부모 
        int parent_b = find(b) ; // b의 부모 
        
        // 1 1 1 5 5 
        // 1 2 3 4 5 
        if(parent_a != parent_b)
            // parent[b] = parent_a ; --> b가 속한 집합의 루트를 a의 부모로 설정 (b원소를 a로 이동 ; a와 b는 다른 집합)
            parent[parent_b]  // parent[find(b)] b의 루트
            = parent_a ; 
        // b가 속한 집합의 루트 자체를 a로 변경  
    }
    
    public int solution(int n, int[][] costs) {
        
        // part 1. 초기 상태
        parent = new int[n] ; 
        for(int i = 0 ; i < n ; i++){
            parent[i] = i ; 
        }
        
        // part 2. 가중치 기준으로 정렬 
        Arrays.sort(costs,  (o1, o2) -> o1[2] - o2[2]) ; 
        
        // part 3. 최소 신장 비용 구하기 
        int answer = 0 ; 
        for(int i = 0 ; i < costs.length ; i++){
            if(find(costs[i][0]) != find(costs[i][1])){
                union(costs[i][0], costs[i][1]) ; 
                answer += costs[i][2] ; 
            }
        }
        return answer ; 
    } // solution 
} // class 
```

<br>



##### ※ 루트 노드 설정

```java
 public void union(int a, int b){
        int parent_a = find(a) ; // a의 부모 
        int parent_b = find(b) ; // b의 부모 
        
       
        if(parent_a != parent_b)
            // parent[b] = parent_a ; 
            parent[parent_b] = parent_a ; 
    }
```

* parent[b] = parent_a : b가 속한 집합의 루트를 a의 부모로 설정 (b원소를 a로 이동 ; a와 b는 다른 집합)

**반례**

 parent:  1 1 1 5 5 
 nodes:   1 2 3 4 5 

* parent[parent_b] = parent_a  : b가 속한 집합의 루트 자체를 a로 변경

<br>





##### 2) 프림 알고리즘

```java
import java.util.* ; 
public class 프림알고리즘 {

	public class Node implements Comparable<Node>{
		int index ; 
		int cost ; 
		
		public Node(int index, int cost) {
			this.index = index ; 
			this.cost = cost ; 
		}
		@Override
		public int compareTo(Node other) {
			return this.cost - other.cost; 
		}	
	}
	
	public List<List<Node>> graph = new ArrayList<>() ; 
	
	public int solution(int n, int[][] costs) {
		
		// 초기화 (그래프 만들기) 
		for(int i = 0 ; i < n ; i++) {
			graph.add(new ArrayList<>()) ; 
		}
		for(int i = 0 ; i < costs.length ; i++) {
			int from = costs[i][0] ; 
			int to = costs[i][1] ; 
			int val = costs[i][2] ; 
			
			graph.get(from).add(new Node(to, val)); 
			graph.get(to).add(new Node(to, val)) ; 
		}
		
		// 프림 알고리즘 
		boolean[] visited = new boolean[n] ; // 각 노드만큼 크기의 방문여부배열
		PriorityQueue<Node> q = new PriorityQueue<>() ; 
		q.add(new Node(0,0)) ; // 검사 시작 노드 
		
		int answer = 0 ; 
		while(!q.isEmpty()) {
			Node cur = q.poll() ; // 가중치가 가장 작은 노드 반환 
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
				q.add(new Node(next, cost)) ; 
			} // for 
		} // while
		return answer ; 
	} 
}
```

