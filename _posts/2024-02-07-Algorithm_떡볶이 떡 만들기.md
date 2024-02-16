---
published: true
title: "BinarySearch-떡볶이 떡 구하기" 
categories: Algorithm 
tag: [algorithm, BinarySearch] 
toc: true
author_profile: false 
  
---



##### 문제 

각각 길이가 다른 떡들 n개.

손님이 요청한 총 떡길이가 M일 때, 설정할 수 있는 절단기 높이의 최댓값은?

<br> 



##### 핵심 아이디어 

'특정 target을 감당할 수 있는가?'유형 이진탐색문제 

절단기의 높이를 (가장긴떡길이/2)로 설정한 후, 감당 가능하면 왼쪽범위로, 불가능하면 오른쪽 범위로 절단기 높낮이 조정

<br>



##### 파라메트릭 서치 

최적화 문제를 결정문제로 바꾸어 해결하는 기법 

되냐/안되냐?를 범위를 좁혀가며 탐색함으로써 최적의 해를 구함

=> 조건 : 현재 이 높이로 자르면 떡 최소 조건 만족 ? --> 만족 여부(예, 아니오) 도출 

=> 범위를 좁힐 때 '이진 탐색' 

<br>



##### 코드 

```java 
import java.util.* ; 

public class 떡볶이떡만들기 {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in) ; 
		int n = sc.nextInt() ; // array 
		int m = sc.nextInt() ;  // target 
		
		int[] yum = new int[n] ; 
		for(int i = 0 ; i < n ; i++) {
			yum[i] = sc.nextInt() ; 
		}
		Arrays.sort(yum);
		int max = yum[yum.length-1]; 
		int start = 0, end = max ;  
		int answer = 0 ; 
		while(start <= end) {
			
			int mid = (start+end)/2 ; 
			if(isPossible(yum, mid,m )) {
				answer = mid; // ※ 최대한 덜 잘랐을 때가 정답이므로, 우선 answer에 기록 후 다음 반복 진행
				start = mid + 1  ;
			}else
				end = mid - 1 ; 
		}
		System.out.println(answer);
	} // main 
	
	public static boolean isPossible(int[] yum, int cut, int m) {
		int dduck = 0 ; 
		for(int i = 0 ; i < yum.length ; i++) {
			// 떡이 부족한 경우도 고려해줘야함 --> 안그러면 음수값이 누적
			if(yum[i] < cut)
				continue ; 
			
			dduck += (yum[i] - cut) ; 
			if(dduck >= m )
				return true ; 
		}
		return false ; 
	}
} // class 
```

<br>
