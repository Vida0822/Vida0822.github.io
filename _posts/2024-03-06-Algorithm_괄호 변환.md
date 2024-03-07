---
published: true
title: "DFS-괄호변환" 
categories: Algorithm 
tag: [algorithm, Programmers, DFS, Stack] 
toc: true
author_profile: false 
  
---



##### 문제 

* 링크 : https://school.programmers.co.kr/learn/courses/30/lessons/60058?language=python3

**"균형잡힌 괄호 문자열"** p가 매개변수로 주어질 때, 주어진 알고리즘을 수행해 **"올바른 괄호 문자열"**로 변환한 결과를 return 

1. 입력이 빈 문자열인 경우, 빈 문자열을 반환합니다. 

2. 문자열 w를 두 "균형잡힌 괄호 문자열" u, v로 분리합니다. 

     단, u는 "균형잡힌 괄호 문자열"로 더 이상 분리할 수 없어야 하며, v는 빈 문자열이 될 수 있습니다. 

3. 문자열 u가 "올바른 괄호 문자열" 이라면 문자열 v에 대해 1단계부터 다시 수행합니다. 

4. 3-1. 수행한 결과 문자열을 u에 이어 붙인 후 반환합니다. 

5. 문자열 u가 "올바른 괄호 문자열"이 아니라면 아래 과정을 수행합니다. 
     4-1. 빈 문자열에 첫 번째 문자로 '('를 붙입니다. 

     4-2. 문자열 v에 대해 1단계부터 재귀적으로 수행한 결과 문자열을 이어 붙입니다. 

     4-3. ')'를 다시 붙입니다. 

     4-4. u의 첫 번째와 마지막 문자를 제거하고, 나머지 문자열의 괄호 방향을 뒤집어서 뒤에 붙입니다. 

     4-5. 생성된 문자열을 반환합니다.



<br>



##### 핵심 아이디어 

> DFS 

문자열 v에 대해 재귀적으로 수행 (dfs 예상) 

* 문자열 크기: 2 이상 1,000 이하 (작은 데이터값) 
* 문제 자체가 재귀함수 구성 로직



##### 로직

1. u, v로 분리 - u기준 : 괄호 갯수 조건 일치하는 부분까지 u로 포함, 나머지가 v  
2. isRightExp() : 올바른 괄호인지 확인 --> 이 때 괄호의 짝 검사 위해 **Stack** 자료구조 이용
3. 올바른 괄호면 result 문자열에 누적하고, 올바르지 않으면 문제 지시사항에 따라 변환 
4.  v에 대해 재귀적으로 수행한 문자열 적절히 누적 (v가 검사 문자열인 p로써 기능)

<br>

 



##### 코드 (Java) 

```java
import java.util.* ; 
class Solution {    
    // 올바른 괄호로 변환하는 메서드
    public String makeCorrectString(String u){
        String changed = "" ; 
        for(int i = 1 ; i < u.length()-1 ; i++){
            char c = u.charAt(i) ; 
            if(c == '(')
                changed += ")" ; 
            else 
                changed += "(" ; 
        }
        return changed ; 
    }
    // '올바른 괄호'인지 확인하는 메서드
    public boolean isRightExp(String u){
        Stack<Character> stack = new Stack<>() ; 
        for(int i = 0 ; i < u.length() ; i++){
            char c = u.charAt(i) ; 
            if(c == ')' ){
                if(stack.isEmpty()) return false ; 
                else  stack.pop() ; 
            }  
            else 
                stack.push(c) ; 
        }
        return stack.size() == 0 ? true : false ;
    }
    // u, v 분리시킬 인덱스를 반환하는 메서드 
    public int v_index(String p){
        int left = 0, right = 0 ; 
        
        for(int i = 0 ; i < p.length() ; i++){
            char c = p.charAt(i) ; 
        
            if(c == '('){
                left++ ;  
            }else{
                right++ ; 
            }
            if(left == right) {
                return i+1 ;  // 분리할 index를 반환 (v 시작점)
            }
        }
        return 0 ; 
    }
    
    public String dfs(String p){
        String result = "" ; 
        
        // 종료조건
        if(p.length() == 0)
            return p ; 
        
        // u, v 분리 
        int v_index = v_index(p) ; 
        String u = p.substring(0, v_index);
        String v = p.substring(v_index, p.length());
               
        // u가 올바른 괄호인지 확인  
        if(isRightExp(u)) 
            result +=  u + dfs(v) ; 
        else{
        // 올바른 괄호가 아니라면 올바른 괄호로 변환
            result += "(" + dfs(v) +")" + makeCorrectString(u); 
        }        
        return result ; 
    }
    
    public String solution(String p) {
        return dfs(p) ; 
    }
}
```

<br>





##### 코드(Python)

```python
# 올바른 괄호로 변환 
def makeCorrect(u) : 
    u = list(u[1:-1]) # 첫번째와 마지막 문자 제거 + 리스트형으로 변환 
    for i in range(len(u)) : 
        if u[i] == '(' : 
            u[i] = ')'
        else : 
            u[i] = '('
    return "".join(u)   
    # '구분자'.join(리스트) :리스트의 각 요소를 하나의 문자열로 이어 붙여주는 함수
    # ex) '_'.join(['a', 'b', 'c']) 라 하면 "a_b_c"

# 올바른 순서인지 확인 
def check_proper(u) : 
    count = 0 
    for i in u : 
        if i == '(': 
            count += 1
        else :
            if count == 0 : 
                return False
            count -= 1 
    return True   # 개수는 무조건 맞으니까(balanced_index()) 순서만 올바르면 해당 괄호는 올바른 괄호 

# 개수만 맞는지 확인 (그 개수가 맞는 index반환)
def balanced_index(p) : 
    count = 0 # 왼쪽 괄호의 개수 
    for i in range(len(p)):
        if p[i] == '(' : 
            count += 1 
        else : 
            count -= 1 
        if count == 0 : 
            return i 
        
def dfs(p):
    answer = '' 
    if p == '' :
        return answer 
    index = balanced_index(p) 
    u = p[ : index+1]
    v = p[index+1 : ]

    if check_proper(u) : 
        answer = u + dfs(v) 
    else : 
        answer = '(' + dfs(v) + ')' + makeCorrect(u) 

    return answer 

def solution(p) : 
    return dfs(p) 
```



 

