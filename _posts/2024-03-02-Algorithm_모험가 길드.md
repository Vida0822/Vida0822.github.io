---
published: true
title: "Greedy-모험가 길드" 
categories: Algorithm 
tag: [algorithm, Programmers, Greedy] 
toc: true
author_profile: false 
  
---



##### 문제

N명의 모험가를 대상으로 '공포도' 측정.

공포도가 X인 모험가는 반드시 X명 이상으로 구성한 모험가 그룹에 참여해야 함 

여행을 떠날 수 있는 그룹수의 최댓값? 

<br>



##### 핵심 아이디어 

> Greedy 

최대한 작은 숫자의 공포도를 가진 사람끼리 그룹을 형성해야 도출 그룹수가 많아진다. 

공포도를 오름차순으로 나열한 뒤 한명씩 그룹후보로 포함시키면서 그룹 형성 조건에 만족하면 바로 그룹결성 후 다음 그룹 고려. 

즉, 매 선택마다 가장 낮은 공포도를 가진 모험가를 후보자로써 포함시키므로 'Greedy 알고리즘'이 적합하다.

<br>



##### Point 

> 현재 확인하는 공포도와 비교 

그룹 후보들의 max값과 비교할 필요 없이 검사하는 모험가 공포도와만 비교하면 된다 

배열이 이미 오름차순으로 정렬되어있기 때문에 현재 검사하는 모험가 공포도가 이전보다 무조건 max이다. 

<br>



##### 코드 (Java)

```java
import java.util.* ; 

public class 모험가길드 {
	public static void main(String[] args) {
		
		Scanner sc = new Scanner(System.in) ; 
		int n = sc.nextInt() ; 
		
		int[] fighters = new int[n] ; 
		for(int i = 0 ; i < n ; i++) {
			fighters[i] = sc.nextInt() ; 
		}
		
		// 1. 모험가 길드원 정렬 
		Arrays.sort(fighters) ; // 1 2 2 2 3 
		
		// 2. 가장 적은 모험가부터 검사하면서 그룹 내 모험가 점수 <= 그룹 인원 되는 순간 그룹수++ 
		int group = 0 ; 
		int m_num = 0 ;
	//	int max_score = 0 ; 
		for(int fighter : fighters) {
			m_num++ ; 			
			if(fighter <= m_num) {
				group++; 
				m_num = 0 ; 
			}
		} // for		
		System.out.println(group);
	}
}

```

<br>



##### 코드 (Python)

```java
n = int(input())
data = list(map(int, input().split())) 
# input() : 입력받은걸, split() : 공백기준으로 오려서, int : int형으로 변환해서, list : list 자료형으로 만든다 
data.sort()

result = 0 # 총 그룹의 수 
count = 0 # 현재 그룹에 포함된 모험가의 수 

for i in data :  # 공포도를 낮은 것부터 하나씩 확인하며
    count += 1  # 현재 그룹에 해당 모험가 포함시키기
    if count >= i : # 현재 그룹에 포함된 모험가 수가 현재의 공포도 이상이라면 
        result+=1 # 그룹 결성
        count = 0  # 모험가 수 초기화 

print(result)
```

<br> 

