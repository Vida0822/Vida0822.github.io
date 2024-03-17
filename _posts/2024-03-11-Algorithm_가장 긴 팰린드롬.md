---
published: true
title: "Two Pointer-가장 긴 팰린드롬" 
categories: Algorithm 
tag: [algorithm, Programmers, Two Pointer, 완전탐색] 
toc: true
author_profile: false 
  
---



##### 문제 

앞뒤를 뒤집어도 똑같은 문자열을 팰린드롬(palindrome)이라고 합니다.

문자열 s가 주어질 때, s의 부분문자열(Substring)중 가장 긴 팰린드롬의 길이를 return  

<br>





##### 핵심 아이디어 

각 문자('경계')를 기준으로 양쪽 방향의 누적 문자열이 같아지는 문자열의 길이를 구하고 그중 최댓값을 반환한다. 

<br>



##### 알고리즘 

1. 완전 탐색 --> 외부 반복문

* **이때, 팰린드롬의 길이가 홀수인지, 짝수인지 구분하여 누적 문자열을 구해야한다. **
* 홀수인 경우엔 가운데 문자를 어느쪽에도 포함하지 X
* 짝수인 경우 경계 문자를 lSum에 포함시킨다 

<br>

2. Two Pointer --> 내부 반복문

* 각 경계점으로부터 한글자씩 추가하면서 해당 문자열이 대칭인지 확인한다 

* 만약 새로 추가하려는 왼쪽, 오른쪽 문자가 다르면 해당 경계점에 대한 탐색은 종료해도 된다. 

  (다른 문제에선 보통 남은 한쪽으로만 누적합을 합산하는 형태로 전개)

* 어느 쪽 하나가 경계에 닿았을 때도 탐색을 종료한다 (모든 경계점에 대해 양방향 누적을 구하기 때문에 한쪽에 대칭가능 문자가 더 있는 경우는 고려하지 않아도 괜찮다.)

<br>





##### 시간 복잡도 

> O(N^2)

* 외부 반복문: 문자열의 각 문자에 대해 한 번씩 반복하므로 O(n)의 시간이 소요
* 내부 반복문: 각 경계를 기준으로 최악의 경우 O(N/2)  x 2 (홀수,짝수 각각 수행)  = O(N)

<br>





##### 코드 (Java)

```java
class Solution{
    public int solution(String s){
     
        int answer = 0 ; 
        for(int i = 0 ; i < s.length() ; i++){
            answer = Math.max(answer, getPalindrome(s , i-1, i+1)) ; // 홀수인 경우 
            answer = Math.max(answer, getPalindrome(s , i, i+1)) ; // 짝수인 경우 
        }
        return answer ; 
    }
    
    public int getPalindrome(String s, int left, int right){
        while (left >= 0 && right < s.length() ) {
            if(s.charAt(left) == s.charAt(right)){        
                left--;
                right++;
            }else{ 
                break; 
            }
        }
        return right - left - 1; // aabbbb - 실제 : 2 5 --> 마지막 반복: 1 6 
    }
}
```

<br>



##### 유사한 문제 

[Lv4. Two-Pointer-쿠키 구입 | 희민이의 개발노트 (vida0822.github.io)](https://vida0822.github.io/algorithm/Algorithm_쿠키-구입/)



<br>

