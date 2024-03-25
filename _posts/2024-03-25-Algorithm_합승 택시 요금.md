---
published: true
title: "Shortest Path-합승 택시 요금" 
categories: Algorithm 
tag: [algorithm, Programmers, Shortest Path, Dijkstra, FloydWarshall] 
toc: false
author_profile: false 
  
---



##### 문제

[코딩테스트 연습 - 합승 택시 요금 | 프로그래머스 스쿨 (programmers.co.kr)](https://school.programmers.co.kr/learn/courses/30/lessons/72413)

<br>

 

##### 핵심 아이디어

> 세 번의 최단 경로 도출

s -> 임의의 x 지점까지 합승한다고 할 때, (s->x까지 최단거리) + (x->a까지의 최단거리) + (x->b까지의 최단거리)가 전체 최단 거리가 될 것이다. 

x 지점에 1~N까지 대입해 봤을 때, 최솟값이 answer이 된다. (이때 시작점 노드도 고려하므로 합승을 안하는 경우도 함께 다뤄진다)

모든 경로는 방향이 없으므로, x -> a 는 a -> x와 동일하다. 따라서 s, a, b를 시작점으로 모든 경로까지의 최단 경로를 각각 구한 뒤, 세 배열의 1 ~ N 값을 넣으며 더해 가장 최소가 되는 값을 return 한다. 

<br>



##### 로직 

1. 그래프를 만든다 
1. 3가지 출발점을 기준으로 최단거리 정보를 갱신한다
1. 교차점 노드를 1 ~ N 으로 설정해 그 교차점까지의 합이 가장 작은 경우를 return 한다/   

<br> 

##### 코드

```java
import java.util.* ; 
class Node implements Comparable<Node>{
    int index ; // 도착 노드 
    int cost ; // 비용 
    
    public Node(int index, int cost){
        this.index = index ;
        this.cost = cost ; 
    }
    
    @Override
    public int compareTo(Node other){
        return this.cost - other.cost ; 
    }
}

class Solution {
    public static ArrayList<ArrayList<Node>> graph ; 
    
    public int solution(int n, int s, int a, int b, int[][] fares) {
        // 최단거리 --> 가중치가 있는 양방향 그래프 
        // 그래프 만들기 
        graph = new ArrayList<>() ; 
        for(int i = 0 ; i <= n ; i++){
            graph.add(new ArrayList<>()) ; 
        }
        for(int i = 0 ; i < fares.length ; i++){
            graph.get(fares[i][0]).add(new Node(fares[i][1], fares[i][2])) ; 
            graph.get(fares[i][1]).add(new Node(fares[i][0], fares[i][2])) ; 
        }
        
        int[] startA = dijkstra(a, n) ; 
        int[] startB = dijkstra(b, n) ; 
        int[] start = dijkstra(s, n) ; 
    
        int answer = 200000001; 
        for(int i = 1 ; i <= n ; i++){
            answer = Math.min(answer , 
                             start[i] + startA[i] + startB[i]) ; 
        }
        return answer ; 
    }
    
    public int[] dijkstra(int start, int n){
        int[] d = new int[n+1] ;  // d[i] : start -> i 까지의 거리 
        Arrays.fill(d, 20000001) ; 
        PriorityQueue<Node> pq = new PriorityQueue<>() ; 
        pq.offer(new Node(start, 0)); 
        d[start] = 0 ; 
        
        while(!pq.isEmpty()){
            Node now = pq.poll() ; 
            
            for(Node next : graph.get(now.index)){
                int cost = d[now.index]+next.cost; 
                if(cost < d[next.index]){
                    d[next.index] = cost ; 
                    pq.offer(new Node(next.index, cost)) ; 
                }
            }
        }
        return d; 
    }
}
```

<br>

 

##### 플로이드 워셜

같은 아이디어를 활용하되, 플로이드 워셜 표에서 더욱 직접적으로 계산한다. 

```java
import java.util.Arrays;

class Solution {
    static final int MAX = 20000001; // 200 * 100000 + 1
    public int solution(int n, int s, int a, int b, int[][] fares) {
        
        // 모든 노드 -> 모든 노드로의 최단거리 구하기 
        int[][] dp = new int[n + 1][n + 1];
        for (int i = 0; i <= n; i++) {
            Arrays.fill(dp[i], MAX);
            dp[i][i] = 0;
        }

        for(int i = 0; i < fares.length; i++) {
            int start = fares[i][0];
            int end = fares[i][1];
            int cost = fares[i][2];

            dp[start][end] = cost;
            dp[end][start] = cost;
        }

        for(int k = 1; k < n+1; k++) {
            for(int i = 1; i < n+1; i++) {
                for(int j = 1; j < n+1; j++) {
                    dp[i][j] = Math.min(dp[i][j], dp[i][k] + dp[k][j]);
                }
            }
        }
		int answer = dp[s][a] + dp[s][b];
        
		// 경유지점을 끼는경우 고려 
        // (s -> x 최단거리) + (x -> a 최단거리) + (t -> b 최단거리) 에서 x가 1~n일때의 최댓값
        for(int x = 1; x <= n; x++) {
            answer = Math.min(answer 
                              , dp[s][x] + dp[x][a] +dp[x][b]); 
            					// 경유 노드가 1번 ~ N번인 경우 
        }
        return answer;
    }
}
```





















