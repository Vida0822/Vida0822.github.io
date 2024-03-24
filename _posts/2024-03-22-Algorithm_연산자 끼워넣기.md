---
published: true
title: "DFS-연산자 끼워넣기" 
categories: Algorithm 
tag: [algorithm, Programmers, DP] 
toc: true
author_profile: false 
  
---



##### 문제

N개의 수로 이루어진 수열 A1, A2, ..., AN이 주어진다. 또, 수와 수 사이에 끼워넣을 수 있는 N-1개의 연산자가 주어진다. 

연산자는 덧셈(+), 뺄셈(-), 곱셈(×), 나눗셈(÷)으로만 이루어져 있다.

우리는 수와 수 사이에 연산자를 하나씩 넣어서, 수식을 하나 만들 수 있다. 이때, 주어진 수의 순서를 바꾸면 안 된다.

<br>



##### 핵심 아이디어

> '가능한 모든 조합을 만드는' DFS

가능한 모든 연산자 조합을 찾는 것, 즉**'순열'**을 만드는 것이 관건이다. 

해당 연산자를 찾으며  함께 value값에 nums 요소들을 계산 & 누적해나가고 

연산자를 다 사용했을 때 누적 값을 최소, 최대로 갱신한다. 

<br>



##### Points

> DFS + 넣고 빼기 트릭 

순열을 만들 땐 DFS + 넣고 빼기 트릭을 사용한다. 

1. 특정 요소 투입 (ex) +)
2. dfs 수행 : 해당 특정 요소까지 확정, 그 이후의 순열값을 계산
3. 특정 요소 빼고 다른 요소 넣어서 조합 구하기 

<br>

이 트릭을 사용하기 위해 **DFS 인자로 1. depth와 2. 현재까지의 누적 value**를 재귀적으로 넘겨줘야 한다. 

종료 조건으로 depth가 기준 값에 달성하였거나 value가 특정 조건을 만족할 때 재귀를 종료한다. 

<br> 

##### 유사한 문제

[DFS-여행경로 | 희민이의 개발노트 (vida0822.github.io)](https://vida0822.github.io/algorithm/Algorithm_여행경로/)

<br>



##### Tip 

재귀적 구조의 코드에서, 재귀적으로 호출할 때마다 달라지는 값이라면 인자로 넘겨주고, 

어디서든 동일한 값을 유지한다면 static 변수로 선언해준다

<br>

 

##### 코드

```java
import java.util.*; 
class Main{
    public static int n ; 
    public static ArrayList<Integer> arr = new ArrayList<>() ; 
    public static int add, sub, mul, div ; 
    public static int minValue = (int)1e9; 
    public static int maxValue = (int)-1e9 ; 
    
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in); 
        n = sc.nextInt() ; 

        // 숫자(n개)
        for(int i = 0 ; i < n ; i++){
            arr.add( sc.nextInt()) ; 
        }
        
        // 연산자 (n-1)개 
        add = sc.nextInt() ; 
        sub = sc.nextInt() ; 
        mul = sc.nextInt() ; 
        div = sc.nextInt() ; 
        
        // DFS 호출 
        dfs(1, arr.get(0)) ; 
        
        // 최댓값, 최솟값 출력 
        System.out.println(maxValue) ; 
        System.out.println(minValue) ; 
    }
    
    
    public static void dfs(int i, int now){ // 자릿수, 현재까지 계산값
        // 모든 연산자를 다 사용한 경우, 최솟값&최댓값 업데이트
        if(i == n){
            minValue = Math.min(minValue, now); 
            maxValue = Math.max(maxValue, now); 
        }else{
            // ** 구조 암기 : 순열 만들기 
            if(add > 0){
                add -= 1 ; 
                dfs(i+1, now+arr.get(i)) ; 
                add += 1 ; 
            }
            if(sub > 0){ 
                // else if 가 아니기 때문에 add if문이 종료되면 같은 자릿수에서 sub가 실행된다.
                // 즉 모든 4개의 분기문이 남은 연산자가 없을 때까지 실행된다.
                sub -= 1 ; 
                dfs(i+1 , now - arr.get(i)) ; 
                sub += 1;  
            }
            if(mul > 0){
                mul -= 1;
                dfs(i+1 ,now*arr.get(i)) ; 
                mul += 1; 
            }
            if(div > 0){
                div -= 1 ; 
                dfs(i+1, now/arr.get(i)) ; 
                div += 1 ;
            }
        }
    }
}
```

<br>





##### 입력 개선 코드 

BufferedReader와 BufferdWriter는 버퍼를 사용하여 읽기와 쓰기를 하는 함수로 Scanner보다 훨씬 성능이 빠르다! 

입출력 작업이 많은 경우 BufferedReader/Writer를, 간단할 경우 쓰기 편한 Scanner을 사용한다! 

```java
import java.util.* ; 
import java.io.* ; 

public class Main{
    public static int N ; 
    public static int[] number ; 
    public static int[] operator = new int[4] ; 
    public static int MAX = Integer.MIN_VALUE ; 
    public static int MIN = Integer.MAX_VALUE ; 
    
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in)) ;
        N = Integer.parseInt(br.readLine()) ; 
        number = new int[N] ; 
        
        // 숫자 입력받기 
        StringTokenizer st = new StringTokenizer(br.readLine(), " ") ; 
        for(int i = 0 ; i < N ; i++){
            number[i] = Integer.parseInt(st.nextToken()) ; 
        }
        
        // 연산자 입력받기 
        st = new StringTokenizer(br.readLine(), " ") ; 
        for(int i = 0 ; i < 4 ; i++){
            operator[i] = Integer.parseInt(st.nextToken()) ; 
        } 
        // 연산자 끼워넣기 
        dfs(number[0], 1) ; // 재귀 내에서 계산하면서 max, min값 동시에 갱신 
        
        // 최대, 최솟값 출력 
        System.out.println(MAX) ; 
        System.out.println(MIN) ; 
    }
    
    public static void dfs(int num, int idx){ // 누적 계산값, 다음 숫자값  
        if(idx == N){ // 다 끼워넣었을 떄 
            // static 갱신 
            MAX = Math.max(MAX, num) ; 
            MIN = Math.min(MIN, num) ; 
            return ; 
        }
        
        for(int i = 0 ; i < 4 ; i++){ // 연산자 하나씩 돌면서 
            if(operator[i] > 0){
                operator[i]-- ; 
                switch(i){
                    case 0 : dfs(num + number[idx], idx+1) ; break ; 
                    case 1 : dfs(num - number[idx], idx+1) ; break ; 
                    case 2 : dfs(num * number[idx], idx+1) ; break ; 
                    case 3 : dfs(num / number[idx], idx+1) ; break ; 
                }
                operator[i]++; 
            }
        }   
    }
}
```

