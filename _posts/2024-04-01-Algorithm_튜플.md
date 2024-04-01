---
published: true
title: "Implementation-튜플" 
categories: Algorithm 
tag: [algorithm, Programmers, BFS] 
toc: true
author_profile: false 
  
---



##### 문제

https://school.programmers.co.kr/learn/courses/30/lessons/64065

<br>



##### 핵심 아이디어

> 문자열 다루기, 특히 정규표현식  

이 문제의 핵심은 규칙성 파악(문제 이해)과 문자열 가공이다. 

<br>



##### 규칙성 

규칙성은 다음과 같이 두가지 방향으로 이해할 수 있다. 

집합 내에선 원소의 순서가 바뀔 수 있다는 유의사항을 염두해야한다. 

1. 등장횟수가 많은 원소 순으로 튜플이 구성되어있다. 

   ex) {2} , {2, 1, 3} , {2, 1} -> 2,1,3 순으로 등장횟수가 많으므로 튜플은 [2,1,3] 이다.

2. 길이가 짧은 튜플일수록 가장 먼저 나와야할 원소를 포함하고 있다. 

   ex) {1,2,3},{2,1},{1,2,4,3},{2}

    가장 짧은 {2} -> [2]

    다음 {2,1 } -> [2, 1]

    다음 {1,2,3} -> 없는 숫자 삽입 [2,1,3]

    다음 {1,2,4,3} -> 없는 숫자 삽입 [2,1,3,4]

<br>



##### 문자열 가공 

튜플 구성이 한 문자열로 전달되므로 이를 각각의 튜플로 변환, 인식하는 과정이 필요하다. 

따라서 replace("정규식")와 split()을 적절히 사용해 각각의 튜플을 뽑아낸다

1. 숫자가 아닌 문자 제거 : '{' ,  '}'를 없애준다

   : 이 때 },{ 단위로 튜플을 분리해야하므로 공백(" ") 이나 "-" 등으로 대체한다 (replace())

2. 맨 끝 공백을 없애준다 : 맨 끝 공백은 분리기준으로도, 검사 대상도 아니다 
3. 대체한 공백 또는 "-"로 나눠주면 각각의 튜플이 나온다
4. 각 튜플을 검사하며 "," 단위로 구분해 해당 튜플의 원소를 하나씩 검사한다. 

<br> 

##### 코드

```java
import java.util.*; 
class Solution {
    public int[] solution(String s) { // 5 이상 1,000,000
        HashMap<Integer, Integer> map = new HashMap<>() ; 
        String[] tuples = s.replaceAll("[{]"," ").replaceAll("[}]", " ").trim().split(" , ") ; 
        for(String tuple : tuples){ // O(1,000,000) 
            String[] nums = tuple.split(",") ; 
            
            for(int i = 0 ; i < nums.length ; i++){
                int num = Integer.parseInt(nums[i]) ; 
                map.put(num, map.getOrDefault(num, 0)+1) ; 
            }          
        }
        
        // value 값으로 정렬 (등장횟수가 많은 순으로 튜플 구성)
        List<Integer> keySet = new ArrayList<>(map.keySet()) ; 
        keySet.sort(new Comparator<Integer>(){
            @Override
            public int compare(Integer o1, Integer o2){
                return map.get(o2).compareTo(map.get(o1)); 
            }
        });
        
        // 답 변환 : Map -> Array 
        int[] answer = new int[keySet.size()] ; 
        for(int i = 0 ; i < keySet.size() ; i++){
            answer[i] = keySet.get(i) ; 
        }
        return answer ; 
    }
}
```

<br>



##### 개선

> 짧은 튜플부터 검사해 처음 등장하는 숫자가 있다면 답에 순차적으로 포함한다. 

위의 코드 튜플을 주어진 순서대로 검사하면서  <원소, 등장 횟수>의 Map형태로 저장한 후 등장횟수(value)값을 기준으로 정렬하는 코드이다. 다만 이렇게 하면 코드의 길이가 길어지고 복잡해진다. 

이를 개선하기 위해 2번 규칙성(길이가 짧은 튜플일수록 가장 먼저 나와야할 원소를 포함하고 있다. ) 를 활용해 다음과 같이 코드를 수정할 수 있다. 

<br>

1. Map 대신 Set 활용 **

```java
Set<String> set = new HashSet<>();
String[] arr = s.replaceAll("[{]", " ").replaceAll("[}]", " ").trim().split(" , ");
// 길이가 짧은 튜플순으로 정렬 **
Arrays.sort(arr, (a, b)->{return a.length() - b.length();});

int[] answer = new int[arr.length]; // 튜플의 갯수와 원소 종류의 갯수는 같다 
int idx = 0;
for(String s1 : arr) {
    for(String s2 : s1.split(",")) {
        if(set.add(s2)) // 이미 있는 숫자가 아니면(삽입 성공으로 true)  
            answer[idx++] = Integer.parseInt(s2); // 답에 추가하고 다음 답이 들어갈 index 증가 
    }
}
return answer;
```

이렇게 하면 Map 정렬 코드를 수행할 필요 없이 처음 튜플 검사할 때 답 배열을 완성할 수 있다. 

<br>





2. 문자열 전체 가공 완료 후 List 정렬 

```java
public ArrayList<Integer> solution(String s) {
        
        // 1. 튜플을 만들 ArrayList 객체.
        ArrayList<Integer> answer = new ArrayList<>();
        // 2. 가장 앞의 {{ 를 제거한다.
        s = s.substring(2,s.length());
        // 3. 가장 뒤의 }} 를 제거한 뒤, },{ 형태의 문자열을 -로 바꾼다.
        s = s.substring(0,s.length()-2).replace("},{","-");
        // 4. 위에서 바꾼 문자열을 기준으로 split 해준다.
        String str[] = s.split("-");        
        // 5. 나눠진 문자열 배열을 길이에 따라 다시 정렬한다.
        Arrays.sort(str,new Comparator<String>(){
            public int compare(String o1, String o2){
                
                return Integer.compare(o1.length(), o2.length());
            }
        });
        
        // 6. 각 문자열을 탐색한다.
        for(String x : str){
            // 7. 한 문자열마다 ,를 기준으로 split하여 새로운 문자열 배열을 만든다.
            String[] temp = x.split(",");
            // 8. 새로만든 문자열 배열에는 정수값만 존재하며 이를 탐색한다.
            for(int i = 0 ; i < temp.length;i++){
                // 9. 각 문자열 값을 정수로 바꾼다.
                int n = Integer.parseInt(temp[i]);
                // 10. 튜플에 들어있는 값이 아니라면 추가해준다.
                if(!answer.contains(n))
                    answer.add(n);
            }
        }
        
        return answer;
    }
```



<br>

