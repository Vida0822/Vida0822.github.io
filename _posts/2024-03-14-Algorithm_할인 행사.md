---
published: true
title: "Two Pointer-할인행사" 
categories: Algorithm 
tag: [algorithm, Programmers, Two Pointer] 
toc: true
author_profile: false 
  
---



##### 문제

정현이는 자신이 원하는 제품 종류와 각 수량이 할인하는 날짜와 10일 연속으로 일치할 경우에 맞춰서 회원가입을 하려 합니다.

정현이가 원하는 제품을 나타내는 문자열 배열 `want`와 정현이가 원하는 제품의 수량을 나타내는 정수 배열 `number`, XYZ 마트에서 할인하는 제품을 나타내는 문자열 배열 `discount`가 주어졌을 때, 회원등록시 정현이가 원하는 제품을 모두 할인 받을 수 있는 회원등록 날짜의 총 일수를 return하세요.



<br>

##### 핵심 아이디어 : 슬라이딩 윈도우

> ‘연속’된 일수가 10일로 ‘고정’ 되어있다.

- 처음 10일에 포함되는 상품명, 갯수 계산
- 해당 10일을 기준으로 한칸씩 이동해가며(왼쪽—, 오른쪽++)
- 그 10일에 포함되는 세일품목구성(상품&갯수) 갱신하고
- 해당 세일 품목 구성이 사용자의 요구를 만족하는지 확인 (메서드 분리)

<br>



##### 시간 복잡도

> O(N)        N = discount.length

- 10일을 기준으로 discount 배열에서 한칸씩 이동하며 순회하므로 O(discount.length)

- 그 과정에서 사용자 니즈를 충족하는지 확인하기위해 want&number 배열을 순회하므로 O(want.length)

- 그러나 want 배열은 최대 크기가 10이므로 O(discount.length * want.length) 의 최대값은 O(10*discount.length),  즉 O(N)이다

  <br>

  

##### Point

‘각 타입에 해당하는 갯수‘ 는 Map형태의 자료형을 사용하는 것이 편리하다.

추가, 삭제 연산시 key값이 있을 때, 없을때의 갯수 설정을 잘해주는 것이 포인트다.

<br>



##### 코드

```java
import java.util.* ; 
class Solution {
    public int solution(String[] want, int[] number, String[] discount) {
        
        Queue<String> q = new LinkedList<>() ;    
        HashMap<String, Integer> count = new HashMap<>() ;
        int answer = 0 ; 

				// 초기설정 (첫 10일) 
        for(int i = 0 ; i < 10 ; i++){
            String sale = discount[i] ; 
            q.offer(discount[i]) ; 
            count.put(sale, count.getOrDefault(sale, 0)+1) ; 
        }
				// 품목 구성이 사용자 니즈 만족하는지 확인
        if(allDiscounted(want,number,count))
            answer++ ; 
        
				// 슬라이딩 윈도우 (10일 고정 왼,오 조정)
        for(int i = 10 ; i < discount.length ; i++){
            String fDay = q.poll() ; 
            count.put(fDay, count.getOrDefault(fDay, 1) -1) ; 

            String lDay = discount[i] ; 
            q.offer(lDay) ; 
            count.put(lDay, count.getOrDefault(lDay, 0) +1) ; 
            
						// 조정된 품목 구성이 사용자 니즈 만족하는지 확인
            if(allDiscounted(want, number, count))
                answer++ ; 
        }
        return answer ; 
    }
    
    public boolean allDiscounted(String[] want, int[] number, HashMap<String,Integer> count){
        for(int i = 0 ; i < want.length ; i++){
            String name = want[i] ; 
            int num = number[i] ; 
            
            if(num > count.getOrDefault(name, 0))
                return false ; 
        }
        return true ; 
    }
}
```

<br>



##### 슬라이딩 윈도우 ?

윈도우 (특정 범위)가 있을 때, 윈도우 내부 요소의 값을 이용하여 문제를 풀이하는 알고리즘이다. 배열이나 문자열같은 선형 구조에서 ’일정 범위 값‘을 비교할 때 유용하다.

투포인터 패턴과 유사하나 주로 범위가 고정되어있다는 점이 다르다.

이중 반복문을 통해 구현해야할 문제를 O(N)의 시간복잡도로 해결 할 수 있다.

패턴의 형식은 다음과 같다

1. 초기화 : window에 0번 인덱스부터 k번 인덱스(윈도우 범위)까지 합을 저장한다
2. k 번 인덱스부터 탐색: 범위 내 첫번째 수(현재 인덱스의 -k한 인덱스) 다음 루프의 첫번째 수(k번 인덱스)를 더해준다.
3. 문제에 따른 조건검사, 연산을 한다

<br>

유의할 점은, 객체, 엔트리 형태라면 Queue, List와 같은 자료구조를 사용해 window를 직접 구현해도 되지만, 단순히 값만 조회하는 거라면 index를 설정해 직접 조회한다.

이를 반영해 코드를 수정하면 다음과 같다.

<br>

