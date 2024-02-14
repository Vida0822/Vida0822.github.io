---
published: true
title: "Implementaion-상하좌우" 
categories: Algorithm 
tag: [algorithm, 프로그래머스, 구현] 
toc: true
author_profile: false 


---



##### 문제 

* 책, '이것이 취업을 위한 코딩 테스트다' / ch4 구현 / 예제 4-1 (p.110)

A는 격자에서 이동하려한다 (1,1)  -> (N,N). 

A의 이동 계획서에 적혀있는 L, R, U, D 문자들에 따라 이동하며 정사각형 공간을 벗어나는 움직임은 무시된다. 

A가 최종적으로 도착할 지점의 좌표를 출력하시오 



##### Step 1. 문제 접근 

계획서에 적혀있는 문자를 한 글자 씩 읽으면서, 이동하기 전에 이동 가능 여부를 조건검사 한 후 이동한다. 

'이동'은 현재 A가 있는 좌표값을 더하기/빼기 하는 방식으로 구현한다. 



##### Step 2. 선택한 알고리즘 

**시뮬레이션** : 일련의 명령에 따라 개체를 차례대로 이동시키는 문제 

--> 구현이 중요 : 격자, 좌표, 이동....  (언어 특징 및 라이브러리 활용)

--> 자바의 경우, dx/dy 배열로 이동 구현 , 좌표 클래스, 격자 배열(장애물, 못가는 곳 등 표시), 갈 수 있는 격자 범위 등 (+추후 방문 배열)





##### Step 3. 선택 이유 

해당 문제는 '격자'라는 추상적인 값을 프로그래밍 언어로 구현해야하며, 

'좌표', '이동'이라는 현실적인 아이디어를 표현하는 문제이기 때문에 

언어와 라이브러리 사용하 중심이 되는 '구현' 문제로 분류하였다. (각 언어마다 구현 방식이 많이 달라지는 문제 유형)

구현 문제의 특징 답게, 여러가지 세부 조건들을 동시에 고려하는게 관건이다.

예상 시간복잡도는 시간 계획서에 나온 문자의 갯수만큼 반복하므로 O(K)이다. 



##### Step 4. 구현 (코드)

```java
package implementation;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.Scanner;

public class 상하좌우 {
	
	static int dx[] = {0 , 0, -1, 1 } ; // LRUD 
	static int dy[] = {-1, 1, 0, 0} ; // ※ 우하향으로 갈 수록 좌표값이 커진다 
	static int n ; 
	
	public static void main(String[] args) {
		
		// part 1. 격자 및 좌표 구현 
		// 격자는 이차원배열로, 좌표상 이동은 각각 dx, dy 배열로 구현하여 상하좌우 이동을 구현한다 
		Scanner sc = new Scanner(System.in) ; 
		n = sc.nextInt() ; 
		
		
		// part 2. 계획서 입력받기 
		sc.nextLine() ; // 버퍼 비우기 - sc.next()로 하면 다음 글자 먹힘
		/*
		 * next()는 개행 문자를 무시하고 입력받기 때문에 해당 명령어가 무시되서 버퍼에 \n 이 남아있게 된다 
		 * 이후의 nextInt() 등이 오면 개행문자를 무시하고 값을 가져오기 때문에 숫자만 정상적으로 가져오지만
		 * nextLine()은 공백문자, 개항문자 분리자를 포함하고, 개항문자를 보는 순간 입력을 종료시키기 때문에(개항문자까지의 문자열을 입력받는 메서드)이후의 값은 무시된다. 
		 * => 중간에 nextLine()을 넣어줘야한다.  
		 */
		String plan = sc.nextLine() ; 
		String[] moves = plan.split(" ") ; 
		String[] moveTypes = {"L", "R", "U", "D"};
		
        // part 3. 이동하기 
		Pair A = new Pair(1, 1) ; 
		for(int i = 0 ; i < moves.length ; i++ ) {
			
			int index = 0 ; 
			/*
			switch(moves[i]) {
				case "L" : 
					index = 0 ; 
					break ; 
				case "R" : 
					index = 1 ; 
					break ; 
				case "U" : 
					index = 2 ; 
					break ; 
				case "D" : 
					index = 3 ; 
					break; 
			} // switch
			*/
			for(int j = 0 ; j < 4 ; j++) {				
				int newX = 0 , newY = 0;
				
				// 이동 계획을 하나씩 비교 하나씩 확인 
				if(moves[i] == moveTypes[j]) {
					newX = A.x + dx[index] ; 
					newY = A.y + dy[index] ; 				
				} // if 
				if(inRange(newX, newY)) {
					A = new Pair(newX, newY) ; 
				} // if 	
			}	 // for 
		} // for 
        
        // part 4. 최종 좌표 출력 
		System.out.println(A.x+" "+ A.y) ; 
	} // main 
	
	public static boolean inRange(int newX, int newY) {
		if(newX < 1 || newX > n) {
			return false ; 
		}
		if(newY < 1 || newY > n) {
			return false ; 
		}
		return true;
	} // inRange
} // class 

// 좌표 클래스 
class Pair{
	int x ; 
	int y ; 
	public Pair(int x, int y) {
		this.x = x ; 
		this.y = y; 
	}
} // Pair

```

