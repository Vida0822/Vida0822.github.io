---
published: true
title: "Simulation-기둥과 보 설치" 
categories: Algorithm 
tag: [algorithm, 카카오, Implementation, Simulation, 격자] 
toc: true
author_profile: false 
  
---



##### ❔ 문제

lv3. 기둥과 보 설치(카카오)

[코딩테스트 연습 - 기둥과 보 설치 | 프로그래머스 스쿨 (programmers.co.kr)](https://school.programmers.co.kr/learn/courses/30/lessons/60061)<br>

<br>



##### 💡 핵심 아이디어

문제에서 요구하는 걸 그대로 코드 구현이 어려워

문제를 해결하기 위한 아이디어를 재구성, 재정의 한 후 코드를 작성해야한다. 

이 문제의 핵심 아이디어는 설치, 삭제할 때 가능한지 여부를 확인하기 복잡하므로 

"일단 설치/삭제를 한 후" 모든 구조물들이 올바른 요구조건을 만족하는지 **전수 조사** 한다.  

<br>

<br>

##### 🌊 로직 

1. 명령어를 하나씩 읽어들이면서... 
2. method1 : '설치'하는 메서드 (Board 변화)  호출 또는
3. method 2 : '삭제'하는 메서드 (Board 변화) 호출 
4. method 3 : '전수조사'하는 메서드 호출 --> 기둥, 보 각각에 맞춰 요구조건 검사 
5. 만약 불가능한 구조면 설치/삭제 Rollback 

<br>

<br>





##### ❌ 내 코드 : 테스트 케이스 통과 X 

```java
import java.util.* ;

class Solution {
    public static int[][] board ; 
    public static int n; 
    
    public int[][] solution(int n, int[][] build_frame) {
        
        // board setting 
        this.board = new int[n+1][n+1] ;   
        this.n = n ; 
        
        for(int i = 0 ; i < n+1 ; i++){
            for(int j = 0 ; j < n+1 ; j++){
                board[i][j] = -1 ; 
            }
        }
        
        // main : 명령어 해독 및 메서드 호출 
        for(int[] req : build_frame){
            int x = req[0], y = req[1] ; 
            int type = req[2] ; 
            int oper = req[3] ; 
            
            if(x < 0 || x > n || y < 0 || y > n )
                continue ; 
            
            if(oper == 1)
                install(x, y, type ) ; 
            else
                distall(x, y, type) ; 
        }
        
        // 답 변환
        ArrayList<int[]> list = new ArrayList<>() ; 
        for(int i = 0 ; i < n+1 ; i++){
            for(int j = 0 ; j < n+1 ; j++){
                if(board[j][i] == -1)
                    continue ; 
                if(board[i][j] == 2){
                    list.add(new int[]{i, j, 0}) ; 
                    list.add(new int[]{i, j, 1}) ; 
                    continue ; 
                } 
                list.add(new int[]{i, j, board[j][i]}) ; 
            }
        }
        list.sort((a, b) ->
                a[0] - b[0] == 0 ?
                        a[1] - b[1] == 0 ?
                                a[2] - b[2] : a[1] - b[1] : a[0] - b[0]);
        return list.toArray(new int[0][0]) ; 
    }
    
    public static void install(int x, int y, int type){        
        
        int temp = board[y][x]  ;             
        if(board[y][x] == -1)
            board[y][x] = type ; 
        else
            board[y][x] = 2;
                    
        if(!check()) 
            board[y][x] = temp ; 
    }
    
    public static void distall(int x, int y, int type){
        

        int temp = board[y][x] ;       
        if(board[y][x] == 2) 
            board[y][x] -= (type+1) ; 
        else
            board[y][x] = -1 ; 
        
        if(!check())
            board[y][x] = temp ; 
    }       
    
    public static boolean check(){
                
        for(int i = 0 ; i <= n ; i++){
            for(int j = 0 ; j <= n ; j++){
                if(board[i][j] == -1)
                    continue ; 
                
                if(board[i][j] == 0 || board[i][j] == 2){ // 기둥 존재 가능 조건 
                   
                    boolean flag = false ; 
                    if(board[i][j] == 0 && i == 0)
                        flag = true ;
                    
                    // 아래에 기둥  
                    if(i != 0 && (board[i-1][j] == 0 || board[i-1][j] == 2))
                        flag = true ; 
                    
                    
                    if(board[i][j] == 1 || board[i][j] == 2)
                        flag = true ; 
                    
                   
                    if(j != 0 && (board[i][j-1] == 1 || board[i][j-1] == 2)) 
                        flag = true ; 
                    
                    if(!flag)
                        return false; 
                }
                
                if(board[i][j] == 1 || board[i][j] == 2){ // 보 존재 가능 조건 
                    
                    boolean flag = false ;                      
                    
                    // 한쪽 아래 기둥 
                    if(i != 0 && (board[i-1][j] == 0 || board[i-1][j+1] == 0 
                      || board[i-1][j] == 2 || board[i-1][j+1] == 2))
                        flag = true ;
                        
                    // 양쪽 옆이 보   
                    if(i != 0 && j > 0 && board[i][j-1] >= 1 && board[i][j+1] >= 1)
                        flag = true ; 
                        
                    if(!flag)
                        return false ;   
                }
            }
        }
        return true; 
    }
}
```

<br>

<br>



##### 🎈Point 

> 같은 위치에 기둥과 보 모두 설치 가능 

단순 배열 격자를 활용하면 이 두개가 동시에 설치되어있는 경우를 별도의 숫자로 따로 표시해야 함.

그렇게 하면 조건식이 두배로 복잡해짐. 

그 대신, 해설에서는 ArrayList에 존재하는 구조물 자체를 삽입하여 각각을 별도의 구조물로 취급 & 존재 가능 여부 검사.  

<br>

<br>





##### ✅ 코드 

>  최악의 경우 O(M * N^2)  + a 

1. 명령문을 순차적으로 하나씩 실행하므로 O(M) = O(1000)
2. 명령 수행할 때 마다 전수 조사를 하기 때문에 O(N^2) = O(100^2)
3. 삭제 시 구조물 list 완전 탐색, 구조물 list 정렬 후 완전 탐색으로 답 배열 구성  ...

따라서 예상 시간복잡도는 O(1000*100^2) 이다. 

```java
import java.util.*; 

class Node implements Comparable<Node>{
    int x ; 
    int y ; 
    int stuff ; 
    
    public Node(int x, int y, int stuff){
        this.x = x ; 
        this.y = y ; 
        this.stuff = stuff ; 
    }
    
    @Override 
    public int compareTo(Node other){
        if(this.x == other.x && this.y == other.y)
            return Integer.compare(this.stuff, other.stuff) ; 
        if(this.x == other.x)
            return Integer.compare(this.y, other.y) ; 
        return Integer.compare(this.x , other.x) ; 
    }
}

class Solution{
    
    public int[][] solution(int n, int[][] build_frame){
        ArrayList<ArrayList<Integer>> answer = new ArrayList<ArrayList<Integer>>() ; 
        
        for(int i = 0 ; i < build_frame.length ; i++){
            int x = build_frame[i][0] ; 
            int y = build_frame[i][1] ; 
            int stuff = build_frame[i][2]  ;
            int operate = build_frame[i][3] ; 
            
            if(operate == 0){
                int index = 0 ; 
                for(int j = 0 ; j < answer.size() ; j++){
                    if(x == answer.get(j).get(0) && y == answer.get(j).get(1) && stuff == answer.get(j).get(2))
                        index = j ; 
                }
                ArrayList<Integer> erased = answer.get(index ) ; 
                answer.remove(index) ; 
                if(!possible(answer))
                    answer.add(erased) ; // 그 객체 그대로 설치 
                
            }else{
                ArrayList<Integer> inserted = new ArrayList<Integer>() ; 
                inserted.add(x) ; 
                inserted.add(y) ; 
                inserted.add(stuff) ; 
                answer.add(inserted) ; 
                if(!possible(answer))
                    answer.remove(answer.size()-1) ; 
            }
                
        }
        ArrayList<Node> ans = new ArrayList<Node>() ; 
        for(int i = 0 ; i < answer.size() ; i++)
            ans.add(new Node(answer.get(i).get(0), answer.get(i).get(1) , answer.get(i).get(2))) ; 
        Collections.sort(ans) ; 
        
        int[][] res = new int[ans.size()][3];
        for (int i = 0; i < ans.size(); i++) {
            res[i][0] = ans.get(i).x;
            res[i][1] = ans.get(i).y;
            res[i][2] = ans.get(i).stuff;
        }
        return res;
    }
    
    public boolean possible(ArrayList<ArrayList<Integer>> answer){
        for(int i = 0 ; i < answer.size() ; i++){
            int x = answer.get(i).get(0) ; 
            int y = answer.get(i).get(1) ; 
            int stuff = answer.get(i).get(2) ; 
            
            if(stuff == 0){
                boolean check = false ; 
                
                if(y == 0)
                    check = true ; 
                
                // 보의 한쪽 끝 위 또는 다른 기둥 위라면 성상 
                for(int j = 0 ; j < answer.size() ; j++){
                    if(1 == answer.get(j).get(2) && 
                       x-1 == answer.get(j).get(0) && 
                        y == answer.get(j).get(1))
                        check = true ; 
                    if(1 == answer.get(j).get(2) &&
                      x == answer.get(j).get(0) && 
                      y == answer.get(j).get(1))
                        check = true ; 
                    if(0 == answer.get(j).get(2) &&
                      x == answer.get(j).get(0) && 
                      y-1 == answer.get(j).get(1))
                        check = true ; 
                }     
            if(!check) return false ; 
        }else if(stuff == 1){
            boolean check = false ; 
            boolean left = false ; 
            boolean right = false ; 
            
            for(int j = 0 ; j < answer.size() ; j++){
                if( 0 == answer.get(j).get(2) && 
                   x == answer.get(j).get(0) && 
                  y-1 == answer.get(j).get(1)) 
                    check = true ; 
                if( 0 == answer.get(j).get(2) && 
                   x+1 == answer.get(j).get(0) && 
                  y-1 == answer.get(j).get(1)) 
                    check = true ; 
                
                if(1 == answer.get(j).get(2) && 
                   x-1 == answer.get(j).get(0) && 
                  y == answer.get(j).get(1))
                    left = true ;
                
                if(1 == answer.get(j).get(2) &&
                  x+1 == answer.get(j).get(0) && 
                  y == answer.get(j).get(1))
                    right = true ; 
                }
            if(left && right) check = true ; 
            if(!check) return false ; 
            }
        
        }
        return true ; 
    }
}
```

<br>

<br>

