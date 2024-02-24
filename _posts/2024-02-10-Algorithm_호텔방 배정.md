---
published: true
title: "Hash-호텔 방 배정" 
categories: Algorithm 
tag: [algorithm, Programmers, Two-pointer] 
toc: true
author_profile: false 


---



##### 문제 

호텔에 투숙하려는 고객들에게 방을 배정하려 합니다. 호텔에는 방이 총 k개 있으며, 각각의 방은 1번부터 k번까지 번호로 구분하고 있습니다. 처음에는 모든 방이 비어 있으며 다음과 같은 규칙에 따라 고객에게 방을 배정하려고 합니다.

1. 한 번에 한 명씩 신청한 순서대로 방을 배정합니다.
2. 고객은 투숙하기 원하는 방 번호를 제출합니다.
3. 고객이 원하는 방이 비어 있다면 즉시 배정합니다.
4. 고객이 원하는 방이 이미 배정되어 있으면 원하는 방보다 번호가 크면서 비어있는 방 중 가장 번호가 작은 방을 배정합니다.

전체 방 개수 k와 고객들이 원하는 방 번호가 순서대로 들어있는 배열 room_number가 매개변수로 주어질 때, 각 고객에게 배정되는 방 번호를 순서대로 배열에 담아 return 하도록 solution 함수를 완성해주세요.

<br>



##### 코드(구현)

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

