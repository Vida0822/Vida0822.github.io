---
published: true
title: "Implementation(문자열)-K진수에서 소수 찾기" 
categories: Algorithm 
tag: [algorithm,Shortest Kruscal, String, 문자열, Implementation] 
toc: true
author_profile: false 
  
---



##### ❔ 문제

https://school.programmers.co.kr/learn/courses/30/lessons/92335

<br>



##### 💡 핵심 아이디어

> 구현

문제에 기재된 진행 과정 (1. 진수 변환, 2. 소수 counting)을 코드로 구현하는 것이 관건인 구현문제이다. 

진수 변환, 소수 카운팅 등 자주 사용되는 로직은 암기하는 것이 좋다.

복잡한 과정들을 각각의 과정 메서드로 분리하여 구조를 쉽게 표현한다. 

1. transform() : 숫자를 k진수로 변환하는 메서드 
2. getCount() : 해당 숫자에 포함된 소수 갯수를 구하는 코드 

<br>



##### Point

> 문자열 다루기 

진수 변환, 문자열 나누기, 문자-> 숫자 변환 등 여러 문자열 조작을 요구하는 문제다. 

n= 1,000,000로 매우 크기 때문에 오래걸리는 문자열 연산은 피해야한다. 

* StringBuilder - insert(0, n%k) : 문자열을 계속 누적(변경)하는 과정이기 때문에 String을 사용하면 매번 배열을 새로 구성하며, 앞에 붙여주는 코드를 따로 작성해야해서 비효울적이다. 따라서 StringBuilder를 사용해 인덱스 0번위치에 문자열을 Build함으로써 조작을 단순화한다.
* parseLong() : n이 매우 크기 때문에 특정 진수로 표현할때 어떤 숫자가 int이상의 값을 가질 수 있다. 따라서 int 가 아닌 long 으로 받아한다. 
* Math.sqrt() : 특정 수를 나누는 수는 해당 값의 제곱근보다 큰 수가 나올 수 없다. 따라서 자기 수 이전까지가 범위가 아닌 자신의 제곱근까지만 하나씩 나눠보면 된다. 

<br>  



##### ✅ 코드

```java
class Solution {
    public int solution(int n, int k) {
        // n -> k 
        // n = 1,000,000...so big - O(N) 
            
        // 1. transform n --> k's n (method 1)
        String s = transform(n, k) ; 
        
        // 2. count sosu... (method 2 )
        return getCount(s) ; 
    }
    
    public static String transform(int n, int k){
        
        StringBuilder ts = new StringBuilder() ; 
        while(n != 0){
            ts.insert(0, n%k) ; 
            n /= k ; 
        }
        return ts.toString() ; 
    }
    
    public static int getCount(String s){
    //    System.out.println(s.replaceAll("0", "#")) ; 
        String[] nums = s.split("0") ; 
        int count = 0 ; 
        
        for(String num : nums){
           if(num.equals(""))
                continue; 
            //System.out.print(num+" ") ; 
            long d = Long.parseLong(num );
            if(checkDecimal(d))
                count++ ; 
        }
        return count ; 
    }
    
    public static boolean checkDecimal(long num){  
        if(num==1)
            return false; 
        
        long a = (long)Math.sqrt(num) + 1; 

        // 3~ 
        for(int i = 2 ; i < a ; i++){
            if (num % i == 0) 
                return false;
        }
        return true; 
    }
}
```

<br>

