---
published: true
title: "Stack-짝 지어 제거하기" 
categories: Algorithm 
tag: [algorithm, Programmers, Stack] 
toc: true
author_profile: false 


---



##### 1. 이중 반복문 

→ ❌ 시간초과 : O(N^2)

```java
import java.util.* ; 
class Solution{
    public int solution(String s){

        int len = s.length(), i = 0 ; 
        while(i != len){
            a: for(i = 0 ; i < len-1; i++){
                if(s.charAt(i) == s.charAt(i+1))
                    break a ;
            }
            String str1 = s.substring( 0, i) ; // i-1 일 때 무한 루프
            String str2 = s.substring( (i+2 > len? len : i+2) , len ) ; 
            s = str1 + str2 ; 
            len = s.length() ; 
        }
        return len == 0 ? 1 : 0 ; 
    }
}
```



##### 2. 스택을 이용한 풀이 

—> ✔ : O (s.length)

```java
import java.util.* ; 
class Solution{
    public int solution(String s){

        Stack<Character> stack = new Stack<>() ;         
        for(int i = 0 ; i < s.length(); i++){
            char character = s.charAt(i) ; 
            if(!stack.isEmpty() && stack.peek() == character)
                stack.pop() ; 
            else
                stack.push(character) ; 
        }
        return stack.isEmpty() ? 1 : 0 ; 
    }
}
```



- Stack,Queue는 객체를 저장하는 자료구조. Java에서 Stack 클래스는 객체를 저장하기 위한 클래스이기 때문에 기본적으로 char나 int와 같은 원시 타입(primitive type)을 직접 저장할 수는 없다
- 따라서 char나 int를 객체로 래핑하여(예: Character, Integer) Stack에 넣는다
- 그러나 보통 pop,push 등의 자료구조 연산 시 자동으로 기본 자료형에서 매핑 해주기 때문에 구현할 땐 신경 안써도 됨
