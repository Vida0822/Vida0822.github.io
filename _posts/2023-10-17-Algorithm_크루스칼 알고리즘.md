---
published: true
title: "Graph-크루스칼 알고리즘" 
categories: Algorithm 
tag: [algorithm, Kruskal, MST, Greedy] 
toc: true
author_profile: false 


---



##### 크루스칼 알고리즘이란? 

그래프 내의 모든 정점들을 가장 적은 비용으로 연결하기 위해 사용.

* 모든 정점을 포함하고 사이클이 없는 연결 선을 그렸을 때, 가중치의 합이 최소가 되는 상황을 구하는 알고리즘 
* 즉, 최소 신장 트리를 구하기 위한 알고리즘

=> 노드는 고려하지 않고 간선만 가중치 기준 오름차순으로 고려한다

<br>



##### 최소 신장 트리(Minimum Spanning Tree)

1) 그래프에서 모든 정점을 포함하고, 
2) 정점 간 서로 연결이 되며,
3) 사이클이 존재하지 않는 그래프 

=> 정점의 갯수가 n개일 때, 간선이 n-1개가 된다 

=> 이런 신장 트리 중 가중치의 합이 최소가 되는 신장 트리 : **최소 신장 트리 **

<br>



##### 알고리즘 로직 

> "그리디 알고리즘의 일종"

그래프 간선들을 가중치의 오름차순으로 정렬해 놓은 뒤,

'사이클을 형성하지 않는 선에서' 정렬된 순서대로 간선을 선택.

선택된 간선의 갯수가 정점의 갯수 -1만큼 되면 종료 

<br>





##### 사이클 판단 : Union & Find

> Union & Find

: **Dijoint Set(서로소 집합)**을 표현하는 자료구조로, 서로 다른 두 집합을 병합하는 **Union연산** 

 & 집합 원소가 어떤 집합에 속해있는지 찾는 **Find 연산**

* **parent 배열** : 각 정점의 root node(부모)를 표현한 배열
* **초기 상태**: 노드들은 각각 서로소 집합, 자기 자신이 루트 노드가 되게 초기화 되어있는 상태 (parent[i] = i) 
* **경로 압축** : 부모 배열에 루트 노드의 번호를 저장해 부모를 타고 올라가는 시간을 단축

<br>

1. 가중치의 오름차순 정렬 순서대로 간선 1-2를 선택 
2. 1번과 2번은 같은 집합 :**2번의 부모는 1번**(작은 index 노드를 부모로)
3. 다음으로 작은 간선 선택 : **"Greedy**(매 반복마다 최소의 것을 선택)"
4. 연결하러보니 두 노드의 루트노드가 같음 --> **Union 연산 X**

<br>







##### 코드(구현)

```java
import java.util.* ; 

public class 크루스칼알고리즘 {
	public static void union(int[] parent, int a, int b) {
		int a_parent = find(parent, a) ; 
		int b_parent = find(parent, b) ;
		
		if(a_parent < b_parent)
			parent[b_parent] = a_parent ; 
		else
			parent[a_parent] = b_parent ; 
	} // union 
	
	public static int find(int[] parent , int i) {
		if (parent[i] == i) 
            return i;
        return parent[i] = find(parent, parent[i]);
	} // find 
	
	public static void main(String[] args) {
		int[][] graph ; 
		int[] parent = new int[6] ; 
		int total = 0; // 최소 신장 트리의 가중치 총합
		
		// part1. 초기상태
		for(int i = 0 ; i < parent.length ; i++) {
			parent[i] = i ; 
		}
		
		// part 2. 가중치 기준으로 정렬 
		Arrays.sort(graph, new Comparator<int[]>() { // int 배열을 정렬하는 기준을 넣어줌
			@Override
			public int compare(int[] o1, int[] o2) {
				return o1[2]-o2[2] ;  // o1이 더 작으면 음수 --> o1이 앞선다 (오름차순) 
			}
		});
		
		// part 3. 최소 신장 비용 구하기 
		for(int i = 0 ; i < graph.length;  i++) {
			if(find(parent, graph[i][0]) != find(parent, graph[i][1])) {
				total += graph[i][2] ; 
				union(parent, graph[i][0], graph[i][1]) ; 
			}
		}
		System.out.println("최소 비용 신장 트리 가중치의 합은" + total);
	} // main  
} // class 
```

<br>



##### 경로 압축

```java
public static int find(int[] parent , int i) {
	if (parent[i] == i) 
        return i;
    return find(parent, parent[i]);
} // find 
```



> 왜 그냥 parent[i]가 아니지? 

재귀 : 원래 'Union Find'의 기본 코드 (부모를 타고 올라가며 루트를 찾는다) 

--> 여기에 특별히 '경로 압축'을 넣어준 것 

: 경로상의 각 노드의 부모를 루트로 갱신하여 경로의 길이를 줄인다 

--> 재귀함수를 사용하여 노드 i의 부모를 찾고, 동시에 해당 노드의 부모를 루트로 갱신 

--> But '부모를 찾는 재귀적 로직'은 유지한다 (비록 한번에 root 노드값이 나오더라도)  



