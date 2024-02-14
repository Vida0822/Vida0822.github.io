---
published: true
title: "해싱-호텔 방 배정" 
categories: Algorithm 
tag: [algorithm, Programmers, Two-pointer] 
toc: true
author_profile: false 


---



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

