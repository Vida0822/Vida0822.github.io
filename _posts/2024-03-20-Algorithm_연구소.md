---
published: true
title: "DFS&Implementation-연구소" 
categories: Algorithm 
tag: [algorithm, Programmers, DFS, Implementation] 
toc: true
author_profile: false 
  
---



##### 문제

일부 칸은 바이러스가 존재하며, 이 바이러스는 상하좌우로 인접한 빈 칸으로 모두 퍼져나갈 수 있다. 

새로 세울 수 있는 벽의 개수는 3개이며, 꼭 3개를 세워야 한다. 벽을 3개 세운 뒤, 바이러스가 퍼질 수 없는 곳을 안전 영역이라고 한다. 

연구소의 지도가 주어졌을 때 얻을 수 있는 안전 영역 크기의 최댓값을 구하는 프로그램을 작성하시오.

<br>



##### 로직 

1. 벽 3개 세우기
2. 바이러스 퍼트리기
3. 안전 영역 count 

<br>



##### 핵심 아이디어 1 : DFS

2번의 DFS 필요  

1. 가능한 모든 조합의 벽 3개 세우기 

2. 벽 다 세웠으면 바이러스 퍼트리기	 

<br> 

 

##### 핵심 아이디어 2: Implementation 

1. 그래프가 아닌 격자에서의 dfs 

​	연결된 노드로의 이동이 아닌 이동 가능한 인접 격자 칸으로 이동한다

2. 완전 탐색 : n의 값이 작기 때문에 가능하다. 

   벽 3개를 격자 전체 칸에 대하여 가능한 모든 조합에 세운다. 바이러스를 퍼트린 후 전체 격자를 순회하며 안전 영역을 계산한다. 

3. 현실의 아이디어(로직)를 코드로 구현하는 것이 까다로운 Simulation 문제다. 

<br>





##### 코드 

```java
import java.util.*;
public class 연구소 {
    public static int n, m, result = 0;
    
     public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        n = sc.nextInt();
        m = sc.nextInt();
        
        // 연구소 만들기 
        int[][] lab = new int[n][m] ; 
        for (int i = 0; i < n; i++) 
            for (int j = 0; j < m; j++) 
                lab[i][j] = sc.nextInt();
            
        // dfs 호출
        buildWalls(0, lab);
        System.out.println(result);
    }
    
    // 깊이 우선 탐색(DFS)을 이용해 울타리를 설치하면서, 매 번 안전 영역의 크기 계산
    public static void buildWalls(int count, int[][] lab) {
    	
        // 울타리가 3개 설치된 경우 (종료조건)
        if (count == 3) {       
            // 바이러스 퍼트릴 배열 복사 
        	int[][] virusLab = new int[n][m] ; 
            for (int i = 0; i < n; i++) 
                for (int j = 0; j < m; j++) 
                	virusLab[i][j] = lab[i][j];
                
            // 각 바이러스의 시작위치에서 전파 진행
            for (int i = 0; i < n; i++) 
                for (int j = 0; j < m; j++) 
                    if (virusLab[i][j] == 2)  
                        spreadVirus(i, j, virusLab);
                
            // 안전 영역의 최대값 갱신
            result = Math.max(result, safeAreas(virusLab));
            return;
        }
        
        // 빈 공간에 울타리를 설치 (완전탐색 -> 모든 조합의 벽건설마다 바이러스 전파시키기)
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (lab[i][j] != 0) 
                    continue ; 
                
                lab[i][j] = 1;
                count ++;
                buildWalls(count, lab);
                    
                lab[i][j] = 0;
                count --;
            }
        }
    } // dfs
    
    // 깊이 우선 탐색(DFS)을 이용해 각 바이러스가 사방으로 퍼지도록 하기
    public static void spreadVirus(int x, int y, int[][] virusLab) {
    	
    	// 4가지 이동 방향에 대한 배열
        int[] dx = {-1, 0, 1, 0};
        int[] dy = {0, 1, 0, -1};
        
        for (int i = 0; i < 4; i++) {
            int nx = x + dx[i];
            int ny = y + dy[i];
            // 상, 하, 좌, 우 중에서 바이러스가 퍼질 수 있는 경우
            if (nx < 0 || nx >= n || ny < 0 || ny >= m) 
                continue ; 
            if (virusLab[nx][ny] == 0) {
               // 해당 위치에 바이러스 배치하고, 다시 재귀적으로 수행
               virusLab[nx][ny] = 2;
               spreadVirus(nx, ny, virusLab);          
            }
        }
    } // virus
    
    // 현재 맵에서 안전 영역의 크기 계산하는 메서드
    public static int safeAreas(int[][] virusLab) {
        int count = 0;
        for (int i = 0; i < n; i++) 
            for (int j = 0; j < m; j++) 
                if (virusLab[i][j] == 0) 
                    count++ ;
      
        return count;
    } // getScore 
}
```

<br>

