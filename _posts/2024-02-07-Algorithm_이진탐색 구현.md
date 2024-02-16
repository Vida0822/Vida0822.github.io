---
published: true
title: "BinarySearch-이진탐색 코드구현" 
categories: Algorithm 
tag: [algorithm, BinarySearch] 
toc: true
author_profile: false 
  
---



##### 1. 재귀 

```java 
public class 이진탐색_재귀 {	
	public static int binarySearch(int[] arr , int target, int start, int end) {

		// 종료조건 1) 찾고자하는 값이 없음 
		if(start > end) return -1 ; 
		
		int mid = (start+end)/2 ; 
		
		// 종료조건 2) 값을 찾음
		if(arr[mid] == target) return mid ; 
		// 중간점의 값보다 찾고자하는 값이 작은경우, 중간점 왼쪽으로 끝점을 옮겨 다시 이진 탐색 
		else if(arr[mid] > target) return binarySearch(arr,target, start, mid-1);
		// 중간점의 값보다 찾고자하는 값이 큰 경우, 중간점 오른쪽으로 사적점을 옮겨 다시 이진 탐색 
		else return binarySearch(arr, target, mid+1, end) ; 		
	}
} // class
```

<br> 

##### 2. 반복문

```java
public class 이진탐색_반복 {
	public static int binarySearch(int[] arr, int target, int start, int end) {
		
        while(start <= end) {
			// 보통 재귀함수의 대체로 while 반복문을 사용한다
			int mid = (start+end)/2 ; 			
			// 종료조건 
			if(arr[mid] == target)
				return mid ; // 반복문을 나가는 것이 아닌 함수를 종료 
			else if(arr[mid] > target) 
				end = mid-1 ; 
			else
				start = mid +1 ;
		}
		return -1; 
	}
} // class 
```

<br>



##### 3, 라이브러리 

```java
import java.util.Arrays;
public class 이진탐색_라이브러리 {	
    
	public static int binarySearch(int[] arr, int target, int start, int end) {	
		return Arrays.binarySearch(arr, target)
	}
} // class 
```

<br>
