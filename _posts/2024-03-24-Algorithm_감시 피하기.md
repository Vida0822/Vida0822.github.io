---
published: true
title: "DFS-감시 피하기" 
categories: Algorithm 
tag: [algorithm, Programmers, DFS] 
toc: true
author_profile: false 
  
---



##### 문제

N개의 수로 이루어진 수열 A1, A2, ..., AN이 주어진다. 또, 수와 수 사이에 끼워넣을 수 있는 N-1개의 연산자가 주어진다. 

연산자는 덧셈(+), 뺄셈(-), 곱셈(×), 나눗셈(÷)으로만 이루어져 있다.

우리는 수와 수 사이에 연산자를 하나씩 넣어서, 수식을 하나 만들 수 있다. 이때, 주어진 수의 순서를 바꾸면 안 된다.

<br>



##### 핵심 아이디어

> '가능한 모든 조합을 만드는' DFS => 백트래킹

N 값이 매우 작다. 따라서 가능한 모든 벽 조합을 찾는 것 관건이다. 

"X"가 있는 자리에 벽을 넣고 빼며 가능한 모든 조합을 찾고, 

해당 조합을 찾을 때마다 학생들이 모두 피할 수 있는지 확인한다.

이 알고리즘을 사용하기 위해선 DFS 인자로 depth가 반드시 들어가며, 종료 조건으로 depth가 기준 값에 달성하였거나,



백트래킹 조건으로, 만약 한번이라도 학생들이 모두 피할 수 있다면(value가 특정 조건을 만족할 때) 모든 재귀를 종료한다. 

<br>



##### 로직 

1. 벽을 3개 세운다 
2. 학생들이 모두 피할 수 있는지 확인한다
3. 피할 수 없으면 다시 벽을 세운다. 
4. 피할 수 있는 경우가 한번이라도 나오면 answer은 true, 한번도 없으면 false이다. 

<br> 



##### 유사한 문제

https://vida0822.github.io/algorithm/Algorithm_연구소/

<br>



##### Tip 

> 하나의 정수값으로 행,열 위치 뽑아내기

N값이 아주 작으면 상관없지만, 어느정도 큰 값이라면 재귀적으로 벽을 세울 다음 위치를 검사한 위치 나중 값으로 설정해주면 좋다. 아래 코드는 해당 트릭을 위한 코드이다.

+ 기존 

```java
for(int i = 0 ; i < N ; i++){
   for(int j = 0 ; j < N ; j++){
       if(board[i][j].equals("X")){
          board[i][j] = "O" ; 
          count++ ; 
          if(buildObstacle(count))
             return true;
                    
          board[i][j] = "X" ; 
          count-- ; 
       }           
   }
}
return false ;
```



* 수정 

```java
for (int i = start; i < N * N; i++) { 
    int x = i / N; // 행 
    int y = i % N; // 열 
    if (board[x][y].equals("X")) {
        board[x][y] = "O";
        if (buildObstacle(count + 1, i + 1)){
            // count , start(벽 시작위치):현재 행/열 다음위치부터 벽을 세우도록 넘겨준다 
          return true;
        }
        board[x][y] = "X";
    }
}
```

<br>

 

##### 코드

```java
package dfs_bfs ; 
import java.util.* ; 

class 감시피하기{
    public static int N ; 
    public static String[][] board ; 
    public static void main(String[] args){
        
        // 입력 
        Scanner sc = new Scanner(System.in); 
        N = sc.nextInt() ; 
        board = new String[N][N] ; 
        
        for(int i = 0 ; i < N ; i++){
            for(int j = 0 ; j < N ; j++){
                board[i][j] = sc.next() ; 
            }
        }
        // dfs 
        boolean result = buildObstacle(0,0) ; 
        
        // 출력 
        if(result)
            System.out.println("YES") ; 
        else
            System.out.println("NO") ; 
    }
    
    public static boolean buildObstacle(int count, int start ){
        // 종료조건 
        if(count == 3){
            for(int i = 0 ; i < N ; i++){  	
                for(int j = 0 ; j < N ; j++){
                    if(board[i][j].equals("T"))
                        if(catchS(i, j))
                            return false; 
                }
            }
            return true; 
        }
        
        // 백트래킹 
        for (int i = start; i < N * N; i++) {
            int x = i / N;
            int y = i % N;
            if (board[x][y].equals("X")) {
                board[x][y] = "O";
                if (buildObstacle(count + 1, i + 1)) { // 재귀 호출 시 시작 인덱스를 다음으로 옮김
                    return true;
                }
                board[x][y] = "X";
            }
        }
        return false;
    }
    
    public static boolean catchS(int x, int y){
        
        for(int i = y ; i >= 0 ; i--){
            if(board[x][i].equals("S"))
                return true ;
            if(board[x][i].equals("O"))
                break; 
        }
        for(int i = y ; i < N ; i++){
            if(board[x][i].equals("S"))
                return true ;
            if(board[x][i].equals("O"))
                break; 
        }
         for(int i = x ; i >= 0 ; i--){
            if(board[i][y].equals("S"))
                return true ;
            if(board[i][y].equals("O"))
                break; 
        }
        for(int i = x ; i < N ; i++){
            if(board[i][y].equals("S"))
                return true ;
            if(board[i][y].equals("O"))
                break; 
        }
        return false; 
    }
}
```

