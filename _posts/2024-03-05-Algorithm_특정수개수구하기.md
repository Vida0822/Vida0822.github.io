---
published: true
title: "Binary Search-특정 수 개수 구하기" 
categories: Algorithm 
tag: [algorithm, Programmers, Sort, upperbound, lowerbound] 
toc: true
author_profile: false 
  
---



##### 문제 

n개의 원소를 포함하고 있는 수열이 오름차순으로 정렬되어 있을 때, 숫자 x가 등장하는 횟수는?

단 시간복잡도를 O(log N)으로 처리해야한다 (N <= 1,000,000)

<br>



##### 핵심 아이디어 

> 이분탐색(Boundary 테크닉)

* 매우 큰 N 값 --> 순차탐색 불가능
* **upper Bound & lower Bound 테크닉 ** : 각각 이진탐색 후 반환된 index들 활용

<br>





##### 코드 (Java)

```java
package sorting;
import java.util.Scanner;

public class 특정수개수구하기 { // O(N log N)으로 
	
	// 찾는 값 target이 처음 나오는 index
	public static int lowerBound(int[] arr, int target, int start, int end) {
		int found = 0 ; 
		while(start<end) {
			int mid = (start+end)/2 ; 
			if(arr[mid] >= target) {				
				found = mid; 
				end = mid ; 
			}
			else // 중간값이 target보다 작으면 
				start = mid+1 ;  // 오른쪽 탐색 --> target과 처음으로 같아지는 중간값 반환
		}
		return found ; // arr[mid] >= target && start == end
	}
	
	// 찾는 값 target 보다 커지는 값의 index 
	public static int upperBound(int[] arr, int target, int start, int end) {
		int found = 0 ; 
		while(start<end) {
			int mid = (start+end)/2 ;
			if(arr[mid] > target) {
				found = mid ;				
				end = mid ;
			}else // 중간값이 target 값보다 작거나 같은 경우  
				start = mid+1 ;  // 오른쪽 탐색 --> target값보다 처음으로 커지는 중간값 반환  
		}
		return found ; // arr[mid]>target && start == end 
	}

	public static int countByRange(int[] arr, int leftValue, int rightValue) {
		int rightIndex = upperBound(arr, rightValue, 0, arr.length) ; 
		int leftIndex = lowerBound(arr, leftValue, 0, arr.length) ; 
		return rightIndex - leftIndex ; 
	}
	
	public static void main(String[] args) {	
		Scanner sc = new Scanner(System.in) ; 
		int n = sc.nextInt() ; 
		int x = sc.nextInt() ; 
		
		int[] nums = new int[n] ; 
		for(int i = 0 ; i < n ; i++) {
			nums[i] = sc.nextInt(); 
		}
		int cnt = countByRange(nums, x, x) ; 
		if(cnt == 0) System.out.println(-1);
		else	System.out.println(cnt);
	}
}
```

<br>



 

