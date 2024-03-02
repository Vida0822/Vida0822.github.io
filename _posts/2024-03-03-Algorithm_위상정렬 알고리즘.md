---
published: true
title: "Graph-위상정렬(Topology)" 
categories: Algorithm 
tag: [algorithm, Programmers, Graph, Sort] 
toc: true
author_profile: false 
  
---



##### 위상정렬 

순서가 정해져 있는 일련의 작업을 차례대로 정렬해야할 때 사용 

방향 그래프의 모든 노드를 선후관계에 맞게 전체 순서를 반환

<br>



##### 진입차수 

특정 노드로 '들어오는' 간선의 갯수 

<br>



##### 로직

1. 진입 차수가 0인 노드를 큐에 넣는다 
2. 큐가 빌 때까지 다음의 과정을 반복한다 

* 큐에서 원소를 꺼내 해당 노드에서 출발하는 간선을 그래프에서 제거 
* 새롭게 진입차수가 0이된 노드를 큐에 넣는다 

3. 큐에서 빠져나간 노드 정보를 순서대로 출력

<br> 

##### Point  

1. Cycle : 만약 모든 원소를 방문하기 전 큐가 빈다면 사이클이 존재함을 의미 

​	: 사이클이 존재하는경우 진입 차수가 1이상이 되서 어떠한 원소도 큐에 들어가지 못한다

2. 정렬 순서는 여러가지가 될 수 있다 

   : 큐에 새롭게 들어가는 원소(진입차수:0)가 2개 이상인 경우 

<br>



##### 시간 복잡도 

> O(V+E)

* 차례대로 모든 노드를 확인하면서  - O(V)
* 해당 노드에서 출발하는 간선을 차례대로 제거 - O(E)

<br>



##### 코드 (Java)

```java
import java.util.* ; 

public class TopologySort {
	
	// 노드의 개수는 최대 100,000개라고 가정
    public static int v, e;
    // 모든 노드에 대한 진입차수는 0으로 초기화
    public static int[] indegree = new int[100001];
    
    // 리스트형 그래프 (각 노드로부터 연결된 노드들)
    public static ArrayList<ArrayList<Integer>> graph = new ArrayList<ArrayList<Integer>>(); 
    
    // 위상 정렬 함수 ** 
    public static void topologySort() {
    	ArrayList<Integer> result = new ArrayList<>(); 
    	Queue<Integer> q = new LinkedList<>(); 
    	
    	for(int i = 1 ; i <= v ; i++) {
    		if(indegree[i] == 0)
    			q.offer(i);
    	}
    	
    	while(!q.isEmpty()) { // 노드들 하나씩 확인 - O(V) 
    		int now = q.poll() ; 
    		result.add(now) ;
    		
    		// 해당 원소와 연결된 노드들의 진입차수에서 1 빼기
    		for(int i = 0 ; i < graph.get(now).size() ; i++) { // 간선들 하나씩 확인 - O(E)
    			indegree[graph.get(now).get(i)] -= 1 ;
    			
    			if(indegree[graph.get(now).get(i)] == 0)
    				q.offer(graph.get(now).get(i)) ; 
    		}
    	}
    	for(int i = 0 ; i < result.size() ; i++) {
    		System.out.print(result.get(i)+ " ");
    	}
    }

	public static void main(String[] args) {
		
		// 그래프 만들기 
		Scanner sc = new Scanner(System.in) ; 
		v = sc.nextInt() ; 
		e = sc.nextInt() ; 
		
		for(int i = 0 ; i <= v ; i++) {
			graph.add(new ArrayList<Integer>()) ; 
		}
		
		for(int i = 0 ; i < e ; i++) {
			int a = sc.nextInt() ; 
			int b = sc.nextInt() ; 
			graph.get(a).add(b); // 정점 A에서 B로 이동 가능 
			indegree[b] += 1 ; // 진입 차수 1증가 	
		}
		topologySort() ; 
	}
}
```

<br>



##### 코드 (Python)

```java
from collections import deque

v, e = map(int, input().split())

indegree = [0]*(v+1)

graph = [[] for i in range(v+1)]

for _ in range(e):
    a, b = map(int, input().split())
    graph[a].append(b) 
    indegree[b] += 1

def topology_sort():
    result = []
    q = deque() 

    for i in range(1,v+1) : #노드들 검사 
        if indegree[i] == 0:
            q.append(i)
    
    while q: # 큐가 비지 않았으면 (존재하면)
        now = q.popleft()
        result.append(now)

        for i in graph[now] :  # 해당 노드와 연결된 노드들 하나씩 가져와서 
            indegree[i] -= 1
            if indegree[i] == 0 : 
                q.append(i)
            
        for i in result : 
            print(i, end = ' ')

topology_sort()
```

<br> 

