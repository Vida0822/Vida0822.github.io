---
published: true
title: "BFS-경쟁적 전염" 
categories: Algorithm 
tag: [algorithm, Programmers, BFS, Queue] 
toc: true
author_profile: false 
  
---



##### 문제

시험관에 존재하는 모든 바이러스는 1초마다 상, 하, 좌, 우의 방향으로 증식해 나간다. 

단, 매 초마다 번호가 낮은 종류의 바이러스부터 먼저 증식한다. 

또한 증식 과정에서 특정한 칸에 이미 어떠한 바이러스가 존재한다면, 그 곳에는 다른 바이러스가 들어갈 수 없다.

시험관의 크기와 바이러스의 위치 정보가 주어졌을 때, *S*초가 지난 후에 (X,Y)에 존재하는 바이러스의 종류를 출력하는 프로그램을 작성하시오.

<br>



##### 핵심 아이디어 : BFS

> 1번 바이러스 상하좌우 -> 2번 바이러스 상하좌우 -> 3번 바이러스 상하좌우 

각각의 바이러스 위치에서 번갈아가며 증식하므로 각 바이러스에 인접한 노드들을 같이 큐에 넣어주고   

BFS를 통해 차례로 방문하며 graph 갱신해줘야

다음 시간대의 올바른 증식을 할 수 있다. 	 

<br> 

##### Point : Virus 객체 

>암기: dfs, bfs에서의 시간 계산 : *각 격자칸까지의 초수 정보*를 객체에 담아 함께 넘겨준다 

다음 노드 검사 시 넘겨줘야 할 정보 중 좌표 뿐 아니라 바이러스 번호, 현재까지 걸린 시간 정보를 함께 전달해야하므로

Virus 객체를 선언하여 함께 넘겨준다

즉 Virus 객체에는 바이러스 번호, 현재 격자까지 전파되는데 걸린 시간, 위치 정보(x , y)이다. 

<br>



##### 코드 

```java
import java.util.* ; 

class Virus implements Comparable<Virus>{
	int index;  
	int second ; 
	int x; 
	int y; 
	
	Virus(int index, int second, int x, int y){
		this.index = index ;
		this.second = second ; 
		this.x = x ; 
		this.y = y ; 
	}
	@Override 
	public int compareTo(Virus other) {
		return this.index - other.index ; 
	}
}

public class 경쟁적전염 {
	public static int n , k ; 
	public static int[][] graph = new int[200][200] ; 
	
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in) ; 
		n = sc.nextInt() ; 
		k = sc.nextInt() ; 
		
		// 그래프 만들기 & 검사할 바이러스 삽입 
		ArrayList<Virus> viruses = new ArrayList<>() ; 
		for(int i = 0 ; i < n ; i++) {
			for(int j = 0 ; j < n ; j++) {
				graph[i][j] = sc.nextInt(); 
	
				// 전염 시작점 바이러스들 모아놓고 (0초) 
				if(graph[i][j] != 0)
					viruses.add(new Virus(graph[i][j], 0 , i, j)); 
			}
		}
        // 낮은 번호 바이러스부터 번갈아 가며 검사 
		Collections.sort(viruses) ; 
		
        // s초 지난후의 증식상황 반영
		int s = sc.nextInt() ; 
		bfs(viruses,s) ; 

        // 답 출력 (x, y의 바이러스)
		int x = sc.nextInt() ; 
		int y = sc.nextInt() ; 
		System.out.println(graph[x-1][y-1]) ; 
	}
	
	public static void bfs(ArrayList<Virus> viruses, int s) {
		
		// 큐 만들기 (전염 시작 바이러스들 정렬)  : 1 , 2, 3
		Queue<Virus> q = new LinkedList<>() ; 
		for(int i = 0 ; i < viruses.size() ; i++) {
			q.offer(viruses.get(i)) ; 
		}
		
		while(!q.isEmpty()) { // 1 상하좌우 -> 2 상하좌우-> 3 상하좌우-> 1 -> 2 -> 3 -> ... 
			Virus virus = q.poll() ; 
			
            // 종료조건
			if(virus.second == s) 
				break ; 
			
			int[] dx = {-1, 1, 0, 0} ; 
			int[] dy = {0, 0, -1, 1} ;
			for(int i = 0 ; i < 4 ; i++) { // 1초마다 상하좌우로 증식 
				int nx = virus.x + dx[i] ; 
				int ny = virus.y + dy[i] ; 
				
				if(nx < 0 || nx >= n || ny < 0 || ny >=n)
					continue ; 
				if(graph[nx][ny] == 0) {
					graph[ny][ny] = virus.index; 
					q.offer(new Virus(virus.index, virus.second+1 , nx, ny)) ;
				}
			}
		}
	}
}
```

<br>