<br>

 

##### 해설 풀이

```java
import java.util.*;

class Combination {
    private int n;
    private int r;
    private int[] now; // 현재 조합
    private ArrayList<ArrayList<Position>> result; // 모든 조합

    public ArrayList<ArrayList<Position>> getResult() {
        return result;
    }

    public Combination(int n, int r) {
        this.n = n;
        this.r = r;
        this.now = new int[r];
        this.result = new ArrayList<ArrayList<Position>>();
    }

    public void combination(ArrayList<Position> arr, int depth, int index, int target) {
        if (depth == r) {
            ArrayList<Position> temp = new ArrayList<>();
            for (int i = 0; i < now.length; i++) {
                temp.add(arr.get(now[i]));
            }
            result.add(temp);
            return;
        }
        if (target == n) return;
        now[index] = target;
        combination(arr, depth + 1, index + 1, target + 1);
        combination(arr, depth, index, target + 1);
    }
}

class Position {
    private int x;
    private int y;

    public Position(int x, int y) {
        this.x = x;
        this.y = y;
    }

    public int getX() {
        return this.x;
    }

    public int getY() {
        return this.y;
    }
}

public class Main {

    public static int n; // 복도의 크기
    public static char[][] board = new char[6][6]; // 복도 정보 (N x N)
    public static ArrayList<Position> teachers = new ArrayList<>(); // 모든 선생님 위치 정보
    public static ArrayList<Position> spaces = new ArrayList<>(); // 모든 빈 공간 위치 정보

    // 특정 방향으로 감시를 진행 (학생 발견: true, 학생 미발견: false)
    public static boolean watch(int x, int y, int direction) {
        // 왼쪽 방향으로 감시
        if (direction == 0) {
            while (y >= 0) {
                if (board[x][y] == 'S') { // 학생이 있는 경우
                    return true;
                }
                if (board[x][y] == 'O') { // 장애물이 있는 경우
                    return false;
                }
                y -= 1;
            }
        }
        // 오른쪽 방향으로 감시
        if (direction == 1) {
            while (y < n) {
                if (board[x][y] == 'S') { // 학생이 있는 경우
                    return true;
                }
                if (board[x][y] == 'O') { // 장애물이 있는 경우
                    return false;
                }
                y += 1;
            }
        }
        // 위쪽 방향으로 감시
        if (direction == 2) {
            while (x >= 0) {
                if (board[x][y] == 'S') { // 학생이 있는 경우
                    return true;
                }
                if (board[x][y] == 'O') { // 장애물이 있는 경우
                    return false;
                }
                x -= 1;
            }
        }
        // 아래쪽 방향으로 감시
        if (direction == 3) {
            while (x < n) {
                if (board[x][y] == 'S') { // 학생이 있는 경우
                    return true;
                }
                if (board[x][y] == 'O') { // 장애물이 있는 경우
                    return false;
                }
                x += 1;
            }
        }
        return false;
    }

    // 장애물 설치 이후에, 한 명이라도 학생이 감지되는지 검사
    public static boolean process() {
        // 모든 선생의 위치를 하나씩 확인
        for (int i = 0; i < teachers.size(); i++) {
            int x = teachers.get(i).getX();
            int y = teachers.get(i).getY();
            // 4가지 방향으로 학생을 감지할 수 있는지 확인
            for (int j = 0; j < 4; j++) {
                if (watch(x, y, j)) {
                    return true;
                }
            }
        }
        return false;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        n = sc.nextInt();

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                board[i][j] = sc.next().charAt(0);
                // 선생님이 존재하는 위치 저장
                if (board[i][j] == 'T') {
                    teachers.add(new Position(i, j));
                }
                // 장애물을 설치할 수 있는 (빈 공간) 위치 저장
                if (board[i][j] == 'X') {
                    spaces.add(new Position(i, j));
                }
            }
        }

        // 빈 공간에서 3개를 뽑는 모든 조합을 확인
        Combination comb = new Combination(spaces.size(), 3);
        comb.combination(spaces, 0, 0, 0);
        ArrayList<ArrayList<Position>> spaceList = comb.getResult();

        // 학생이 한 명도 감지되지 않도록 설치할 수 있는지의 여부
        boolean found = false; 
        for (int i = 0; i < spaceList.size(); i++) {
            // 장애물들을 설치해보기
            for (int j = 0; j < spaceList.get(i).size(); j++) {
                int x = spaceList.get(i).get(j).getX();
                int y = spaceList.get(i).get(j).getY();
                board[x][y] = 'O';
            }
            // 학생이 한 명도 감지되지 않는 경우
            if (!process()) {
                // 원하는 경우를 발견한 것임
                found = true;
                break;
            }
            // 설치된 장애물을 다시 없애기
            for (int j = 0; j < spaceList.get(i).size(); j++) {
                int x = spaceList.get(i).get(j).getX();
                int y = spaceList.get(i).get(j).getY();
                board[x][y] = 'X';
            }
        }

        if (found) System.out.println("YES");
        else System.out.println("NO");
    }
}
```

