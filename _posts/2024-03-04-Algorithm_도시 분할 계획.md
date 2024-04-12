---
published: true
title: "Kruscal-도시 분할 계획" 
categories: Algorithm 
tag: [algorithm, Programmers, Graph, Sort] 
toc: true
author_profile: false 
  
---



##### 문제 

n개의 집과 m개의 길로 이루어진 마을을 2개로 분리하려고 한다. 

각 분리된 마을 안의 집들은 서로 연결되도록 분할해야한다. 

분리하면서 분리된 두 마을 사이에 있는 길들은 필요가 없으니 없앨 수 있으며 , 

분리된 마을 안에서도 임의의 두 집 사이 경로가 항상 존재하게 되면서 길을 더 없앨 수 있다. 

길을 없애고 남은 유지비 합의 최솟값은?  

<br>



##### 핵심 아이디어 

> 크루스칼 알고리즘

전체 마을을 최소 비용으로 잇는 최소 비용신장트리를 구하고, 

그중 가장 가중치가 큰 간선 하나를 없애 마을을 분리한다. 

그렇게 분리된 각각의 마을은 최소 비용신장트리 형태를 띈다

<br>





##### 코드 (Java)

```java
package graph;

import java.util.*;

public class 도시분할계획 {
	
	public static int[] parent  = new int[100001]; 
	public static ArrayList<Edge> edges = new ArrayList<>() ; 
	public static int n, m ;
	public static int result = 0 ; 
	
	public static int find(int x) { // (int x , int[] parent) 
		if(x == parent[x]) return x ;
		return parent[x] = find(parent[x]) ;
		// 이부분 문제 : 담아놓고 반환하지 않음 
		// static이 아닌 그냥 전달받은 매개변수를 변화시키는 것 --> 실제로 호출해준 곳의 parent 배열엔 아무 영향 주지 x 
		// ㄴㄴ 전달할 때 배열(참조형)을 전달하기 때문에 그 매개변수를 갱신하면 자동으로 해당 주소의 배열이 변경됨 
 	}
	
	public static void union(int x, int y) {
		int x_root = find(x) ; 
		int y_root = find(y) ; 
		/* 
		if(x_root < y_root)
			parent[y] = x_root ; 
		else
			parent[x] = y_root ; 
		ㄴ 이렇게 하면 안되는 이유 : union연산은 각 집합의 '루트노드'의 '부모'를 다른쪽 루트노드로 설정해주는 것 
		=> 이렇게 하면 그냥 x,y의 부모가 루트노드로 됨 (해당 노드의 '이동'인셈) -->  
		*/
		if(x_root < y_root)
			parent[y_root] = x_root ; 
		else
			parent[x_root] = y_root ; 
	}
	
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in) ; 
		
		n = sc.nextInt() ; 
		m = sc.nextInt() ; 
		
		// 부모 테이블 초기화 
		for(int i = 1 ; i <= n ; i++) {
			parent[i] = i ; 
		}
		
		// 그래프 만들기 
		for(int i = 0 ; i < m ; i++) {
			int a = sc.nextInt() ;
			int b = sc.nextInt() ; 
			int c = sc.nextInt() ;
			edges.add(new Edge(c, a, b)) ;		
		}
		// 간선 가중치 오름차순 정렬 
		Collections.sort(edges);
		
		// 크루스칼 알고리즘 : 최소비용신장트리 만들기 
		int last = 0 ; 
		for(int i = 0 ; i < edges.size(); i++) { 
			// 마지막 간선은 더하지 않는다(분리) 
			// --> x : 얘는 분리가 아님, 마지막으로 검사한 간선이 꼭 사용되는 간선이라고 볼 수 없기 때문
			// => 최소신장비용트리를 먼저 만들고, 구성 간선 중 가장 큰 걸 빼줘야함
			int cost = edges.get(i).getDistance() ; 
			int nodeA = edges.get(i).getNodeA() ; 
			int nodeB = edges.get(i).getNodeB() ; 
			
			if(find(nodeA) != find(nodeB)) {
				union(nodeA, nodeB) ; 
				result += cost ; 
				last = cost ;  
			}
		}
		System.out.println(result-last);
	}
}

```

<br>



##### 코드 (Python)

```java
def find_parent(parent, x) : 
    if parent[x] != x :
        parent[x] = find_parent(parent, parent[x])
    return parent[x] 

def union_parent(parent, a, b):
    a = find_parent(parent, a) 
    b = find_parent(parent, b)

    if a < b : 
        parent[b] = a 
    else :
        parent[a] = b 

v, e = map(int, input().split())
parent = [0]*(v+1)

edges = [] 
result = 0 

for i in range(1, v+1) : 
    parent[i] = i 

for _ in range(e) : 
    a, b, cost = map(int, input().split())
    # 비용순으로 정렬하기 위해 튜플의 첫번째 원소를 비용으로 설정 
    edges.append((cost,a,b))

edges.sort()
last = 0 

for edge in edges : 
    cost, a, b = edge 

    if find_parent(parent, a) != find_parent(parent,b):
        union_parent(parent, a, b)
        result += cost 
        last = cost 
print(result-last)
```

<br> 

