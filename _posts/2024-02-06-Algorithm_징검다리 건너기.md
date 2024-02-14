---
published: true
title: "BinarySearch-징검 다리 건너기" 
categories: Algorithm 
tag: [algorithm, Programmers, BinarySearch] 
toc: true
author_profile: false 


---



##### 1. 연결 리스트 —> O(N logN)

```python
from collections import defaultdict // Map 

def solution(stones, k):
    n = len(stones)
    life_2_idx = defaultdict(list)
    for idx, life in enumerate(stones, start=1):
        life_2_idx[life].append(idx)
    my_front, my_back = [*range(-1, n+1)], [*range(1,n+3)]

    for life in sorted(life_2_idx):
        for idx in life_2_idx[life]:
            if my_back[idx]-my_front[idx] > k:
                return stones[idx-1]
            my_back[my_front[idx]] = my_back[idx]
            my_front[my_back[idx]] = my_front[idx]
```



##### 2. 이분 탐색 —> O(N)

```java
import java.util.Arrays;
public class Solution {
    public int solution(int[] stones, int k) { // 건널 수 없는 돌의 연속된 개수
        int answer = 0;

				// 1) part 1. 인원수 범위 줄여나가기 
				// 수용가능 인원수를 도출하기 위한 배열은 가공해도 괜찮 
        int[] copy = Arrays.copyOf(stones, stones.length);
        Arrays.sort(copy);

        int low = 1; // 가능한 최소 인원 수
        int high = copy[copy.length - 1]; // 가능한 최대 인원 수 (최대 디딤돌 횟수만큼) 

        while (low <= high) {
            int mid = (low + high) / 2; // 이진탐색 --> 인원수 범위 줄여나가기 
            if (isPossible(stones, k, mid)) { // 해당 인원수로 건널 수 있으면 
                low = mid + 1; // 오른쪽 범위 탐색 (더 큰 인원수로 가능여부 검사) 
            } else { // 건널 수 없으면 
                high = mid - 1; // 왼쪽 범위 탐색 (더 작은 인원수로 가능여부 검사) 
            }
        }
        answer = low; // 마지막엔 low와 high가 엇갈리므로 low가 최댓값을 의미 
        return answer;
    }

		// part 2. 해당 인원수로 건널 수 있는지 검사  
		// 돌 자체는 '연속성'이 중요하기 때문에 매개변수로 받은 배열을 그대로 씀 
    public boolean isPossible(int[] stones, int k, int mid) {
        int count = 0; // 건널 수 없는 돌의 연속횟수 
        for (int i = 0; i < stones.length; i++) {
            if (stones[i] - mid <= 0) { // 현재 인원 수(mid)로 해당 돌을 건널 수 없으면 
                count++; // count에 누적 
            } else { // 건널 수 있으면 
                count = 0; // 연속되지 않으므로 count 초기화 (연속된 개수가 k를 초과하는지 확인)
            }
            if (count >= k) { // 건널 수 없는 돌의 연속된 갯수가 k개 초과 
                return false; // 해당 인원수로는 건널 수 없음 
            }
        }
        return true; // 현재 인원 수(mid)는 건널 수 있음 
    }
}
```



##### 3. 슬라이딩 윈도우

```java
import java.util.*;
class Data{
   int value, index;
   public Data(int value, int index) {
      this.value = value; // 디딤돌 횟수 
      this.index = index; // 인덱스 
   }
}
class Solution {

   public static int solution(int[] stones, int k) {
      
      ArrayDeque<Data> q = new ArrayDeque<>();

			int answer = Integer.MAX_VALUE;
      for(int i = 0; i < stones.length; i++) {
         int cur = stones[i];
         if(!q.isEmpty() && q.peek().index <= i - k) {
            q.pollFirst();
         }
         while(!q.isEmpty() && q.peekLast().value <= cur) {
            q.pollLast();
         }
         q.add(new Data(cur, i));
         if(answer >= q.peekFirst().value && i >= k - 1) {
            answer = q.peekFirst().value;
         }
      }

      return answer;
   }
}
```



<details>
<summary><b> 배운 것 </b></summary>
<div markdown="1">

1. 슬라이딩 윈도우 알고리즘

- Queue + 투 포인터 이용
- 고정 사이즈의 윈도우가 이동하면서 윈도우 내에 있는 데이터를 이용해 문제를 풀이하는 알고리즘
- **교집합의 정보를 공유하고, 차이가 나는 양쪽 끝 원소, 그에 따른 통계값(반환값)만 갱신**
- 투포인터를 이용하지만, **항상 구간의 넓이가 고정되어 있다는 차이점 존재**

2. Deque (interface)

- 구현 클래스 : ArrayDeque, LinkedBlockingDeque, ConcurrentLinkedDeque, LinkedList
- addFirst(), offerFirst() / removeFirst(), pollFirst() / peekFirst()
- addLast(), offerLast() / removeLast(), pollLast() / peekLast()
- pop(), push() : deque를 stack으로 사용할 때

</div>
</details> 
    
</br>
