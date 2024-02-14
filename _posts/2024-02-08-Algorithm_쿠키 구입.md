---
published: true
title: "Two-pointer-쿠키 구입" 
categories: Algorithm 
tag: [algorithm, Programmers, Two-pointer] 
toc: true
author_profile: false 


---



```java
import java.util.* ;    
public class Solution{
    public int solution(int[] cookie){
		  int len = cookie.length;
		  int answer = 0;
  
		  for(int i = 0; i < len-1; i++) {
		    int left = i;
		    int right = i+1;
		    int lsum = cookie[left];
		    int rsum = cookie[right];
    
		    while(true) {
		      if (lsum == rsum && answer < lsum) {
		        answer = lsum;
		      } else if (lsum <= rsum && left != 0) {
		        lsum += cookie[--left];
		      } else if (rsum <= lsum && right != len-1) {
		        rsum += cookie[++right];
		      } else {
		        break;
	      }
	    }
	  }  
	  return answer;
	}
}
```

- 배운 것

  - 

  [Untitled](http://44.com)

  

<details>
<summary><b> 배운 것 </b></summary>
<div markdown="1">

1. **핵심 아이디어** : ‘모든’ 경계를 기준으로 양쪽 방향의 순열의 합이 같아지는 경우의 최대 값을 구하는 문제



2. **알고리즘 1: 완전 탐색** —> 외부 반복문

- 완전탐색 : 양방향 누적합이 같아지더라도 더 큰 경우가 있을 수 있기 때문에 배열의 마지막 요소까지 경계점으로 고려해서 최대합을 구해야함
- 순차탐색 : 정렬되어있지 않기때문에 순차탐색 (배열의 첫 요소부터 검사 시작)



3. **알고리즘 2: 투포인터** —> 내부 반복문

- 각 경계점마다 순열의 합이 같아지는지, 그 합이 얼만지 확인
- 두 형제의 바구니는 연속해야하기 때문에 경계점은 하나라는게 포인트
- 누적합을 비교해가면서 포인터 확대하다가, 각각의 포인터가 배열의 양끝에 도달할 경우 해당 경계점은 pass
- 주의: 해당 경계점에 대한 누적 합이 같게 나오더라도 탐색을 종료해선 안됨. 계속 확대 하다 보면 더 큰 누적합 값이 나올 수 있기 때문



4. **유사한 문제** : ‘가장 긴 펠린드롬’  ([문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/12904)) 

</div>
</details> 
