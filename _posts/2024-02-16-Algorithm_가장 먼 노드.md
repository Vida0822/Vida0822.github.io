---
published: true
title: "Shortest Path-가장 먼 노드" 
categories: Algorithm 
tag: [algorithm, Programmers, Shortest Path, Dikstra] 
toc: true
author_profile: false 
  
---



##### 문제

1번 노드에서 가장 멀리 떨어진 노드의 갯수를 구하려고 합니다. 

가장 멀리 떨어진 노드란 최단경로로 이동했을 때 간선의 개수가 가장 많은 노드들을 의미

<br>



##### 핵심 아이디어

다익스트라 이용 : 1번에서 갈 수 있는 모든 노드로의 최단 거리 --> 그 중 가장 큰 값과 일치하는 노드 갯수 구하기 

<br>



##### 코드

```java 
import java.util.*  ; 

class Solution {
    public static int INF = (int)1e9; 
    public static int n ; 
    public static int[] d ; 
    public static ArrayList<ArrayList<Node>> graph = new ArrayList<>() ; 
    
    public int solution(int n, int[][] edge) {
        this.n = n ; 
        this.d = new int[n + 1];
        
        // part 1. 그래프 만들기 
        for(int i = 0; i <=n ; i++){
            graph.add(new ArrayList<>()) ; 
        }
        for(int i = 0 ; i < edge.length ; i++){
            int from = edge[i][0] ; 
            int to = edge[i][1] ; 
            graph.get(from).add(new Node(to, 1)); // 수정된 부분
            graph.get(to).add(new Node(from, 1)); // 수정된 부분

        }
        Arrays.fill(d, INF); 
        
        // part 2. 다익스트라 수행 
        dikstra(1) ;
        
        // part 3. 최단거리 배열에서 가장 큰 값을 가진 노드들 반환        
        // 최댓값 구하기 
        int max = 0 ; 
        for(int i = 1 ; i <= n ; i++ ){
            if(d[i] < INF){
                System.out.print(d[i]+" ") ; 
                max = Math.max(max, d[i]) ; 
            }
        }
        // 최댓값과 일치하는 노드 갯수
        int answer = 0; 
        for(int i = 1 ; i <= n ; i++ ){
            if(d[i] == max){
                answer++; 
            }
        }
        return answer; 
    }
    
    public static void dikstra(int start){
        PriorityQueue<Node> pq = new PriorityQueue<>(); 
        pq.offer(new Node(start,0)) ; 
        d[start] = 0 ; 
        
        while(!pq.isEmpty()){
            Node node = pq.poll() ; 
            int now = node.index ; 
            int dist = node.distance ; 
            
            if(d[now] < dist)  // 1 
                continue ; 
            
            for(Node nextNode:graph.get(now)){
                int cost = d[now] + nextNode.distance ; 
                if(cost < d[nextNode.index]){
                    d[nextNode.index] = cost ; 
                    pq.offer(new Node(nextNode.index, cost)) ; 
                } // if 
            } // for 
        } // while 
    }
}
```

<br> 

