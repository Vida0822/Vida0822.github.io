---
published: true
title: "Math-특정 숫자를 n진수로 변환" 
categories: Algorithm 
tag: [algorithm,Math, Binary, Octal, Hex] 
toc: true
author_profile: false 
  
---



##### 🎈N 진수 변환 

1. Integer.toString(i, n) ; 

i를 n진수로 변환해 String값으로 돌려준다. 

```java
String converted_num = Integer.toString(num, n) ;

Integer.toBinaryString(a); // 2진수 변환 
Integer.toOctalString(a); // 10진수 변환 
Integer.toHexString(a) ; // 16진수 변환 
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



