---
published: true
title: "DP-단어퍼즐" 
categories: Algorithm 
tag: [algorithm, Programmers, DP] 
toc: true
author_profile: false 
  
---



##### 문제

링크 : https://school.programmers.co.kr/learn/courses/30/lessons/12983

단어 퍼즐은 주어진 단어 조각들을 이용해서 주어진 문장을 완성하는 퍼즐입니다. 이때, 주어진 각 단어 조각들은 각각 무한개씩 있다고 가정.

사용 가능한 단어 조각들을 담고 있는 배열 strs와 완성해야 하는 문자열 t가 매개변수로 주어질 때, 주어진 문장을 완성하기 위해 사용해야 하는 단어조각 개수의 최솟값을 return

<br>



##### 재귀 : X (시간 초과)

```java
import java.util.* ; 

class Puzzle{
    String word ; 
    int depth ;
    
    public Puzzle(){
        this.depth = 0 ;
    }
    public Puzzle(String word, int depth){
        this.word = word ; 
        this.depth = depth ; 
    }
}

class Solution {    
    public int solution(String[] strs, String t) {
        int answer = dfs(new Puzzle("",0), strs, t)  ; 
        return answer == 20000? -1 : answer ; 
    }
    
    public int dfs(Puzzle input, String[] strs, String t){
        
        // ending condition 
        if(input.word.equals(t))
            return input.depth ; 
        
        if(input.word.length() >= t.length())
            return 20000 ; 
        
        // backtracking 
        if(input.word != "" && input.word.charAt(input.word.length()-1) != t.charAt(input.word.length()-1))
            return 20000 ; 
        
        // logic
        String baseWord = input.word ; 
        char left = t.substring(baseWord.length()).charAt(0) ; 
        
        // searching 
        int min = 20000 ; 
        for(String str : strs){
            if(str.charAt(0) == left){
                Puzzle newP = new Puzzle(baseWord + str, input.depth+1) ; 
//                System.out.println(baseWord+str+" " + (input.depth+1)); 
                min = Math.min(min, dfs(newP, strs, t))   ;
            }
        }
        return min; 
    }
}
```

<br>







------

##### 핵심 아이디어

> DP

t의 문자 각각에 대해 해당 문자까지 구성하는데 필요한 단어 퍼즐 갯수의 최댓값을 dp[i]로 기록

* 점화식: dp[i] = Math.min(dp[i], dp[i-퍼즐.length]+1)

<br>

##### 해설 풀이

```java
import java.util.* ; 

class Solution {    
    public int solution(String[] strs, String t) {
        int[] dp = new int[t.length()+1] ; 
        Arrays.fill(dp, 20000) ; // t길이 최댓값
        
        // 1. 초기 조건 
        dp[0] = 0 ;  
        // 문자 각각에 대해 dp 배열 채우기 
        for(int i = 1 ; i < t.length()+1 ; i++){
		        // 단어 퍼즐을 하나씩 검사하면서 
            for(int j = 0 ; j < strs.length; j++){
                String word = strs[j] ; 
               
                if(i < word.length()) // 해당 단어 퍼즐의 길이가 현재글자까지의 길이보다 길다면
                    continue ; // 해당 퍼즐은 패스 
                
                // 2. 점화식 
                if(word.equals(t.substring(i-word.length(), i))){ // 현재 글자까지의 마지막 글자들이 단어 퍼즐 글자과 일치한다면
                    /*if(i - word.length() == 0) // 단어 퍼즐 하나로 현재까지의 문자를 만들 수 있는경우 
                        dp[i] = 1 ; // 단어 하나가 최소값 
                    else if(dp[i-word.length()]>0) // 단어 퍼즐이 더 필요하다면 (이전 문자열이 더 존재하면) 
                        dp[i] = Math.min(dp[i] , dp[i-word.length()]+1) ; // 이전 위치까지의 최소 단어 갯수 + 1 과 기존 dp 값 중 작은값으로 갱신
	                  => 생략 가능 
	                  */  
	                  dp[i] = Math.min(dp[i] , dp[i-word.length()]+1) ; 
                }
            }
        } 
        // 3. 최종 값 반환 
        return dp[t.length()] == 0? -1 : dp[t.length()] ; 
    }
}
```

<br>



##### Tip

재귀함수로 효율성 테스트를 통과하지 못한다면,

dp + 반복문을 생각해보자 !

<br>

