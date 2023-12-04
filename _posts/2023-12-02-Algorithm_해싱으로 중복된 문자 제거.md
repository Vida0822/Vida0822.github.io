---
published: true
title: "[알고리즘] 해싱 - 중복된 문자 제거" 
categories: 알고리즘 
tag: [algorithm, 프로그래머스, BinarySearch, Hash ] 
toc: true
author_profile: false 
---



### 8번. 중복된 문자 제거

---

##### 문제 : 중복된 문자 제거 (LEVEL 1)



##### 내 풀이 : Hashing 이용 

주어진 문자열을 차례로 읽으며 

처음 등장하는 문자는 Hash Table에 기록 

=> 다음에 똑같은 문자가 등장했을 땐 포함 x 

==>  중복을 허용하지 않는 HashSet 이용 

```java
package hashing ; 

import java.util.* ; 

class 중복된문자제거 {
    public String solution(String my_string) {
        String answer = "";
        HashSet<String> hashSet = new HashSet() ; 
        
        for(int i = 0  ; i < my_string.length() ; i++ ){
            String ch = ""+ my_string.charAt(i); 

            if(!hashSet.contains(ch)){
                hashSet.add(ch) ; 
                answer = answer + ch ; 
            }
        }
        return answer;
    }
}
```



---

##### 개선 코드 1 : LinkdedHashSet

※ 위의 코드 개선 필요 

: 이 코드에선 정답 문자열과 hash table을 따로 다루면서 

정답 문자열에 포함하기 위해서 매번 해시테이블에서 검사하는 문자가 있는지 확인해야함 

=> 중복값을 허용하지 않는 HashSet의 장점 전혀 활용 x 

+ 순서 또한 유지되지 않으니 따로 문자열에 쌓아줘야함 (오히려 불편) 

=> 중복값 검사는 난 전혀 신경쓰지 않고 값을 넣으면 알아서 걸러지는 기능 + 집어넣은 순서대로 유지되는 자료구조 필요 

* LinkdedHashSet  : *입력 순서를 유지하면서* 중복을 허용하지 않는 Set 구현체  

```java
import java.util.*;
class Solution {
    public String solution(String my_string) {
        String[] answer = my_string.split(""); // 문자 단위로 나눠서 
        Set<String> set = new LinkedHashSet<String>(Arrays.asList(answer));
        // LinkedHashSet(Collection)

        return String.join("", set); // join(구분자, Iterable elements) 
    }
}
```







---

##### 개선 코드 2 : Stream 

```java
import java.util.* ; 
import java.util.stream.*; 
 
public class 중복된문자제거_Stream {
	public String solution(String my_string) {
        
		return my_string.chars() // String의 각 문자의 아스키 코드 값을 요소로 가지는 IntStream 반환 
				.mapToObj(Character::toString)
				/* Character 클래스의 toString으로 해당 아스키 코드 값들을 각각 문자열로 변환  
					=> 각 문자가 요소값인 String 스트림 생성 
					※ 바로 저 String을 Stream으로 변환하지 않은 이유 
						: 문자 단위로 분리되는게 아니라 저 String 자체가 하나의 요소값이 됨
						==> 문자단위로 먼저 잘라 그걸 요소값으로 갖는 스트림을 만들고  - chars() 
								그걸 각각 문자열로 변환해줘야함 
				*/
				.distinct()  // 중복된 문자 제거 
				.collect(Collectors.joining()) ; // 스트림의 요소들을 다 연결해 하나의 문자열 반환 
	}
}
```

