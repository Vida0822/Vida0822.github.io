---
published: true
title: "Topological-커리큘럼" 
categories: Algorithm 
tag: [algorithm, Programmers, Graph, Sort] 
toc: true
author_profile: false 
  
---



##### 문제 

총 n개의 강의를 듣고자 하는데 각 과목은 선수과목이 있다. 

각 줄에는 강의 시간, 선수과목들이 입력되며, 동시에 여러개의 강의를 들을 수 있다고 가정한다. 

n개의 강의에 대해 수강하기 까지 걸리는 최소시간을 각각 출력 

<br>



##### 핵심 아이디어 

> 위상정렬 

* 방향 그래프 
* 그래프를 이루는 각 노드의 자기까지 오는데 걸리는 비용을 출력 
* 즉 모든 노드를 화살표 순서대로 정렬한 후  노드 하나씩 점검 

<br>





##### 코드 (Java)

```java
import java.util.* ; 

public class 커리큘럼 {

	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in) ; 
		int n = sc.nextInt() ; 
		
		ArrayList<ArrayList<Integer>> graph = new ArrayList<>() ; 
		int[] indegree = new int[n] ; 
		int[] times = new int[n] ; 
		
		// 그래프 만들기 
		for(int i = 0 ; i < n ; i++) {
			graph.add(new ArrayList<Integer>()) ; 
		}
		for(int i = 0 ; i < n ; i++) {
			times[i] = sc.nextInt() ; 
			int x = 0  ; 
			while( (x = sc.nextInt()) != -1 ) {
				graph.get(x-1).add(i) ; // 여기가 문제 : 지정하는 수업 번호는 자연수기 때문에 0인덱스에서 맞춰주려면 -1 해줘야함  
				indegree[i] += 1; 
			}
		}
		
		// 차수 0인것부터 큐에 넣기 
		Queue<Integer> q = new LinkedList<>() ; 
		for(int i = 0 ; i < indegree.length ; i++) {
			if(indegree[i] == 0)
				q.offer(i) ; 
		}
		
		// 큐에서 하나씩 빼면서 위상정렬 
		int[] result = Arrays.copyOf(times , n) ;
		
		while(!q.isEmpty()) {
			int lesson = q.poll() ;
	
			for(int i = 0 ; i < graph.get(lesson).size() ; i++) {
				int nextClass = graph.get(lesson).get(i) ; 
				
                // '인접 노드들의' 소요시간 갱신 
				result[nextClass] = Math.max(result[nextClass], result[lesson]+times[nextClass]) ; 
				// 이유 : 매번 해당 강의를 듣는데 필요한 시간 갱신, 더 큰값이 나왔다는 건 선수과목이 더 있다는 얘기
                // 강의를 동시에 수강할 수 있으므로 단순히 누적하는게 아닌 기존 값과 비교해 큰값으로 설정 
				indegree[nextClass] -= 1 ; 
				if(indegree[nextClass] == 0) {
					q.offer(nextClass) ; 
				}
			}
		}
		Arrays.stream(result).forEach(System.out::println);
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

