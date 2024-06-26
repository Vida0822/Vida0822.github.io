---
published: true
title: "BinarySearch-순위 검색"
categories: Algorithm 
tag: [algorithm, 프로그래머스, BinarySearch, Hash ] 
toc: true
author_profile: false 
---



##### 문제 : 순위 검색 (LEVEL 2)

* 지원자 정보 (data)

  : 지원자가 지원서에 입력한 4가지의 정보와 획득한 코딩테스트 점수 

  ; 개발 언어(cpp, java, python) , 직군(backend, frontend) , 경력 (junior, senior) , 소울푸드(chicken, pizza) , 코테점수 (1~ 100,000)

  --> 각 지원자별로 이 내용들로 하나의 문자열로 구성 (공백으로 구분 )

  --> 전체 지원자들의 각각의 정보를 배열로 구성 : String[] info

* 검색 조건 (query) 

​	: 개발팀이 요구하는 지원자 합격 역량 

​	; 각 조건들이 and 로 이어진 문자열 (공백 + and + 공백으로 구분 )

​	 ※ '-'는 고려하지 않겠다는 의미 

​	ex)  "cpp and - and senior and pizza 500"은 "cpp로 코딩테스트를 봤으며, 경력은 senior 이면서 소울푸드로 pizza를 선택한 지원자 중 코딩테스트 점수를 500점 이상 받은 사람은 모두 몇 명인가?"를 의미

​	--> 이 조건들이 여러개 있을 때 각각의 조건들을 모아 저장한 배열 : String[] query



* 도출해야하는 답 

  : 배열에 저장된 조건 순서대로(String[] query) 지원자들을 검토했을 때 (String[] info)

  , 만족하는 지원자의 명수를 각각 도출 하고 그 값들을 요소 값들로 배열에 저장 

​	: int[] answer



---

##### 내 풀이 ❌

* 코딩 테스트 점수로 정렬되어있으면 탐색이 쉬울 듯 ? 

  * 이걸 어떻게 하냐 ... 

    일단 지원자들 조건 공백으로 쪼개서 (배열? ...?)

    정렬 기준 선언해야할 듯 ?

  => Step 1 

* 이중 FOR 문  : i --> 쿼리문 / j --> 지원자들   ; 각 쿼리문마다 지원자들 싹 검토 

  => 도출되는 값을 int 배열에 담기 

  * i번째 쿼리문 --> 지원자들 적용 방법 

    1. 쿼리문을 '공백 + and + 공백' 기준으로 쪼갠다 

    2. 코딩 점수 기준으로 큰 지원자들을 이진 탐색으로 찾고(index) 

       그 index부터 순차 탐색한다 

    3. 해당 단어들이 j번째 지원자 문자열에 포함되는지 확인한다. (and 연산으로)

  => Step 2 

```java
import java.util.* ;
public class 순위탐색 {
	public int[] solution(String[] info, String[] query) {

		// step 1 . 코딩테스트 점수 기준으로 정렬 
		Arrays.sort(info, new Comparator(){
			@Override
			public int compare(Object o1, Object o2) {

				String[] data1 = ((String)o1).split(" ") ; 
				String[] data2 = ((String)o2).split(" ") ; 
				int score1 = new Integer(data1[4] ) ; 
				int score2 = new Integer(data2[4] ) ; 

				return score1 >score2 ? 1 : (score1 == score2 ? 0 : -1) ;
			} // compare
		}); // sort 

		// step 2. 쿼리문으로 검색하기 
		int[] answer = {};
		
		for (int i = 0; i < query.length; i++) {
			String[] queryParams = query[i].split(" and ") ; 
			int index = Arrays.binarySearch(info , queryParams[4] ) ; 
			int count = 0 ; 
			for (int j = index ; j < info.length; j++) {
				String[] datas = info[j].split(" ") ; 
				
				boolean flag = false ; 
				for (int k = 0; k < queryParams.length; k++) {
					
					if(datas[k].contains(queryParams[k]) ||  queryParams[k] == "-" ) {
						flag = true ;
					}
				} // k 
				if(flag) {
					count++ ; 
				}
			} // j 
			answer[answer.length] = count ; 
		} // i 

		return answer;
	} // main 
} // class 

```







---

##### 해설 풀이 ✔

**문제: "이러이러한 조건을 만족하는 지원자 수가 몇명이냐"**



**logic**

* Query 의 효율성 : 여러번 일어나는 검색에 추가적인 연산이 필요하면 안됨 

=> 모든 조건 조합에 따른 지원자를 모두 구분해 놓고 쿼리 검색이 들어왔을 때 Hashing으로 한번에 찾아야 빠름 (O(1)) 

