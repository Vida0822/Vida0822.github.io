---
published: true
title: "Sort-직접 기준 정렬하기" 
categories: Algorithm 
tag: [algorithm, Programmers, 정렬, 직접기준정렬, compare ] 
toc: true
author_profile: false 

---



### 4번. 정렬 - 직접 기준 정하기 

---

##### 문제 : 가장 큰 수 (Level 1)

주어진 정수가 [6, 10, 2]라면 [6102, 6210, 1062, 1026, 2610, 2106]를 만들 수 있고, 이중 가장 큰 수는 6210입니다.

0 또는 양의 정수가 담긴 배열 numbers가 매개변수로 주어질 때, 순서를 재배치하여 만들 수 있는 가장 큰 수를 문자열로 바꾸어 return 하도록 solution 함수를 작성해주세요.

* 정답이 너무 클 수 있으니 문자열로 바꾸어 return 합니다



##### 세운 로직 

* 숫자 구성요소 뽑기 
* 문자 조합 만들기 (순열 만들기)
* 만든 문자조합 숫자로 바꾸기  
* 오름차순 정렬 후 최대값 반환하기



##### 내 풀이   ❌ (못품)

```java
package sorting;

import java.util.ArrayList;
import java.util.Arrays;

public class 가장큰수 {

	public String solution(int[] numbers) {
        String answer = "";
        
        // part 1. 숫자 구성요소 뽑기  
        ArrayList<Integer> sums = new ArrayList<>() ; 
        
        String s1 = "" ; 
        for(int i = 0 ; i < numbers.length ; i++){         
           s1 += numbers[i]+"" ; 
        } // i   
        String[] stringNumber = s1.split(' ') ;  // 6 , 1, 0 , 2 
       
        // part 2. 문자 조합 만들기 (순열 만들기)
        
       
        // part 3. 만든 문자조합 숫자로 바꾸기  
        Integer[] answerArray = sums.toArray(new Integer[0]) ;  
        
        // part 4. 오름차순 정렬 후 최대값 반환하기 
        Arrays.sort(answerArray) ;
        int[] answers = Arrays.stream(answerArray)
            .mapToInt(Integer::intValue).toArray();

        return answers[answers.length-1]+"";
    }

}

```



##### 좋은 모범 로직: 자바 비교기 사용 1 (diff)

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

class Solution {
    public String solution(int[] numbers) {
        String answer = "";

        List<Integer> list = new ArrayList<>();
        for(int i = 0; i < numbers.length; i++) {
            list.add(numbers[i]);
        }
        Collections.sort(list, (a, b) -> {
            String as = String.valueOf(a), bs = String.valueOf(b);
            return -Integer.compare(Integer.parseInt(as + bs), Integer.parseInt(bs + as));
        });
        StringBuilder sb = new StringBuilder();
        for(Integer i : list) {
            sb.append(i);
        }
        answer = sb.toString();
        if(answer.charAt(0) == '0') {
            return "0";
        }else {
            return answer;
        }
    }
}
```



##### 좋은 모범 로직: 자바 비교기 사용 2 (easy)

````java
import java.util.Arrays;
import java.util.Comparator;

class Solution {
    public String solution(int[] numbers) {
        String[] nums = new String[numbers.length];

        for (int i=0; i<nums.length; i++) 
            nums[i] = numbers[i] + "";

        Arrays.sort(nums, new Comparator<String>() {
            public int compare(String o1, String o2) {
                return (o2 + o1).compareTo(o1 + o2);
            }
        });

        String ans = "";
        for (int i=0; i<numbers.length; i++)
            ans += nums[i];

        return ans.charAt(0) == '0' ? "0" : ans;
    }
}
````

