---
published: true
title: "Shortest Path-다익스트라 알고리즘" 
categories: Algorithm 
tag: [algorithm, Programmers, Shortest Path, Dikstra] 
toc: true
author_profile: false 


---



##### 최단 경로

한 노드에서 특정 노드, 또는 모든 노드까지의 가장 짧은 경로.

문제에선 '경로'보다는 단순히 최단 거리를 요구한다. 



<br>



##### 다익스트라 알고리즘?  

모든 노드로의 최단 경로를 구한다 (음의 간선 X). 

그리디 알고리즘과 유사 : **매번**, **가장** 비용이 적은 노드를 선택해 임의의 과정 반복 . 

<br>





##### 핵심 아이디어 

방문하지 않는 노드 중 가장 최단거리가 짧은 노드를 선택하고, 해당 노드를 거쳐가는 경로를 확인하여,  

그에 따른 최단거리 테이블 갱신 과정을 반복 

<br>



##### 로직 

1. 초기 설정 : 최단 거리 테이블 초기화 (무한)

2. 출발 노드 설정 : 인접한 노드들 최단 거리 갱신

3. 방문하지 않는 노드 중 최단 거리가 가장 짧은 노드 선택

4. 인접한 노드들 최단 거리 갱신

   ​		: 

<br>



##### Point 

한번 선택한 노드는 최단 거리가 감소하지 않는다 

> 한 단계당 하나의 노드에 대한 최단 거리를 확실히 찾음 

<br>



##### 단순 Ver 구현

구현하기 쉽지만 느리게 동작하는 코드 



>  시간 복잡도 : O(V^2)

선형 탐색 : 매 단계마다 모든 노드를 확인해 최단 거리 노드 탐색 O(V) 

➕ 그래프 탐색 : 해당 노드에 인접한 노드들 최단거리 비교/갱신 O(V)

<br>

*코드*

```java 
import java.util.* ; 

class Node{
	private int index ; // 노드 번호 
	private int distance ; // 특정 노드에서 해당 노드까지의 거리 
} // Node 

public class Dikstra_Simple { // O(V^2) , 전체 노드 갯수 5000개 이하 
	// 무한 (10억)
	public static final int INF = (int) 1e9 ;   
	// 노드갯수, 간선갯수, 시작노드번호 
	public static int n , m , start ; 
	// 그래프 ; 각 노드에 연결되어 있는 노드에 대한 정보를 담는 배열 
	public static ArrayList<ArrayList<Node>> graph = new ArrayList<ArrayList<Node>>() ; 
	// 방문 여부 배열 
	public static boolean[] visited = new boolean[100001] ; 
	// 최단 거리 테이블 (계속 갱신됨) 
	public static int[] d = new int[100001] ; 
	
	// 방문하지 않는 노드 중, 가장 최단거리가 짧은 노드의 번호를 반환 - O(V) 
	public static int getSmallestNode() { 
		int min_value = INF; 
		int index = 0 ; // 가장 최단거리가 짧은 노드(인덱스) 
		
		for(int i = 1 ; i <= n ; i++ ) {
			if(d[i] < min_value && !visited[i]) {
				min_value = d[i]; 
				index = i ; 
			}
		}
		return index ; 
	} // getSmallestNode

	// 최단 경로 구하기 (전체 과정) 
	public static void dijkstra(int start) {
		// 시작 노드에 대해서 초기화 
		d[start] = 0 ; 
		visited[start] = true; 	
		// 시작 노드에 인접한 노드들 최단 거리 갱신  
		for(int j = 0 ; j < graph.get(start).size() ; j++) { 
			d[graph.get(start).get(j).getIndex()] 
					= graph.get(start).get(j).getDistance() ; 
		}
		for(int i = 0 ; i < n-1 ; i++) {
			int now = getSmallestNode(); // 최단거리가 가장 짧은 노드를 꺼내서 
			visited[now] = true ; // 방문 처리 
			
			// 현재 노드와 연결된 다른 노드를 확인 
			for(int j = 0 ; j <graph.get(now).size() ; j++) {
				int cost = d[now] // 현재 노드까지의 최단 거리 
						+ graph.get(now).get(j).getDistance(); // 현재 노드에서 해당 노드까지의 거리
				if(cost < d[graph.get(now).get(j).getDistance()]) {
					// 현재 노드를 거치는게 기존 최단 거리보다 더 짧다면
					d[graph.get(now).get(j).getIndex()] = cost ; // 최단거리 갱신
				}
			}		
		}	
	}
} // class 
```

<br> <br>



**복잡 ' Ver 구현**

구현하기 더 까다롭지만 빠르게 동작하는 코드 



> 시간 복잡도 O(N log N)

**우선순위 큐**를 사용해 가장 거리가 짧은 노드를 O(log N) 시간 복잡도로 찾아낸다 ! 

이때, 검사 노드에 '인접했던' 노드들만 저장하는 목적으로 큐 사용

=> **O(E log E)**  : 한번 realease 된 간선은 다시 검사하지 않는다 

<br>



```java
class Node implements Comparable<Node>{
	private int index ; 
	private int distance ; 

	@Override
	public int compareTo(Node other) {
		// 음수가 나오면 this가 우선 
		if(this.distance < other.distance) {
			return -1 ; 
		}
		return 1 ; 
	}
} // Node 

public class Dikstra_Queue {    
	public static void dijkstra(int start) {
		PriorityQueue<Node> pq = new PriorityQueue<>() ; 
		pq.offer(new Node(start,0)) ; // 시작 노드로 가기 위한 최단 경로는 0 
		d[start] = 0 ; 
		
		while(!pq.isEmpty()) {
			Node node = pq.poll() ; // 가장 짧은 최단거리 노드 꺼내기 
			int dist = node.getDistance() ; 
			int now= node.getIndex() ; 
			
			// 현재 노드가 이미 처리된 적이 있는 노드라면 무시 (이미 realease된 간선)
			if(d[now] < dist) continue ; 
			
			for(int i = 0 ; i < graph.get(now).size() ; i++) {
				int cost = d[now] + graph.get(now).get(i).getDistance() ; // 인접 노드들 방문 
				
				// 현재 노드를 거치는 거리가 더 짧은 경우 
				if(cost < d[graph.get(now).get(i).getIndex()]) {
					// 최단 거리 갱신 
					d[graph.get(now).get(i).getIndex()] = cost ; 
					// 해당 인접 노드 큐에 추가 (왜 더 짧아야 추가?? ) 
					pq.offer(new Node(graph.get(now).get(i).getIndex(), cost)); 
				} // if 
			} // for 
		} // while
	} // dijkstra
} // class
```



<br>



##### 단순 Ver vs 복잡 Ver 

| 단순 Ver                                      | 복잡 Ver                                                     |
| --------------------------------------------- | ------------------------------------------------------------ |
| 1. 전체 노드 중 가장 작은 최단 거리 탐색      | 1. 큐에서 최단거리 노드 추출                                 |
| 2. 해당 노드 기준으로 인접한 노드 realease    | 2. 해당 노드 기준으로 인접한 노드 realease <br />=> 더 짧은 경로로 갱신된 인접 노드만 큐에 넣어줌 |
| 3. 다시 전체 노드 중 가장 작은 최단 거리 탐색 | 3. 큐에 담긴 인접했던 노드들 중 최단 거리 노드 추출          |