<-> 모든 가능한 조건 조합의 경우의 수를 전부 키로 만들어 놓고, 각 키에 해당하는 지원자들의 점수를 Value 값인 ArrayList에 차곡차곡 쌓음으로 써 각 키에 해당하는 지원자들을 그루핑 함 

 

**Step 1. info를 기반으로 HashMap 생성 **

* HashMap<String key , ArrayList > 

* "-" 를 포함한 모든 조건 조합을 키로 생성해 줘야함 



**Step 2. 각 hashMap의 Value를 오름차순 정렬 **

: 특정 조건을 만족하는 지원자들 중 코테 점수가 X 점 이상인 지원자들만 찾는 것이므로 미리 sort 작업 

=> 쿼리 작업할 때 이분탐색으로 검색 



**Step 3 .  query 처리 **

: query 조건에 맞는 지원자들 가져오기 

--> Hashing으로 get()



**Step 4. lower-bound (하한선) 찾기 **

=> 전체 지원자 중 하한선 위치 뺀 만큼이 조건 충족 지원자 수 ! 



```java
package search ; 

import java.util.* ;

public class 순위탐색_해설{
	public int[] solution(String[] info, String[] query) {

		// 1. info를 기반으로 hashMap 만들기 
		HashMap<String, ArrayList<Integer>> hashMap = new HashMap<>(); 

		for(String i : info) {
			String[] data = i.split(" ") ; 
			String[] languages = {data[0], "-"} ; // 조건 조합에 "-" 인 경우도 포함해야함 
			String[] jobs = {data[1]  , "-" } ; 
			String[] exps = {data[2] , "-" } ; 
			String[] foods = {data[3] , "-" } ;
			Integer value = Integer.parseInt(data[4]) ; // 점수 

			for(String lang : languages)
				for(String job : jobs)
					for(String exp : exps)
						for(String food : foods) {
							String[] keyData = {lang, job, exp, food} ; 
							String key = String.join(" ", keyData) ; // 각 값들을 띄어쓰기를 기준으로 하나의 문자열로 합침 
							ArrayList<Integer> arr = hashMap.getOrDefault(key, new ArrayList<Integer>()) ; // 해당 key값에 해당하는 value, ArrayList를 가져오는데 만약 value 값이 없으면 새로은 ArrayList를 생성해 반환한다
							arr.add(value) ; 
							hashMap.put(key, arr); // 같은 key 값이 있으면 덮어쓰기 됨 
						}
		} // for info 

		// 2. hashMap의 각 key의 value(ArrayList) 를 오름차순 정렬하기 
		for(ArrayList<Integer> scoreList : hashMap.values())
			scoreList.sort(null); // ※ list 정렬할때는 오름차순일 땐 null을 넣어줘야함 

		// 3. query 조건에 맞는 지원자들 가져오기 
		int[] answer = new int[query.length] ;
		// 사전 코드에 나타나있는 크기 0짜리 배열은 크기 지정해서 다시 생성해줘야함 

		int i = 0; 
		for(String q : query) { // 쿼리를 하나씩 뽑아서  "java and backend and junior and pizza 100" 
		
			String[] keyData = q.split(" and ") ;  // 각 쿼리를 " and " 를 기준으로 split  -  "java/backend/junior/pizza 150" 
			int target = Integer.parseInt(keyData[3].split(" ")[1] ) ; // "pizza 150" --> 150 (int) : 얜 따로 key 배열에 저장하지 않음 
			keyData[3] = keyData[3].split(" ")[0]; // "pizza"  (update : pizza 150 --> pizze) 
			String key = String.join(" ", keyData) ; 
			
			
			// 4. lower - bound 하한선 찾기 (이진탐색) 
			if(hashMap.containsKey(key)) { // hashTable증 해당 key 가 있으면 
				ArrayList<Integer> list = hashMap.get(key) ; 
				int left = 0 ; 
				int right = list.size(); 
				while(left < right ) {
					// 이진탐색 직접 구현 
					int mid = (left + right) / 2 ; 
					if(list.get(mid) >= target)
						right= mid ; 
					else
						left = mid + 1 ; 
				}
				answer[i] = list.size() -left ; 
			}
			i++ ;  //query index와 answer index 맞춰주기 위해 
		} // for q 
		return answer ; 
	} // solution 


	public static void main(String[] args) {
		순위탐색_해설 sol = new 순위탐색_해설();
		String[] info = { "java backend junior pizza 150", "python frontend senior chicken 210",
				"python frontend senior chicken 150", "cpp backend senior pizza 260", "java backend junior chicken 80",
		"python backend senior chicken 50" };
		String[] query = { "java and backend and junior and pizza 100",
				"python and frontend and senior and chicken 200", "cpp and - and senior and pizza 250",
				"- and backend and senior and - 150", "- and - and - and chicken 100", "- and - and - and - 150" };
		System.out.println(sol.solution(info, query));
	} // main 

}
```

