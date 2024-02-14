---
published: true
title: "해싱-호텔 방 배정" 
categories: Algorithm 
tag: [algorithm, Programmers, Two-pointer] 
toc: true
author_profile: false 


---



##### 1. 핵심 아이디어 : ‘모든’ 경계를 기준으로 양쪽 방향의 순열의 합이 같아지는 경우의 최대 값을 구하는 문제



##### 2. 알고리즘 1: 완전 탐색 —> 외부 반복문

- 완전탐색 : 양방향 누적합이 같아지더라도 더 큰 경우가 있을 수 있기 때문에 배열의 마지막 요소까지 경계점으로 고려해서 최대합을 구해야함
- 순차탐색 : 정렬되어있지 않기때문에 순차탐색 (배열의 첫 요소부터 검사 시작)



##### 3. 알고리즘 2: 투포인터 —> 내부 반복문

- 각 경계점마다 순열의 합이 같아지는지, 그 합이 얼만지 확인
- 두 형제의 바구니는 연속해야하기 때문에 경계점은 하나라는게 포인트
- 누적합을 비교해가면서 포인터 확대하다가, 각각의 포인터가 배열의 양끝에 도달할 경우 해당 경계점은 pass
- 주의: 해당 경계점에 대한 누적 합이 같게 나오더라도 탐색을 종료해선 안됨. 계속 확대 하다 보면 더 큰 누적합 값이 나올 수 있기 때문



##### 4. 유사한 문제 : ‘가장 긴 펠린드롬’  ([문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/12904)) 



```java
import java.util.* ; 

class Solution {
    HashMap<Long, Long> m = new HashMap<>() ; // <방번호, 실제 배정 방번호> 
    
    public long[] solution(long k, long[] room_number) {
        long[] answer = new long[room_number.length] ; 
        // answer: 실제로 각 사용자들에게 매칭된 방 배열
        
        for(int i = 0 ; i < room_number.length ; i++){
            answer[i] = matchRoom(room_number[i]) ; 
            // 사용자들이 각각 요청한 방번호에 대해 룸 매칭 함수 호출
        }
        return answer ; // req : room_number --> res : answer 
    }  
    
    // 요청한 방번호(reqRnum)에 대해 실제로 배정된 방 번호(resRnum)를 return 하는 함수 
    public long matchRoom(long reqRnum){ 
        long resRnum = reqRnum ; 
        
        // 요청한 방 번호가 이미 배정되었으면 (배정되어야 map에 insert됨)
        if(m.containsKey(reqRnum))
            resRnum = matchRoom(m.get(reqRnum)) ; // 매칭함수 다시 호출
        
        // 배정되어있지 않으면 (map에 존재하지 않으면)
        m.put(reqRnum, resRnum + 1);  // resRnum으로 방 배정 후 reqRnum에 대한 매칭할 방 정보를 update해줌   
        return resRnum ; // 실제로 배정해준 방번호 resRnum을 반환 
    } // matchRoom    
}
```

