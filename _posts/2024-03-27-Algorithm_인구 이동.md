---
published: true
title: "BFS-인구이동" 
categories: Algorithm 
tag: [algorithm, Programmers, BFS] 
toc: true
author_profile: false 
  
---



##### 문제

https://www.acmicpc.net/problem/16234

<br>



##### 핵심 아이디어

> 두가지 메서드 : 기계적 BFS(or DFS) + 핵심 로직 

이 문제는 BFS 또는 DFS로 연합 가능한 좌표 목록을 뽑아 낸후, 그 즉시 인구를 조정해야한다. 

1. 연합 만들기 : 방문하는 노드 조건으로 연합 가능 (L이상 R이하)인지 검사한 후, 해당 노드를 포함&방문, 이동한다. 
2. 인구 이동하기 : 해당 연합에 포함되는 국가들 인구조정한다
3. 다른 연합 있는지 확인 : 검사 안 한 국가 중 다른 연합이 만들어 질 수 있는지 확인한다
4. 인구 이동하기
5. 시간을 증가한다
6. 다시 처음부터 연합을 만든다. 

<br>



##### TIP 

BFS, DFS는 기계적으로 코드를 생성해내고, 거기에 핵심 로직을 앞뒤에 붙이는 방식으로 복잡한 문제를 풀이한다. 

각각의 기능은 메서드로 분리하여 구성한다. 

<br>



##### 코드

```java
import java.util.*;
import java.io.*;
class Pair {
    int x;
    int y;

    public Pair(int x, int y) {
        this.x = x;
        this.y = y;
    }
}

public class Main {
    public static int N, L, R;
    public static int[][] A;
    public static boolean[][] visited;
    public static int[] dx = {0, 1, 0, -1}; // 수정된 부분
    public static int[] dy = {1, 0, -1, 0}; // 수정된 부분
    public static ArrayList<Pair> union = new ArrayList<>();
    public static boolean isMove = false;
    public static int answer = 0;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());

        N = Integer.parseInt(st.nextToken());
        L = Integer.parseInt(st.nextToken());
        R = Integer.parseInt(st.nextToken());
        A = new int[N][N];

        for (int i = 0; i < N; i++) {
            st = new StringTokenizer(br.readLine());
            for (int j = 0; j < N; j++)
                A[i][j] = Integer.parseInt(st.nextToken());
        }
        move();
        System.out.println(answer);
    }

    static void move() {
        while (true) {
            isMove = false;
            visited = new boolean[N][N]; // 새 BFS 시작할 때마다 방문 초기화
            

            for (int i = 0; i < N; i++) {
                for (int j = 0; j < N; j++) {
                    if (visited[i][j])
                    	continue; 
                    
                    // 연합 만들기
                    bfs(i, j);

                    // 인구 이동하기
                    movePoplulation();
                    
                    // 연합 비우기 
                    union.clear();
                }
            }
            if (!isMove) break;
            else answer++;
        }
    }

    static void bfs(int x, int y) {
        Queue<Pair> q = new LinkedList<>();
        q.add(new Pair(x, y));
        visited[x][y] = true;
        union.add(new Pair(x, y));

        while (!q.isEmpty()) { // bfs 적용은 암기, 기계처럼 하기
            Pair p = q.poll();
            for (int i = 0; i < 4; i++) {
                int nx = p.x + dx[i];
                int ny = p.y + dy[i];

                if (nx < 0 || ny < 0 || nx >= N || ny >= N)
                    continue;
                if (visited[nx][ny])
                    continue;
                if (Math.abs(A[p.x][p.y] - A[nx][ny]) < L || Math.abs(A[p.x][p.y] - A[nx][ny]) > R)
                    continue;

                isMove = true;
                visited[nx][ny] = true;
                union.add(new Pair(nx, ny));
                q.add(new Pair(nx, ny));
            }
        }
    }

    static void movePoplulation() {
        int sum = 0;
        for (int i = 0; i < union.size(); i++) {
            Pair p = union.get(i);
            sum += A[p.x][p.y];
        }
        int avg = sum / union.size(); // 수정된 부분
        for (int i = 0; i < union.size(); i++) {
            Pair p = union.get(i);
            A[p.x][p.y] = avg;
        }
       
    }
}

```







