---
published: true
title: "Simulation-표 편집" 
categories: Algorithm 
tag: [algorithm, Simulation, Implementation, Kakao] 
toc: true
author_profile: false 
  
---



##### ❔ 문제

카카오 lv3. 표 편집 

[코딩테스트 연습 - 표 편집 | 프로그래머스 스쿨 (programmers.co.kr)](https://school.programmers.co.kr/learn/courses/30/lessons/81303)

<br>

<br>



##### 💡 핵심 아이디어

이 문제는 표 내 이동, 삭제, 복구라는 행동을 코드로 구현하는 구현, 즉 '시뮬레이션 문제'이다. 

문제에 제시된 전개 방향을 그대로 코드에 구현하고 

예외 사항을 꼼꼼히 반영한다. 

<br>

<br>



##### ❌ 내 코드

```java
import java.util.* ; 

class Solution {
    public static Stack<Integer> stack = new Stack<>() ; 
    public static int tb_size; 
    public String solution(int n, int k, String[] cmd) {
        // step 1. ArrayList<> 채워넣기 
        // indexing 가능 + 정렬된 상태 유지 자료구조  
        this.tb_size = n ; 
    
        int cur = k ; 
        for(String req : cmd){ // 200,000
            String operation = req.substring(0,1) ; 
            
            switch(operation){
                case "U" :
                    cur = up(cur,Integer.parseInt(req.substring(2,3))) ; 
                    break; 
                case "D" : 
                    cur = down(cur,Integer.parseInt(req.substring(2,3))) ; 
                    break ;
                case "C" : 
                    tb_size-- ; 
                    cur = delete(cur) ;
                    break; 
                case "Z" : 
                    tb_size++ ; 
                    cur = restore(cur) ; 
                    break; 
            }
        } 
        StringBuilder builder = new StringBuilder();
        for(int i = 0 ; i < tb_size; i++)
            builder.append("O");
        while(!stack.isEmpty())
            builder.insert(stack.pop().intValue(), "X");
        return builder.toString(); 
    }
    
    
    public static int up(int cur, int size){
        // 건너뛰기 로직... 
        return cur-size ; 
    }
    
    // cur - (cur-i)
    public static int down(int cur, int size){      
        return cur+size ; 
    }
    
    public static int delete(int cur){
        stack.push(cur) ; 
        if(cur == tb_size)
            return up(cur,1) ; 
        return cur ; 
    }
    
    public static int restore(int cur){   
        
        if(stack.pop() <= cur)
            return down(cur,1) ;
        return cur ; 
    }
}
```

<br>

<br>



##### 🤔 에러 원인

메서드를 분리해서 각각 다른 메서드에서 static 변수를 조정하는 것이 에러 원인으로 예상된다. 

만약 메서드를 분리했을 경우, 매개변수를 주고받는 방식으로 사용하고 static 변수 사용은 최소화한다.

그리고 로직이 짧을 경우 main으로 다시 합치는 방법도 고려한다(최후)

<br>

<br>



##### 🌊 로직 : 두번 정렬

>  Step 1. 표 이동하기 

실제로 표를 구현해 이동하는 것이 아닌 현재 포인터가 가리키는 '인덱스'만 변화시킨다. 

마지막 답 출력시 실제 표의 내용이 아닌 삭제 여부에 따른 OX만 출력하기 때문에 가능하다.

(안그랬으면 LinkdedList-prev,next로 표를 구현했어야 한다.)

삭제, 복구시에도 표를 직접 변형시키는 것이 아닌 삭제, 복구된 행의 인덱스만 파악하고 현재 포인터 위치를 조정한다. 

<br> 

>  Step 2. OX로 답 출력하기

Stack에 쌓아놓은 '제거된 요소의 인덱스' 제외 "O"를 출력한다.

남아있는 행이면 O를 출력하는 것이 아닌 삭제된 행이면 X를 출력하는 방식이어야한다. 

<br>

<br>



##### 🎈Point 

> 1. Stack 사용 

Stack은 '제거된 요소의 인덱스'이다. 

'복구'를 구현하기 위해 Stack을 사용한다. 

최근에 삭제된 순서대로 위에 쌓으면(push) 가장 최근의 삭제 요소를 복구(pop)한다. 

마지막 출력시 해당 제거된 목록의 인덱스에 해당하는 문자만 O에서 X로 치환한다. 

<br>

> 2.  StringBuilder 

문자열을 계속 누적, 변환해야하는 부분이기 때문에 단순히 String을 사용하면 공간, 시간 효율이 떨어진다. 

따라서 StringBuilder를 사용하여 정답 문자열을 요리조리 조립한 뒤 

최종적으로 String으로 변환해 반환한다. 

StringBuilder의 사용법은 암기해두는 것이 좋다. 

<br>

<br>



##### ✅ 코드 

>  최악의 경우 O(N)  
>
>  = Step 1 - O(cmd.length) = O(200,000) + Step 2 - O(n) = O(1,000,000)

정렬 후 전체검사 x 2 

```java
import java.util.* ; 

class Solution { 
    public String solution(int n, int k, String[] cmd) {
        Stack<Integer> remove_order = new Stack<Integer>() ;  
        for(int i = 0 ; i < cmd.length ; i++){
            char c = cmd[i].charAt(0) ; 
            
            if(c == 'D')
                k += Integer.parseInt(cmd[i].substring(2)) ; // 현재 '위치'만 표시함 (실제 그 표를 구현할 필요 없음)
            else if(c=='U')
                k -= Integer.parseInt(cmd[i].substring(2)) ; 
            else if(c == 'C'){
                remove_order.add(k) ; // 제거된 행 번호 추가 
                n-- ; // 테이블 사이즈 감소 
                if(k == n)// 선택된 행이 테이블 사이즈를 벗어나면 
                    k-- ; 
            }else if(c == 'Z'){
                if(remove_order.pop() <= k)
                    k++ ;
                n++ ; 
            }
        }
        
        // Step 2. 답 출력 
        StringBuilder builder = new StringBuilder();
        for(int i = 0 ; i < n; i++)
            builder.append("O");
        while(!remove_order.isEmpty())
            builder.insert(remove_order.pop().intValue(), "X"); 
        	// 스택에서 인덱스 꺼내 해당 위치에 "X"를 삽입. 삭제된 항목이 "O"에서 "X"로 변경
        return builder.toString();
    }
}
```

<br>

<br>

