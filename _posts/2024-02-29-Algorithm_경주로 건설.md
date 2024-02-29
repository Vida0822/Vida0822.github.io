---
published: true
title: "BFS-경주로 건설" 
categories: Algorithm 
tag: [algorithm, Programmers, BFS,DP, Node, 3차원배열] 
toc: true
author_profile: false 
  
---



##### 문제

출발점인 (0, 0) 칸에서 출발한 자동차가 도착점인 (N-1, N-1) 칸까지 무사히 도달할 수 있게 중간에 끊기지 않도록 경주로를 건설. 

경주로는 상, 하, 좌, 우로 인접한 두 빈 칸을 연결하여 건설할 수 있으며, 벽이 있는 칸에는 경주로를 건설할 수 없습니다. 

`직선 도로` 하나를 만들 때는 100원이 소요되며, `코너`를 하나 만들 때는 500원이 추가될 때, 

경주로를 건설하는데 필요한 최소 비용을 return

<br>



##### 핵심 아이디어 

> BFS+  DB

* **BFS** : 최단의 경로로 목적지까지 이동하는 문제이므로 BFS를 이용한다

* **DP** : 특정 격자칸까지의 최소비용을 저장할 DP 배열이 필요하다 

  --> Board를 그대로 이용한다 

   

<br>



##### Point : 방향정보 

특이한 점은, 여러 경로로부터 동일한 칸에 도착했을 때 더 작은 값으로 해당 칸의 값을 갱신하게 되면, 

그 다음 움직임이 코너 움직임인지 직선 움직임인지에 따라서 다음 값이 역전될 수 있다.

ex) 1번 움직임과 **2**번 움직임이 각각 2600원과 2700원이라고 가정해보자. 

그렇다면 그 다음 움직임은 동일하게 우측으로 이동하는 움직임일 때, 더 작았던 **1**번 움직임의 2600원은 `2600+600=3200원`이 되지만 

**2**번 움직임은 `2700+100=2800원`이 된다. 즉, 

* 이동 방향에 따른 최소 비용을 각각 도출해줘야한다 
* 3차원 배열 또는 사용자 정의 클래스를 이용해 특정 노드까지의 '방향에 따른 비용'을 저장해줘야한다 
* 만약 최소 비용이 갱신되었다면, 해당 노드를 다시 검사노드로 설정해 인접 노드 최소비용 또한 다시 갱신해줘야한다.   

<br>





##### 코드 (구현)

```java
import java.util.* ; 
class Solution {
    
    int N;
    int[][][] visited;

    public int solution(int[][] board) {
        N = board.length;
        visited = new int[N][N][4]; // 방향도 고려
        return bfs(board);
    }

    public int bfs(int[][] board) {
        int cost = Integer.MAX_VALUE;

        Node node = new Node(0, 0, -1, 0);
        Queue<Node> queue = new LinkedList<>();
        queue.add(node);

        while (!queue.isEmpty()) {
            Node cur = queue.poll();
            
            // 최종 위치 도달 
            if (cur.y == N - 1 && cur.x == N - 1) {
                cost = Math.min(cost, cur.cost); // 4 방향중 작은값 반환 
            }

            // 인접 노드 방문 
            int[] dx = new int[]{0, 0, -1, 1};
            int[] dy = new int[]{-1, 1, 0, 0};
            
            for (int i = 0; i < 4; i++) { // i 자체가 방향값 나타냄 (상하좌우)
                int nx = cur.x + dx[i];
                int ny = cur.y + dy[i];

                // 격자 범위밖 또는 방문할 수 없는 노드 
                if (nx < 0 || nx >= N || ny < 0 || ny >= N || board[ny][nx] == 1) {
                    continue;
                }
                
                // 검사노드를 거쳐서 갈 때 각 인접노드의 방문비용 계산
                int nCost = cur.cost;
                if (cur.direction == -1 || cur.direction == i) { // 예외방향 또는 같은방향이면(검사노드의 direction==i) 
                    nCost += 100; 
                } else { // 다른 방향이면 
                    nCost += 600; // 코너 
                }
                
                // 최소비용갱신 및 재검사 setting  
                if (visited[ny][nx][i] == 0 || board[ny][nx] >= nCost) { 
                    // 해당 위치&방향 방문 안했거나, 재계산한 비용이 더 저렴한 경우
                    visited[ny][nx][i] = 1;  // 방문 처리
                    board[ny][nx] = nCost; // 비용 테이블 갱신 (board 그대로 이용)
                    queue.add(new Node(ny,nx,i,nCost)); // 해당노드 다시 검사대상 (그 인접노드들도 다시 갱신될 수 있음) 
                }
            }
        }
        return cost;
    }

    class Node {
        int x;
        int y; // 위치 
        int direction; // 방향  
        int cost; // 현재 노드까지 비용 

        Node(int y, int x, int direction, int cost) {
            this.y = y;
            this.x = x;
            this.direction = direction;
            this.cost = cost;
        }
    }
}
```

<br> 



##### 문제구분 - DFS vs BFS  

|                        DFS                         |                      BFS                       |
| :------------------------------------------------: | :--------------------------------------------: |
| 다음 분기로 넘어가기전에 해당 분기를 완벽하게 탐색 | 임의의 노드에 인접한 노드를 먼저 탐색하는 방법 |
|                  Stack, 재귀 함수                  |                     Queue                      |
|          경로의 전체의 내용, 특징을 저장           |                   최단 거리                    |

* 그래프의 모든 정점 방문 문제 --> 두 방법 모두 가능하다 

* BFS 가 최단거리 문제에서 유리한 이유 :  너비를 우선으로 탐색하므로 현재 노드에서 가까운 곳부터 찾기때문에, 

  경로 탐색시 먼저 찾아지는 답이 곧 최단거리)

<br> 

