---
published: true
title: "Shortest Path-간단한 다익스트라" 
categories: Algorithm 
tag: [algorithm, Programmers, Shortest Path, Dikstra] 
toc: true
author_profile: false 


---



##### 최단 경로

한 노드에서 특정 노드, 또는 모든 노드까지의 가장 짧은 경로

 (문제에선 '경로'보다는 단순히 최단 거리를 요구한다. )





##### 다익스트라 알고리즘?  

* 모든 노드로의 최단 경로를 구한다 (음의 간선 X)
* 그리디 알고리즘과 유사 : **매번**, **가장** 비용이 적은 노드를 선택해 임의의 과정 반복 





##### 핵심 아이디어 

방문하지 않는 노드 중 가장 최단거리가 짧은 노드를 선택하고, 그에 따른 최단거리 갱신 과정을 반복 



##### 로직 

1. 초기 설정 : 최단 거리 테이블 초기화 (무한)

2. 출발 노드 설정 : 인접한 노드들 최단 거리 갱신

3. 방문하지 않는 노드 중 최단 거리가 가장 짧은 노드 선택

4. 인접한 노드들 최단 거리 갱신

   ​		: 



##### Point 

한번 선택한 노드는 최단 거리가 감소하지 않는다 

: 한 단계당 하나의 노드에 대한 최단 거리를 확실히 찾음 





##### 단순 Ver 코드

구현하기 쉽지만 느리게 동작하는 코드 



>  시간 복잡도 : O(V^2)

매 단계마다 모든 노드를 확인해 최단 거리 노드 탐색 O(V) 

+해당 노드에 인접한 노드들 최단거리 비교/갱신 O(V)



```java 
import java.util.* ; 

class Node{
	private int index ; 
	private int distance ; 
	public Node(int index, int distance) {
		this.index = index;  // 노드 번호 
		this.distance = distance ;  // 간선 가중치 ; 이전 노드에서 현재 노드까지의 거리  
	}
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



