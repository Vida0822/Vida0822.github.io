---
published: true
title: "Two-Pointer-보석쇼핑" 
categories: Algorithm 
tag: [algorithm, Programmers, Two-Pointer] 
toc: true
author_profile: false 


---



```java
import java.util.*;
class Solution {
    public int[] solution(String[] gems) {
        int kind = new HashSet<>(Arrays.asList(gems)).size(); // 4

        int[] answer = new int[2]; 
        int length = Integer.MAX_VALUE ; // 가장 짧은 길이의 부분 배열의 길이를 저장하기 위한 변수
        int L = 0, R = 0 ;  // two-pointer 알고리즘
        Map<String, Integer> map = new HashMap<>(); // 보석명, 등장횟수 
        
        for (R = 0; R < gems.length; R++) { // 오른쪽 포인터(R)를 이동하면서 
            map.put(gems[R], map.getOrDefault(gems[R], 0) + 1); // 해당 보석의 등장 횟수를 업데이트

            while (map.get(gems[L]) > 1) { // 두번 이상 등장하는 보석이 생겼을 때
                map.put(gems[L], map.get(gems[L]) - 1); // 보석의 등장 횟수가 1 이상인 경우에는 해당 보석의 등장 횟수를 감소 (가장 왼쪽에 있는 보석을 제거)
                // 필수 등장 횟수인 1을 유지 
                L++; // 왼쪽 포인터(L)를 이동
            }
            if (map.size() == kind // 맵의 크기가 보석의 종류 수와 동일하고
                && length > (R - L)) { //  현재 부분 배열의 길이가 기존의 최소 길이보다 작을 경우
                length = R - L; // 최단 배열로 갱신 
                answer[0] = L + 1;
                answer[1] = R + 1; // answer 배열을 업데이트
            }
        }
        return answer;
    }
}
```



- two-pointer Algorithm: 1차원 배열에서 각자 다른 원소를 가리키고 있는 2개의 포인터를 조작해가면서 원하는 값을 찾을 때 까지 탐색하는 알고리즘

  —> 2차 반복문을 대체할 수 있어 O(N) 으로 수행 가능

- Map은 Collections의 상속 클래스가 아니기 때문에 Map의 고유 메서드 따로 암기 필요

  ex) contains : X → containsKey / getOrDefault(key)  / iterator 사용 불가 —> entrySet(), keySet() 등으로 변환 후 사용 …

- 단순히 특정 값 집합의 종류를 구할땐 일일히 HashSet에 요소를 넣는게 아닌 컬렉션 인터페이스 이용해서 바로 HashSet 매개변수롤 넣어주고 size 바로 뽑아주기 (한줄로 코드 압축)
