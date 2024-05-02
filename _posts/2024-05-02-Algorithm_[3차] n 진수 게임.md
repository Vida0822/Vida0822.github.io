---
published: true
title: "Implementation(String)-[3차]n진수 게임" 
categories: Algorithm 
tag: [algorithm,Implementation, 카카오, String, Decimal] 
toc: true
author_profile: false 
  
---



##### ❔ 문제

카카오 lv2. [3차] n진수 게임

https://school.programmers.co.kr/learn/courses/30/lessons/17687

<br>

<br>



##### 💡 핵심 아이디어

> 문자열 가공 + 까다로운 코드 구현 => '구현' 문제

- 문제 이해 : 두 자리 숫자는 한사람이 한글자씩 말하면서 순서를 바꾼다
- 'p번째 차례가 돌아올때'를 코드로 구현 
- 종료 조건 : 한글자씩 말하기 때문에 무지가 말한 숫자들(String)의 길이가 구해야할 답 갯수 t가 되면 종료 

<br>

<br>



##### 🌊 로직 

1. answer Strin 선언 
2. 숫자 1씩 증가시키면서... 
3. 각 숫자 n 진수로 변환 
4. 해당 변환 숫자(String)을 한글자씩 읽으면서(index++) 무지 차례가 되면 (index==p) 해당 숫자 문자열 추가 
5. 답 길이가 t와 같아지면 종료 

<br>

<br>



##### 🎈Points ; N 진수 변환 

1. Integer.toString(i, n) ; 

i를 n진수로 변환해 String값으로 돌려준다. 

```java
String converted_num = Integer.toString(num, n) ;
```

<br>

2. 직접 코드 구현 

- 숫자(i)를 진수(n)으로 나눈 나머지를 역순으로 누적한다 
- 이때 나머지가 10 이상이면 A~F로 치환한다.
- 숫자(i)를 숫자/진수의 몫으로 치환한다. 
- 숫자(i)가 0이 되면 종료한다 

```java
 public static String toNDecimal(int n, int num){
        String result = "" ; 
        
        if(num == 0 || num == 1)
            return num+"" ; 
        
        while(num != 0){
            int turn = num % n ; 
            
            if(n > 10 && turn > 9){
                result = toAlpa(turn) + result ; 
            }else{
                result = turn + result ; 
            }
            num /= n; 
        }
        return result ; 
    }
    
    public static char toAlpa(int turn){
        switch(turn){
            case 10 : 
                turn = 'A' ; 
                break ; 
            case 11 : 
                turn = 'B' ; 
                break ; 
            case 12 : 
                turn = 'C' ; 
                break ; 
            case 13 : 
                turn = 'D' ; 
                break ; 
            case 14 : 
                turn = 'E' ; 
                break ; 
            case 15 : 
                turn = 'F' ; 
                break ; 
        }      
        return (char)turn ; 
    }
```

<br>

<br>



##### ✅ 코드 

>  최악의 경우 O(N)  : O(msg.length()) = O(1000)

일치하는 key값이 한글자 단위로만 있는 경우 해당 문자열의 길이만큼 한글자씩 사전을 갱신(O(1))한다 

```java
class Solution {
    public String solution(int n, int t, int m, int p) {
        
        int index = 1 ; 
        String answer  = "" ; 
            
        a : for(int num = 0 ; num < t*m ; num++){
            // 1. 각 숫자 n진수로 변환 
            String converted_num = toNDecimal(n, num) ;
             
            // 2. 한글자씩 말하면서
            for(int i = 0 ; i < converted_num.length() ; i++){
                        
                // 3. 만약 무지의 차례면 자기 숫자에 추가  
                if(index == p){ 
                    // index % m == p 가 안되는 이유 : m == p 인경우 해당 값은 절대 나올 수 없다(0). 
                    // 이걸 분기문으로 고려하기 어렵기 때문에 무지의 '순번' 자체를 다시 갱신해준다
                    answer += converted_num.charAt(i) ;  
                    
                    // 4. 무지의 '순번' 자체를 다시 갱신
                    p += m ;
                    
                    // 5. 만약 t개만큼 답 구했으면 종료 
                    if(answer.length() == t) break a;
                }
                index++ ; 
            } 
        }
        return answer ; 
    }
    
    public static String toNDecimal(int n, int num){
        String result = "" ; 
        
        if(num == 0 || num == 1)
            return num+"" ; 
        
        while(num != 0){
            int turn = num % n ; 
            
            if(n > 10 && turn > 9){
                result = toAlpa(turn) + result ; 
            }else{
                result = turn + result ; 
            }
            num /= n; 
        }
        return result ; 
    }
    
    public static char toAlpa(int turn){
        switch(turn){
            case 10 : 
                turn = 'A' ; 
                break ; 
            case 11 : 
                turn = 'B' ; 
                break ; 
            case 12 : 
                turn = 'C' ; 
                break ; 
            case 13 : 
                turn = 'D' ; 
                break ; 
            case 14 : 
                turn = 'E' ; 
                break ; 
            case 15 : 
                turn = 'F' ; 
                break ; 
        }      
        return (char)turn ; 
    }
}
```

<br>

<br>

