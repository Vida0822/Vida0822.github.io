---
published: true
title: "Sort-여러가지 정렬기법" 
categories: Algorithm 
tag: [algorithm, Sort] 
toc: true
author_profile: false 

---



##### 1. 버블 정렬

```java
public class Ex01_BubbleSort {
	
	public static void swap(int[] a, int idx, int idx2) {
		int t = a[idx] ;  // '교환' : temp 변수에 넣어두기 
		a[idx] = a[idx2] ; 
		a[idx2] = t ; 
	} // swap 
	
	public static void bubbleSort(int[] a, int n) {	
		int k = 0 ;
		while(k < n-1 ) {
			int last = n-1 ; 
			for (int i = n-1 ; i >  k ;  i-- ) {
				if(a[i-1] > a[i]) {
// 					swap(a, i-1 , i) ; 				
					int t = a[i-1] ; 
					a[i-1] = a[i] ; 
					a[i] = t ; 
					
 					last = i ; 
				}
			k= last; 
			}	
		}
	} // bubbleSort
}
```

<br>



##### 2. 선택 정렬

```java
public class SelectionSort { // O(n^2)
	public static void main(String[] args) {
		
		int n = 10 ; 
		int[] arr = {7, 5, 9, 0, 1, 6, 2, 4, 8 }; 
		
		for(int i = 0 ; i < n ; i++) { // 배열 크기만큼 작은 수 찾기 시행 
			int min_index = i ; // 가장 작은 원소가 들어갈 자리  
			
			// 가장 작은 수를 찾아서 
			for(int j = i+1 ; j < n ; j++) { 
				// 대소관계 비교 범위가 i가 증가함에 따라 그 뒤에것들만 비교 
				if(arr[min_index] > arr[j])
					min_index = j ; 
			}
			// 맨 앞이랑 스와프 
			int temp = arr[i] ; 
			arr[i] = arr[min_index] ; 
			arr[min_index] = temp; 
		}
		
		for(int i = 0 ; i < n ; i++) {
			System.out.println(arr[i]+" ");
		}
	}
}
```

<br>



##### 3. 삽입 정렬 

```java
public class InsertionSort { // O(n^2) 
	public static void main(String[] args) {
		
		int n = 10 ; 
		int[] arr = {7, 5, 9, 0, 3, 1, 6, 2, 4, 8} ; 
		
		for(int i = 1 ; i < n ; i++) {
			// 배열의 요소 갯수 만큼 반복한다 
			
			for(int j = i ; j > 0 ; j--) {
				// 각 요소마다 왼쪽 요소와 비교해가면서 
				
				if(arr[j] < arr[j-1]) { // 왼쪽요소가 크면 swap --> 비교 이어서 
					int temp = arr[j] ; 
					arr[j] = arr[j-1] ; 
					arr[j-1] = temp; 
				}else
					break ; // 자기보다 작은 데이터를 만나면 그 위치에서 멈춤  
			}
		}
		for(int i = 0 ; i < n ; i++) {
			System.out.println(arr[i]+" ");
		}
	}
}
```

<br>



##### 4. 퀵 정렬 (피봇 정렬)

```java
package sorting;

public class QuickSort { // O(NlogN)
	
	public static void quickSort(int[] arr, int start, int end) {
		// 퀵 정렬은 재귀함수로 구현한다 
		
		// 종료조건 
		if(start >= end) // 원소가 1개인 경우  
			return ; 
		
		int pivot = start ; 
		int left = start + 1 ; // 왼쪽부터 피봇보다 작은지 검사 
		int right = end ; // 오른쪽부터 피봇보다 큰지 검사 
		
		while(left <= right) { // '인덱스'가 교차할 때까지 
			while(left <= end && arr[left] <= arr[pivot]) 
				// 피봇보다 작은 데이터의 index 뽑고 
				left++ ;
			// 검사 index가 end를 맞닦드리면 반복 종료 
			
			while(right > start && arr[right] >= arr[pivot])
				// 피봇보다 큰 데이터의 index를 뽑아서 
				right-- ; 
			// 검사 index가 start를 맞닦드리면 반복 종료 
			
			// 1) pivot을 기준으로 큰값, 작은 값 분류 
			if(left <= right) {
				int temp = arr[left] ; 
				arr[left] = arr[right] ; 
				arr[right] = temp ; 
			
			// 2) 엇갈렸다면 피봇과 작은 데이터를 교체 ※ 엇갈린 상태로 index가 고정되었으므로 left가 아니라 right가 pivot보다 작은 값임  
			// 		=> 그루핑 
			}else {
				int temp = arr[pivot] ; 
				arr[pivot] = arr[right]; 
				arr[right] = temp ; // pivot값이 right 위치에 들어감 
			}
		}
		// 3) 피봇을 기준으로 왼쪽, 오른쪽 리스트를 각각 퀵정렬 수행 (재귀함수 호출)
		quickSort(arr, start, right-1) ; // start ~ pivot-1 퀵 정렬 
		quickSort(arr, right+1, end) ; // pivot+1 ~ end 퀵정렬 	
	} // quickSort
	
	public static void main(String[] args) {
		int n = 10 ; 
		int[] arr = {7,5,9,0,3,1,6,2,4,8} ; 
		
		quickSort(arr, 0, n-1); // 전체를 대상으로 퀵정렬 수행 
		
		for(int i = 0 ; i < n ; i++) {
			System.out.println(arr[i]+" ");
		}
	} // main
} // class 

```

<br>





##### 5. 계수 정렬 

```java
package sorting;

public class CountSort { // O(N+K)
	
	public static final int MAX_VALUE = 9 ; 

	public static void main(String[] args) {
		int n = 15 ; 
		
		// 정렬 대상 배열 (제한 조건 : 모든 원소의 값은 0~9안에 속하는 자연수이다 )
		int[] arr = {7,5,9,0,3,1,6,2,9,1,4,8,0,5,2} ; 

		// 도메인 배열 : 도메인에 포함될 수 있는 값 갯수(종류)를 크기로하는 List 생성
		int[] cnt = new int[MAX_VALUE+1] ; 
		
		// 정렬 대상 배열의 데이터를 하나씩 읽으며, 해당 도메인값의 등장횟수를 도메인 배열의 value값(등장횟수)에 반영
		for(int i = 0 ; i < n ; i++) { // 데이터들의 값을 하나씩 검사하면서
			int num = arr[i] ; // 7이 나오면 
			cnt[num] += 1 ; // 7번째 배열의 값(count) 1증가	
		}
		
		// 도메인 배열의 key값을 value값 만큼 반복해서 출력 --> TreeMap<>으로도 구현할 수 있지 않을까?(+동적으로 관리할 수 있는 장점 추가) 
		for(int i = 0 ; i <= MAX_VALUE ; i++) {
			for(int j = 0 ; j < cnt[i] ; j++) {
				System.out.print(i+" ");
			}
		}
	} // main 
} // class 

```

<br>
