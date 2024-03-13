---
published: true
title: "Stack-괄호 회전하기" 
categories: Algorithm 
tag: [algorithm, Programmers, Stack, Bracket] 
toc: true
author_profile: false 
  
---



##### 문제 

대괄호, 중괄호, 그리고 소괄호로 이루어진 문자열 `s`가 매개변수로 주어집니다. 

이 `s`를 왼쪽으로 x (*0 ≤ x < (`s`의 길이)*) 칸만큼 회전시켰을 때 

`s`가 올바른 괄호 문자열이 되게 하는 x의 개수를 return

<br>





##### 핵심 아이디어 

> Stack

괄호를 집어넣고, 조건에 맞으면 스택에서 빼는 과정을 반복해 최종적으로 스택이 비었는지 여부를 반환한다 

* 소괄호, 중괄호, 대괄호로 나누어져 있어 괄호 문제 중 난이도가 있는 문제 
* 조건 : pop하기 전에 비어있는지(조건 1) 훔쳐본 괄호가 동일한 종류의 괄호인지(조건 2**) 먼저 확인한다  

<br>



##### 시간 복잡도 

> O(N^2)

* 외부 반복문: 문자열 길이만큼 한번씩 회전시킨다 
* 내부 반복문: 변환된 문자열을 한글자씩 확인하며(스택) 올바른 괄호인지 확인한다

<br>





##### 코드 (Java)

```java
import java.util.* ;
class Solution {
    public int solution(String s) {
        int answer = 0 ; 
        if(s.length()%2 == 1)
            return 0; 
            
        for(int i = 0 ; i < s.length() ; i++){
            s = rotationBracket(s) ; 
        
            if(isProper(s))
                answer++ ; 
        }
        return answer ;
    }
    
    static String rotationBracket(String s){
        return s.substring(1) + s.charAt(0) ; 
    }
    
    static boolean isProper(String s){
        Stack<Character> stack = new Stack<>() ;
        
        for(int i = 0 ; i < s.length();i++){
            char c = s.charAt(i) ; 
            if(c == '[' || c == '{' || c == '(')
                stack.push(c) ; 
            else{
                if(stack.isEmpty())
                    return false ; 
                switch(c){
                    case ']' : 
                        isProperBracket(stack, '['); 
                        break ; 
                    case '}' : 
                        isProperBracket(stack,'{') ; 
                        break ; 
                    case ')' : 
                        isProperBracket(stack,'(') ; 
                        break ; 
                }
            }
        }
        return stack.isEmpty() ; 
    }
        
    static void isProperBracket(Stack<Character> stack, char bracket){
        if(stack.peek() == bracket)
            stack.pop() ; 
    }
}
```

<br>



##### TIP : 메서드 구분 **

코드가 길어지는, 기능이 여러가지인 복잡한 기능문제인 경우, 

메서드 명으로만 기능을 명시하여 큰 틀(main)을 짠 후, 

해당 기능을 수행하도록 각각의 메서드를 구현한다

<br>

