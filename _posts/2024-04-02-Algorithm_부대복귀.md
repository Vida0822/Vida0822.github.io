---
published: true
title: "Shortest Path-부대 복귀" 
categories: Algorithm 
tag: [algorithm,Shortest Path,BFS, Queue] 
toc: true
author_profile: false 
  
---



##### 문제

https://school.programmers.co.kr/learn/courses/30/lessons/132266

<br>



##### 핵심 아이디어

> '효율적인' BFS

문제 자체는 전형적인 BFS이지만, n과 roads 배열의 길이가 커 그래프가 광범위하게 형성된다. 

따라서 전형적인 BFS 풀이로 풀어 그래프를 순회하면 시간 제한을 통과하지 못한다. 

즉, 시간을 단축할 수 있는 효율적인 bfs 트릭이 필요하다. 

<br> 

##### ❌ 실패 코드 (시간초과)

```java
import java.util.*; 
class Node{
    int index ; 
    int dist ; 
    
    public Node(int index, int dist){
        this.index = index; 
        this.dist = dist;
    }
}

class Solution {
    public static ArrayList<ArrayList<Integer>> graph ; 
    public static int n ; 
    
    public int[] solution(int n, int[][] roads, int[] sources, int destination) {
        // 최단거리 --> bfs 
        this.n = n ; 
        
        // 그래프 만들기 
        graph = new ArrayList<>() ; 
        for(int i = 0 ; i <= n ; i++){ // O(100000)
            graph.add(new ArrayList<>()) ; 
        }
        
        for(int i = 0 ; i < roads.length ; i++){ // O(500000)
            graph.get(roads[i][0]).add(roads[i][1]) ; 
            graph.get(roads[i][1]).add(roads[i][0]) ; 
        }
        
        // bfs 
        int[] answer = new int[sources.length] ; 
        for(int i = 0 ; i < sources.length ; i++){
            answer[i] = bfs(sources[i], destination) ; 
        }
        return answer ; 
    }
    
    
    // 특정 출발지에서 destination까지의 최단거리 반환 
    public int bfs(int source, int destination){
        boolean[] visited = new boolean[n+1]; 
        
        Queue<Node> q = new LinkedList<>() ; 
        q.offer(new Node(source,0)) ;
        
        while(!q.isEmpty()){
            Node start = q.poll() ; 
            int s = start.index ; 
            int d = start.dist; 
            
            if(s == destination) 
                return d ; 
            visited[s] = true ; 
                        
            ArrayList<Integer> linked = graph.get(s) ; 
            for(int i = 0 ; i < linked.size() ; i++){
                int e = linked.get(i) ; 
                if(visited[e])
                    continue ;
                q.offer(new Node(e, d+1)) ; 
            }
        }
        return -1 ; 
    }
}
```

<br>



##### ✔ 시간 복잡도 개선 

> 다익스트라 

특정 출발지에서 도착지로의 최단 거리를 매번 구하지 않고(모든 source의 원소마다 재 bfs 수행) 

다른 모든 노드에서 한 목적지로의 전체 최단거리를 구하는데, 양방향 그래프이기 때문에 하나의 출발지에서 여러 목적지로의 전체 최단거리를 구하는 문제와 같다(도착지 --> 출발지)

이렇게 도착지에서 전체 노드로의 최단거리를 전부 구한 후, sources[i]에서 명시한 노드의 최단 거리를 반환한다. 

이렇게 하면 다익스트라 시간복잡도인 O(N log N)으로 문제를 해결할 수 있다. 

<br>



1. **양방향 그래프이기 때문에 단순 BFS(가까운 노드부터 방문)을 시행해도 해당 노드가 가장 비용이 적은 노드이다. **

```java
 private static void dijkstra(int departure) {
        Queue<Integer> q = new LinkedList<>() ; 
        // 가중치가 없을땐 우선순위 큐와 Node 클래스를 따로 정의해줄 필요 없이 정수값(1)씩 누적한다
        q.add(departure); 
        dist[departure] = 0 ; 
        
        while(!q.isEmpty()){
            int cur = q.poll() ; 
            
            for(int i = 0 ; i < graph.get(cur).size() ;i++){
                int next = graph.get(cur).get(i) ; 
                if(dist[next] > dist[cur] + 1){
                    dist[next] = dist[cur]+1 ; 
                    q.add(next) ; 
                }
            } // for
        } // while
    } // dijkstra
```

즉 가중치를 비교자로 둔 우선순위 큐나 노드 클래스를 만들필요 없이 Queue에서 들어온 순으로 꺼내주고, Node 클래스도 별도로 만들필요 없이 현재 dist 값에 1을 누적한다   

<br>



2. **도착지(destination)를 출발지 (departure)로 바꾸어 전체 노드로의 최단 거리를 갱신한다. **

> 다익스트라, 플로이드 워셜은 발상의 역전 문제가 많이 나옴 
>
> 유사한 문제 : 합승 택시 요금 

```java
 public static int[] solution(int n, int[][] roads, int[] sources, int destination) {
		//... //
        dijkstra(destination); // destinatin : 도착지 --> 출발지

        int[] answer = new int[sources.length];
        for (int i = 0; i < sources.length; i++) { // sources : 출발지들 -> 도착지들 
            answer[i] = (dist[sources[i]] < MAX) ? dist[sources[i]] : -1;
        }
        return answer;
    }

    private static void dijkstra(int departure) { // 출발지 --> 다른 모든 노드로의 최단거리 
       // ... // 
    } // dijkstra
}
```

<br>



##### 전체 코드

```java
import java.util.*;

class Solution {
    private static List<List<Integer>> graph;
    private static int[] dist;
    private static final int MAX = 1_000_000_000;

    public static int[] solution(int n, int[][] roads, int[] sources, int destination) {

        graph = new ArrayList<>();
        for(int i=0; i<n+1; i++) graph.add(new ArrayList<>());

        for (int[] road : roads) {
            int s = road[0];
            int e = road[1];

            graph.get(s).add(e);
            graph.get(e).add(s);
        }

        dist = new int[n+1];
        Arrays.fill(dist, MAX);
        dijkstra(destination);

        int[] answer = new int[sources.length];
        for (int i = 0; i < sources.length; i++) {
            answer[i] = (dist[sources[i]] < MAX) ? dist[sources[i]] : -1;
        }
        return answer;
    }

    private static void dijkstra(int departure) {
        Queue<Integer> q = new LinkedList<>() ; 
        // 가중치가 없을땐 우선순위 큐와 Node 클래스를 따로 정의해줄 필요 없이 정수값(1)씩 누적한다
        q.add(departure); 
        dist[departure] = 0 ; 
        
        while(!q.isEmpty()){
            int cur = q.poll() ; 
            
            for(int i = 0 ; i < graph.get(cur).size() ;i++){
                int next = graph.get(cur).get(i) ; 
                if(dist[next] > dist[cur] + 1){
                    dist[next] = dist[cur]+1 ; 
                    q.add(next) ; 
                }
            } // for
        } // while
    } // dijkstra
}
```

<br>

