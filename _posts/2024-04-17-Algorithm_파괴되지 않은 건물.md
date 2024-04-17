---
published: true
title: "Simulation-파괴되지 않은 건물" 
categories: Algorithm 
tag: [algorithm,Implementation, 카카오, Simulation] 
toc: true
author_profile: false 
  
---



##### ❔ 문제

카카오 lv3 . 파괴되지 않은 건물

https://school.programmers.co.kr/learn/courses/30/lessons/92335

<br>

<br>



##### ❌ 시간초과 : 완전 탐색

> O(K*N*M)  - K (Skill.length) * N (row) * M (col)

스킬을 뽑고 --> 해당 스킬에 해당하는 범위에 맞춰 공격/회복을 수행한다. 

만약 각 스킬마다 해당하는 범위가 board 배열 전체라면, 스킬을 뽑을때마다 board 배열을 완전탐색 해야한다. 

```java
class Solution {
    public int solution(int[][] board, int[][] skill) {
        for(int[] s : skill){
            if(s[0]==1){
                for(int i = s[1] ; i <= s[3] ; i++ ){
                    for(int j = s[2] ; j <= s[4]  ; j++){
                        board[i][j] = board[i][j] - s[5] ; 
                    }
                }
            }else{
                for(int i = s[1] ; i <= s[3] ; i++ ){
                    for(int j = s[2] ; j <= s[4]  ; j++){
                        board[i][j] = board[i][j] + s[5] ; 
                    }
                }
            }
        }
        
        int answer = 0 ;
        for(int i = 0 ; i < board.length ; i++){
           for(int j = 0 ; j < board[0].length ; j++){
               if(board[i][j] > 0)
                  answer ++ ;
            }
        }  
        return answer ;
    }
}
```

<br>

<br>



##### 💡 핵심 아이디어

> 구현 (Simulation) - 누적합 

변화하는 값들을 계속 읽어들인 다음에 한 번에 누적합으로 그 변화값의 결과를 낼 수 있다. 

(x1, y1) ~ (x2, y2)까지에 n만큼의 변화를 주고 싶다면, (x1, y1) = n, (x2+1, y1) = -n, (x1, y2+1) = -n, (x2+1, y2+1) = n 만큼의 값을 더해주면, 원하는 부분에 원하는 변화량만큼 값을 바꿀 수 있다.

한 번 5를 가지고 만들어볼까? (0,0 ~ 2,3 까지 영향을 주고 싶다)

[5, 0, 0, -5]
[0, 0, 0, 0]
[0, 0, 0, 0]
[-5, 0, 0, 5]

<br>

먼저 각 행마다 왼쪽에서 오른쪽으로 각각 누적합을 해준다.

[5, 5, 5, 0]
[0, 0, 0, 0]
[0, 0, 0, 0]
[-5, -5, -5, 0]

<br>

그 다음, 각 열마다 위에서 아래로 각각 누적합을 해준다.

[5, 5, 5, 0]
[5, 5, 5, 0]
[5, 5, 5, 0]
[0, 0, 0, 0]

=> 원하는 누적합 완성! 

<br>

<br>



##### 🌊 로직 

1. Skill을 하나씩 읽어와 각 누적합을 계산할 위치를 갱신한다 : 모든 스킬의 누적합 시작 위치 반영   - O(K)
2. 모든 누적 시작점으로 부터 변화값 가감  :   좌 -> 우 / 상 -> 하  - O(2*N*M) = O(N*M)

​	=> board 좌표 각각 최종 변화량 도출

3. board 배열에 좌표별 최종 변화량 반영 -> 0보다 크면 counting

<br>

<br>



##### ✅ 코드 

> O(K+N*M) 

```java
class Solution {
    public int solution(int[][] board, int[][] skill) {
        
        int row = board.length;
        int col = board[0].length;
        int[][] sum = new int[row+1][col+1]; // 공격 좌표 저장
        
        // Skill 뽑아서 누적합 시작점 준비 : O(K)
        for (int[] ints : skill) {
            int x1 = s[1] , y1 = s[2] , x2 = s[3] , y2 = s[4] ; 
            int degree = s[5] * (s[0] == 1 ? -1 : 1) ; 
            
            damage[x1][y1] += power; // x1 
            damage[x1][y2 + 1] -= power; // 
            damage[x2+1][y1] -= power;
            damage[x2+1][y2+1] += power;
        }

        // 누적합 계산 --> board 좌표 각각 최종 변화량 도출 : O(2*N*M) = O(N*M)
        // 가로로
        for(int i=0; i<row+1; i++) {
            for(int j=1; j<col+1; j++) {
                sum[i][j] += sum[i][j-1] ; 
            }
        }
        // 세로로
        for(int j=0; j<col+1; j++) {
            for(int i=1; i<row+1; i++){
               sum[i][j] += sum[i-1][j] ; 
            }
        }

        // 최종 누적합 board에 반영 + counting : O(N*M)
        int answer = 0;
        for(int i=0; i<row; i++) 
            for (int j=0; j<col; j++) 
                if(board[i][j] + sum[i][j] > 0) 
                    answer++;

        return answer;
    }
}
```

<br>

<br>



##### 🙇🏻‍♀️ 참고 해설 

해설 : [접근 방법. | 프로그래머스 스쿨 (programmers.co.kr)](https://school.programmers.co.kr/questions/25471)

공식 블로그 : [2022 카카오 신입 공채 1차 온라인 코딩테스트 for Tech developers 문제해설 – tech.kakao.com](https://tech.kakao.com/2022/01/14/2022-kakao-recruitment-round-1/)
