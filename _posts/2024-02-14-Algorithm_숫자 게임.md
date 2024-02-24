---
published: true
title: "Two-Pointer-숫자게임" 
categories: Algorithm 
tag: [algorithm, Programmers, Two-Pointer] 
toc: true
author_profile: false 


---



##### 문제

모든 사원이 무작위로 자연수를 하나씩 부여받고, 각 경기당 A팀에서 한 사원이, B팀에서 한 사원이 나와 서로의 수를 공개한다. 

그때 숫자가 큰 쪽이 승리하게 되고, 승리한 사원이 속한 팀은 승점을 1점 얻는다.

A팀이 빠르게 출전순서를 정했고 자신들의 출전 순서를 B팀에게 공개해버렸다. 

B팀은 그것을 보고 자신들의 최종 승점을 가장 높이는 방법으로 팀원들의 출전 순서를 정하려한다. 이때의 B팀이 얻는 승점은? 

<br>



##### 사용 로직 

  : 이길거면 가장 근소한 차이로 이기는게 낫다 + 질거면 가장 작은 수로 지는게 낫다

       1) 오름차순으로 각 배열 정렬 후 i 번째 자릿수 비교 
       2) B 배열 요소가 작으면 다음 요소와 비교 
       3) 배열을 반환하는게 아니므로 더 큰 요소가 나오기만 하면 answer++(실제론 swap해야함)
       4) 이렇게 하면 최종적으로 작은 요소가 가장 뒤로 감 

<br>



##### 구현(코드)

```java
import java.util.* ; 

class Solution {
    public int solution(int[] A, int[] B) {

        // 각 배열 오름차순 정렬
        Arrays.sort(A) ; 
        Arrays.sort(B) ; 
              
        // 각각의 pointer를 다르게 적용 
        int ptrA = 0 ; 
        int ptrB = 0 ; 
        int answer = 0 ;  

         while (ptrA < A.length && ptrB < B.length) { // 비교하는 pointer가 배열 밖으로 나가지 않을때  
            if(A[ptrA] < B[ptrB]){ // 비교 자릿수의 요소가 B 배열이 더 크면
                answer++ ;  // 승점 획득 

                // 포인터 증가(다음 비교 준비) : 각 배열의 다음 요소끼리 비교 
                ptrA ++ ; 
                ptrB ++ ; 
            }else{
                // 포인터 증가(다음 비교 준비): A의 동일 요소와 B 배열의 더 큰 요소와 비교
                ptrB ++ ; 
            }
        } // while 
        return answer ; 
    }
}
```

<br>



