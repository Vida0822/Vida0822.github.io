---
published: true
title: "Implementation-문자열 압축" 
categories: Algorithm 
tag: [algorithm, Programmers, Implementation, String] 
toc: true
author_profile: false 
  
---



##### 문제

( 링크 : https://school.programmers.co.kr/learn/courses/30/lessons/60057 ) 

"abcabcdede"와 같은 경우, 문자를 2개 단위로 잘라서 압축하면 "abcabc2de"가 되지만, 3개 단위로 자른다면 "2abcdede"가 되어 

3개 단위가 가장 짧은 압축 방법이 됩니다. 이때 3개 단위로 자르고 마지막에 남는 문자열은 그대로 붙여주면 됩니다.

압축할 문자열 s가 매개변수로 주어질 때, 위에 설명한 방법으로 1개 이상 단위로 문자열을 잘라

압축하여 표현한 문자열 중 가장 짧은 것의 길이를 return

<br>



##### 핵심 아이디어 

> Implementation (구현)

문제에서 제시한 문자열 압축 과정(흐름, 순서)를 그대로 코드로 옮기면 되는 시뮬레이션 문제다. 

그렇기에 아이디어보단 아이디어 --> 코드 가 더 중요함 

모든 분리 단위(step, 1~s/2) 각각의 압축 문자열 갯수를 확인해주는 완전탐색 유형에 해당한다 

<br>



##### Point 

구현 문제는 반복문, 배열 등의 **'Index'**를 잘 설정하는 것이 관건이다. 

prev, sub 기법을 사용해 적절히 초기화, 갱신해줘야한다. 

<br>



##### 시간 복잡도 

> O(N^2)

* 외부 반복문: O(N/2)

  1개 단위(step)부터 압축 단위를 늘려가며 확인 (모든 압축 단위별 문자열 길이 반환 )

* 내부 반복문 : O(N) 

  문자들을 순차적으로 순회하면서 로직 수행 (slicing 단위가 step일 뿐 결국 문자열 끝까지 하나씩 포함해야한다) 

  (다음 단어 뽑기 -> 기준 단어와 동일한지 확인 -> 압축 -> 다음 단어 뽑기 -> ... )

* n = 1000로 작기 때문에 O(N^2) 도 괜찮다 

<br> 



##### 코드

```java
class Solution {
    public int solution(String s) {
        
        // 최악의 경우 : 어떤 문자도 압축되지 않고 문자열이 그대로 반환 
        int answer = s.length() ; 
        
        // 1개 단위(step)부터 압축 단위를 늘려가며 확인 (최대 : 절반 나누기 ** ex) 12글자 --> 6글자가 최대) 
        for(int step = 1 ; step <= s.length() / 2  ; step++){ // O(N/2)
            
            // 해당 step의 최종 압축 문자열 
            String compressed = "" ; 

            // 비교할 기준 문자열 추출 
            String prev = s.substring(0, step) ; // 앞에서부터 step-1까지의 문자열 추출 
            int cnt = 1 ; 
            
            // 단위 크기만큼 문자열 분리 
            for(int j = step; j < s.length() ; j+= step){ // 뒤의 문자를 하나씩 확인(그 단위가 step일 뿐) --> O(N) 
                // 기준 문자열 뒤(step)의 단위 크기만큼의 문자열 뽑아내서 
                String sub = extract(j, j+step, s) ; 
             
                // 이전 문자열과 같은지 확인 
                if(prev.equals(sub))  // 같으면
                    cnt++ ;  // 개수 증가 
                else{ // 다르면 
                    // 현재까지 확인한 문자로 압축 진행 
                    compressed += (cnt >= 2)? cnt+prev: prev ; 
                    
                    // 기준 문자열 초기화 
                    prev = extract(j, j+step, s) ; 
                    cnt=1 ; 
                }
            }
            // 남아있는 문자열 압축 처리 : 만약 마지막 추출 문자열이 압축대상(같은 문자열)인 경우 else분기에 들어가지 않아 압축 과정에 포함되지 않으므로 최종적으로 압축 처리해주고  
            compressed += (cnt >= 2)? cnt+prev : prev;
            
            // 답 갱신 : 문자열 길이 vs 기존 최솟값 
            answer = Math.min(answer, compressed.length()) ; 
        }
        return answer ; 
    }
    
    // ~step 다음 단어를 뽑아내는 메서드 : 반복되는 코드는 메서드로 분리한다
    public String extract(int start, int end, String s){
         String word = "" ; 
         for(int k = start ; k < end ;k++){
            if(k < s.length())
                word+= s.charAt(k) ; 
        }
        return word; 
    }
}
```

<br>



##### 코드 (주석 x)

```java
class Solution {
    public int solution(String s) {
        int answer = s.length() ; 
        
        for(int step = 1 ; step <= s.length() / 2  ; step++){
            String compressed = "" ; 
            String prev = s.substring(0, step) ; 
            int cnt = 1 ; 
            
            for(int j = step; j < s.length() ; j+= step){
                String next = extract(j, j+step, s) ; 
             
                if(prev.equals(next))  
                    cnt++ ;  
                else{ 
                    compressed += (cnt >= 2)? cnt+prev: prev ;          
                    prev = extract(j, j+step, s) ; 
                    cnt=1 ; 
                }
            }
            compressed += (cnt >= 2)? cnt+prev : prev;
            answer = Math.min(answer, compressed.length()) ; 
        }
        return answer ; 
    }
    
    public String extract(int start, int end, String s){
         String word = "" ; 
         for(int k = start ; k < end ;k++){
            if(k < s.length())
                word+= s.charAt(k) ; 
        }
        return word; 
    }
}
