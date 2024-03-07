---
published: true
title: "DP-금광" 
categories: Algorithm 
tag: [algorithm, Programmers, DP] 
toc: true
author_profile: false 
  
---



##### 문제 

* 링크 : [코딩테스트 연습 - 실패율 | 프로그래머스 스쿨 (programmers.co.kr)](https://school.programmers.co.kr/learn/courses/30/lessons/42889)

실패율= (스테이지에 도달했으나 아직 클리어하지 못한 플레이어의 수 / 스테이지에 도달한 플레이어 수)

전체 스테이지의 개수 N, 게임을 이용하는 사용자가 현재 멈춰있는 스테이지의 번호가 담긴 배열 stages가 매개변수로 주어질 때, 

실패율이 높은 스테이지부터 내림차순으로 스테이지의 번호가 담겨있는 배열을 return

<br>



##### 테스트 케이스 분석 

> 정렬  

stages : [1, 2, 2, 2, 3, 3, 4, 6] 

1번 스테이지 도달한 사람 수 : 8명, 실패한 사람 수 : 1명 --> 실패율 저장 

2번 스테이지 도달한 사람 수 : 7명, 실패한 사람 수  3명 --> 실패율 저장 

3번 스테이지 도달한 사람 수 : 4명, 실패한 사람 수 : 2명 --> 실패율 저장 

4번 스테이지 도달한 사람 수 ; 2명, 실패한 사람 수 : 1명 --> 실패율 저장 



stages :  [2, 2, 2, 4, 6] 

2번 스테이지 도달한 사람 수 : 5명, 실패한 사람 수 : 3명 

--> 1번, 2번 스테이지 실패율 저장 

​	: 1번 스테이지 도달한 사람 수 5명, 실패한 사람 수 : 0명  --> 실패율  : 0

4번 스테이지 도달한 사람 수 : 2명, 실패한 사람 수 : 1명 

--> 3번, 4번 스테이지 실패율 저장 

​	: 3번 스테이지 도달한 사람 수 : 2명, 실패한 사람 수: 0명   --> 실패율  : 0

6번 스테이지 도달한 사람 수 : 1명, 실패한 사람 수 : 1명

-->  5번, 6번 스테이지 실패율 저장 

​	: 5번 스테이지 도달한 사람 수 : 1명, 실패한 사람 수 0명  --> 실패율  : 0





##### 핵심 아이디어 

> 정렬

위의 분석을 보았을 때, 사용자들이 멈춰있는 stage외에는 실패율이 0이다. 

사용자들이 멈줘있는 stage만 실패율을 계산해 내림차순으로 정렬하고, 

그 뒤에 나머지 stage들을 스테이지 번호 오름차순으로 정렬하여 덧붙인다. 

<br>





##### 시간 복잡도 

> O(20000)

* 전체 스테이지의 수 N (1 ~ 500) 

* 각 사용자가 멈춰있는 스테이지 번호 배열 : stages (~200000)

전체 스테이지수(N)의 범위가 500으로 아주 작기 때문에 stages 배열은 한번만, O(N)은 여러번 수행해도 된다

<br>



##### 로직

1. Stage 클래스 : 1차 정렬기준(실패율), 2차 정렬기준(인덱스)를 구현하기 위해 클래스를 정의한 후 compareTo() 메서드를 오버라이딩

2. stages 배열 정렬 : 같은 스테이지에 멈춰있는 회원수를 검사하기 위해 오름차순으로 정렬한다

3. 실패율이 0이상인 스테이지 우선순위큐에 저장  

   : stages 배열을 순회하며 특정 stage에 멈춰있는 회원수를 구한 후, 이를 바탕으로 실패율을 구하여 우선순위 큐에 삽입한다 

   -> 이 때, 각 stage들은 자동으로 실패율 내림차순 , 인덱스 오름차순으로 정렬된다

4. 멈춘 사람이 있다는 의미로 visited 배열에 표시한다
5. 멈춘 사람이 있는 Stage는 실패율이 존재하므로, 먼저 큐에서  하나씩 꺼내 stage 번호만 답배열에 넣어준다
6. 멈춘 사람이 없는 Stage는 실패율이 0이므로,  그 뒤에 덧붙인다

<br>



##### Point

1. 이 문제는 많은 배열의 포인터 및 인덱스를 관리하는 것이 핵심이다. 

동일한 스테이지에 머무른 사람수를 counting 하고, 그 사람 수만큼 건너뛰어, 그 이후의 stage에 멈춘 사람을 계산해야해

배열의 포인터인 i에 count를 누적해줘야 한다 



2. 만약 어떤 값을 dictionary 형태로 저장해야한다면, 이를 Map으로 구현할 지, Class로 정의할지 고민한다. 

 만약 단순히 짝짓기 외 부가정보를 저장해야한다면 Class를 활용한다. 

<br>

3. 다른 자료형의 필드값을 compareTo로 구현할 땐 래퍼 클래스의 compare() 함수에 두 값을 넣어준다 



##### 코드 (Java)

```java
import java.util.* ; 

class Stage implements Comparable<Stage>{
    int index ;
    double failRatio ; 
    
    public Stage(int index, double failRatio){
        this.index = index ;
        this.failRatio = failRatio ; 
    } 
    @Override
    public int compareTo(Node other) {
        if (this.fail == other.fail) {
            return Integer.compare(this.stage, other.stage);
        }
      	return Double.compare(other.fail, this.fail);
    }
}

class Solution {
    public int[] solution(int N, int[] stages) {
      
        boolean[] visited = new boolean[N+1] ; 
        int people = stages.length ;
        
        // stages 배열 정렬
        Arrays.sort(stages) ; 

        // 실패율이 0이상인 스테이지 우선순위큐에 저장 
        PriorityQueue<Stage> stopStages = new PriorityQueue<>() ;
        for(int i = 0 ; i < stages.length ; i++){ // O(200000)
            int stopAt = stages[i] ; // 3 
            int count = 1; 
            while(i+count != stages.length && stopAt == stages[i+count] ){
                count ++ ; 
            }
            if(stopAt <= N){
                stopStages.offer(new Stage(stopAt, (double)count/(people-i))) ; 
                visited[stopAt] = true ; 
            }
            i+=count -1 ; // i가 반복문 자체에서 한번 ++ 되기 때문에 -1 해서 다음 반복문으로 넘겨준다
        }
        
        
        // list에 실패율 내림차순으로 stage 번호 넣기 
        ArrayList<Integer> list = new ArrayList<>() ; 
        while (!stopStages.isEmpty()) {  // O(N)
            Stage stage = stopStages.poll() ; 
            if(!list.contains(stage.index))
                list.add(stage.index) ; 
        }
                
        // list없는 stage들 오름차순으로 stage 번호 넣기 
        for(int i = 1 ; i <= N ; i++){  // O(N)
            if(!visited[i])
                list.add(i) ; 
         }
        
        // 답 반환(배열로 변환)
        int[] answer = new int[N] ; 
        for(int i = 0 ; i < list.size() ; i++){   // O(N)
            answer[i] = list.get(i) ;
        }
        return answer ;       
    }
}
```

<br>





---

##### 해설 풀이 

>  각 스테이지 index를 하나씩 검사한다 

해당 스테이지에 머물러있는 사람수를 count해 실패율을 계산.

이렇게 하면 실패율을 따로따로 고려해줄 필요 없이 사람이 없는 스테이지는 count가 0이되어 자동으로 실패율이 0으로 세팅된다.

<br> 

##### 해설 코드 (Java)

```java
import java.util.*;

class Node implements Comparable<Node> {

    private int stage;
    private double fail;

    public Node(int stage, double fail) {
        this.stage = stage;
        this.fail = fail;
    }

    public int getStage() {
        return this.stage;
    }

    @Override
    public int compareTo(Node other) {
        if (this.fail == other.fail) {
            return Integer.compare(this.stage, other.stage);
        }
        return Double.compare(other.fail, this.fail);
    }
}

class Solution {
    public int[] solution(int N, int[] stages) {
        int[] answer = new int[N];
        ArrayList<Node> arrayList = new ArrayList<>();
        int length = stages.length;

        // 스테이지 번호를 1부터 N까지 증가시키며
        for (int i = 1; i <= N; i++) {
            // 해당 스테이지에 머물러 있는 사람의 수 계산
            int cnt = 0;
            for (int j = 0; j < stages.length; j++) {
                if (stages[j] == i) {
                    cnt += 1;
                }
            }

            // 실패율 계산
            double fail = 0;
            if (length >= 1) {
                fail = (double) cnt / length;
            }

            // 리스트에 (스테이지 번호, 실패율) 원소 삽입
            arrayList.add(new Node(i, fail));
            length -= cnt;
        }

        // 실패율을 기준으로 각 스테이지를 내림차순 정렬
        Collections.sort(arrayList);

        // 정렬된 스테이지 번호 반환
        for (int i = 0; i < N; i++) {
            answer[i] = arrayList.get(i).getStage();
        }
        return answer;
    }
}
```



##### 해설 코드(Python)

```python
def solution(N, stages):
    answer = [] 
    length = len(stages) 

    # 스테이지 번호를 1부터 N까지 증가시키며 
    for i in range(1, N+1):

        # 해당 스테이지에 머물러 있는 사람수 계산
        count = stages.count(i)

        # 실패율 계산
        if length == 0 :
            fail = 0 
        else : 
            fail = count / length 
        # 이 경우 멈춘사람이 없으면 count가 0이라 실패율이 0이 되고, 
        # 더 이상 검사할 플레이어가 없으면 남은 검사 스테이지들은 자동으로 실패율이 0이된다 

        # 리스트에 튜플(스테이지 번호, 실패율) 원소 삽입한다
        answer.append((i,fail)) 

        # 전체 플레이어의 수에서 현재 스테이지에 머무르고 있는 플레이어의 수를 뺍니다. 
        length -= count 
    
    # 실패율 기준으로 스테이지 내림차순 정렬 
        answer = sorted(answer, key = lambda t: t[1], reverse=True)
		
    # 배열 형태로 반환 
    answer = [i[0] # stage번호만 반환해 배열 생성 
              for i in answer] # 튜플값을 하나씩 꺼내서
    return answer 
```

<br>

 



##### sort() vs sorted()

sort 함수는 리스트명.sort( ) 형식으로 "리스트형의 메소드(list객체 내부의 메서드)"이며 리스트 원본값을 직접 수정합니다.

sorted 함수는 sorted( 리스트명 ) 형식으로 "내장 함수"이며 리스트 원본 값은 그대로이고 정렬 값을 반환

두 메서드 모두 인자로 정렬기준(key = lamba k: ) 를 넣어줄 수 있다. 

<br>

