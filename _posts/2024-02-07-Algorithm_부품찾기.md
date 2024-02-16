---
published: true
title: "Sort-부품 찾기" 
categories: Algorithm 
tag: [algorithm, Programmers,  Sort, BinarySearch, HashSet] 
toc: true
author_profile: false 
  
---



##### 문제

N개의 부품 중 사용자가 원하는 M개의 물품이 있는지 각각 확인  

<br>



##### 핵심 아이디어

3가지 풀이 방법 : 계수 정렬, 이진 탐색, 집합 자료형 

<br>



##### 1) 계수 정렬 

```java 
import java.util.*;

public class 부품찾기_계수정렬 {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in) ; 

		int n = sc.nextInt() ; 
		int[] arr = new int[1000001] ; // 모든 원소 도메인을 포함할 수 있는 크기의 리스트  
		for(int i = 0 ; i < n ; i++) {
			int x = sc.nextInt() ;
			arr[x] = 1 ; // x번 물품이 1개 있다 
		}
		int m = sc.nextInt() ; 
		int[] targets = new int[m] ;
		for(int i = 0 ; i < m ; i++) {
			targets[i] = sc.nextInt() ; 
		}		
		for(int i = 0 ; i < m ; i++) {
			int target = targets[i]; 
			if(arr[target]==1) {
				System.out.println("yes ");
			}else {
				System.out.println("no ");
			}
		}
	}
}
```

<br> 

##### 2) 이진 탐색

```java
import java.util.* ; 

public class 부품탐색_이진탐색 {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in) ; 

		int n = sc.nextInt() ; // 1,000,000 --> 매우 큼 : 이진 탐색 고려
		int[] arr = new int[n] ; 
		for(int i = 0 ; i < n ; i++) {
			arr[i] = sc.nextInt() ; 
		}
		Arrays.sort(arr); // 핵심 
		
		int m = sc.nextInt() ; 
		int[] search = new int[m] ; 
		for(int i = 0 ; i < m ; i++) {
			search[i] = sc.nextInt() ; 
		}
		
		for(int i = 0 ; i < m ; i++) {
			if(binarySearch(arr, 0, arr.length-1 , search[i] )!=-1)
				System.out.print("yes ");
			else 
				System.out.print("No ");
        }				
	}
	public static int binarySearch(int[] arr , int start, int end, int target) {
		
		if(start > end) return -1 ; 		
		int mid = (start+end)/2 ;
		if(arr[mid] == target) return mid ; 
		else if(arr[mid] < target) return binarySearch(arr, mid+1, end, target ) ; 
		else return binarySearch(arr, start, mid-1, target) ; 	
	}
}
```

<br>



##### 3) 집합 자료형

```java
import java.util.* ; 

public class 부품찾기_집합 {
	public static void main(String[] args) {		
		Scanner sc = new Scanner(System.in) ; 
		int n = sc.nextInt() ; 
		
		HashSet<Integer> s = new HashSet<>() ; 
		for(int i = 0 ; i < n ; i++) {
			int x = sc.nextInt() ;
			s.add(x) ;
		}
		int m = sc.nextInt() ; 
		int[] targets = new int[m] ;
		for(int i = 0 ; i < m ; i++) {
			targets[i] = sc.nextInt() ; 
		}
		for(int i = 0 ; i < m ; i++) {
			if(s.contains(targets[i]))
				System.out.print("yes ");
			else
				System.out.println("no ");
		}
	}
}
```



<br>

