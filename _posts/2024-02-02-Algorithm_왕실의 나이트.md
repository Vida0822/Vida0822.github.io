---
published: true
title: "Greedy-왕실의 나이트" 
categories: Algorithm 
tag: [algorithm, 이것이 코딩테스트다, greedy] 
toc: true
author_profile: false 


---

##### 문제 

* 책, '이것이 취업을 위한 코딩 테스트다' / ch4 구현 / 예제 4-3 (p.115)

나이트는 다음과 같은 두가지 경우로 이동할 수 있다. 

1. 수평으로 두 칸 이동한 뒤 수직으로 한 칸 이동 
2. 수직으로 두 칸 이동한 뒤 수평으로 한 칸 이동 

나이트의 위치 좌표가 입력으로 주어질 때, 이동할 수 있는 경우의 수를 구하는 프로그램을 작성하시오. 



##### Step 1. 문제 접근 

격자, 좌표 안에서 이동하는 Simultaion문제로, 갈 수 있는 이동 좌표를 체크해보면서 범위 밖으로 벗어나는지 확인하는 문제다. 



##### Step 2. 선택한 알고리즘 

완전 탐색 - dx, dy 배열을 구현하여 이동할 수 있는 모든 경우의 수를 체크한다.   



##### Step 3. 선택 이유 

이동 할 수 있는 최대 경우의 수가 한정되어있기 그 경우의 수를 모두 배열로 구현한 후, 모두 테스트 해보며 이동 가능 여부를 체크한다. 



##### Step 4. 구현 (코드)

```java
package implementation;

import java.util.Scanner;

public class 왕실의나이트 {

	public static void main(String[] args) {
		
		Scanner sc = new Scanner(System.in) ; 
		String[] coordinate = sc.next().split("") ;  // a1 
		int x = coordinate[0].codePointAt(0) - 96 ;  
		/*
		 * 자바 인코딩은 '아스키 코드'를 기준으로 하고 있다. 
		 * ㄴ 암기 <A : 65 / a : 97 >
		 */
		int y = Integer.parseInt(coordinate[1]); 
		
		int[] dx = {2, 2, -2, -2,1, -1, 1 , -1} ; 
		int[] dy = {1, -1, 1 , -1,2, 2, -2, -2} ; 
		int length = dx.length;
		
		int answer = 0 ; 
		for(int i = 0 ; i < length ; i++) {
			int newX = x + dx[i] ; 
			int newY = y + dy[i] ; 
			
			if(canGo(newX, newY)) {
				answer++; 
			}
		}
		System.out.println(answer);
	} // main  
	public static boolean canGo(int newX, int newY) {
		if(newX<1 || newX > 8 || newY<1 || newY > 8)
			return false; 
		return true ; 
	}
} // class 
 
```

