---
published: true
title: "DFS-단어 변환" 
categories: Algorithm 
tag: [algorithm, Programmers, DFS] 
toc: true
author_profile: false 


---



```java
import java.util.* ; 
class Solution{    
    static class Node{ // <단어, 변환횟수> 
        String word ;  // words[i] 가 담길 
        int steps ; // 몇다리 건너서 여기까지 변환
        
        public Node(String word, int steps){
            this.word = word ; 
            this.steps = steps ; // 이 Node 까지 몇개의 edge? 
        }
    } // Node
    
    public int solution(String begin, String target, String[] words){
        int n = words.length ; // 단어 길이 (모두 동일)
        boolean[] visited = new boolean[n];  // 각 단어 방문 여부 
        int answer = 0 ;
       
        Queue<Node> q = new LinkedList<>() ;
        q.add(new Node(begin , 0)) ; // begin 부터 탐색 시작 
        
        while(!q.isEmpty()){
            Node cur = q.poll() ; 
            if(cur.word.equals(target)){ //만약 queue에서 뺀 단어가 target이면 (※단어 집합에 target도 포함되어있음)
                answer = cur.steps ; //  변환 완료 상태 --> 현재까지의 steps 반환 
                break ; 
            } // if 
            
            // queue에서 뺀 단어가 중간단어면 
            for(int i = 0 ; i < n ; i++){ // 해당 단어에서 다른 단어들로의 변환 가능여부 확인 
                if(!visited[i] && isConvertable(cur.word, words[i])){ // 방문하지 않았고,해당노드로 변환 가능하면
                    visited[i] = true ;  // 방문표시 후 
                    q.add(new Node(words[i] , cur.steps+1)) ; // 검사 목록에 추가 
                } // if 
            } // for
        } // while 
        return answer ; 
    } // solution
    
    static boolean isConvertable(String cur, String n){
        int cnt = 0 ; 
        for(int i = 0 ; i < n.length() ; i++){
            if(cur.charAt(i) != n.charAt(i)){
                if(++cnt > 1) // count 값을 1 증가한 값이 1보다 크면 
                    return false ; 
            }
        } // for 
        return true ; 
    } // isConvertable
}
```

