---
published: true
title: "Implementation-블록게임" 
categories: Algorithm 
tag: [algorithm, Programmers, Implementation] 
toc: true
author_profile: false 
  
---



##### 문제

3 종류의 블록을 회전해서 총 12가지 모양의 블록을 만들 수 있다.

1 x 1 크기의 정사각형으로 이루어진 N x N 크기의 보드 위에 이 블록들이 배치된 채로 게임이 시작된다.

플레이어는 위쪽에서 1 x 1 크기의 검은 블록을 떨어뜨려 쌓을 수 있다. 이때, 검은 블록과 기존에 놓인 블록을 합해 **속이 꽉 채워진** 직사각형을 만들 수 있다면 그 블록을 없앨 수 있다.

보드 위에 놓인 블록의 상태가 담긴 2차원 배열 board가 주어질 때, 검은 블록을 떨어뜨려 없앨 수 있는 블록 개수의 최댓값을 구하라.

<br>



##### 핵심 아이디어

> 구현

‘테트리스’라는 현실의 사고(아이디어)를 코드로 나타내는게 까다로운 구현, 시뮬레이션 문제이다.

격자 안의 indexing과 수학적 사고(직사각형), 규칙성 발견이 핵심이다

<br>

⇒ 핵심 : 블럭을 없앨 수 없는 경우의 수

1. 불가능한 블럭 모양이다
2. 채워야할 부분 위에 다른 블럭이 있어 내려오지 못한다

<br>



##### 로직

1. 맨 위에 있는 블럭 먼저 하나씩 확인하면서
2. 블럭 타입 파악 : 없앨 수 있는 모양인지 확인
3. 채울 수 있는지 파악 : 위에 다른 블럭 겹쳐져 있는지 확인
4. 제거
5. 다시 맨 위의 블럭부터 검사

<br>



##### Point

- 블럭의 종류가 한정적 + 각 블럭마다 직사각형이 될 수 있는 방법이 정해져있음 (5가지)

  ⇒ 구현문제에서 이렇게 경우의 수가 적은 경우 하드코딩하는게 좋다 !

- 격자안에서 주변 좌표를 확인해야하는 경우 격자채로 확인하는 것이 아닌 좌표 pointer를 이용해 검사한다

  ex)블럭 모양 검사 : 블럭 모양 격자를 만들어 비교하는게 아니라 기준 좌표부터 떨어진 특정 위치에 있어야할 블럭이 있는지 검사한다.

  ⇒ Tip: 좌표 다루는게 너무 어려운 경우 그냥 일일히 넘겨준다 (꼼꼼함이 생명)

<br>



##### 코드

```java
class Solution {
    private int n;
    private int[][] board;

    public int solution(int[][] board) {
        this.board = board;
        n = board.length;
        int answer = 0;

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if (board[i][j] == 0) continue;

                if (isA(i, j)) {
                    if (drop(i + 1, j + 1, board[i][j]) && drop(i + 1, j + 2, board[i][j])) {
                        remove(i, j, i + 1, j, i + 1, j + 1, i + 1, j + 2);
                        j = -1;
                        answer++;
                    }
                } else if (isB(i, j)) {
                    if (drop(i + 2, j - 1, board[i][j])) {
                        remove(i, j, i + 1, j, i + 2, j, i + 2, j - 1);
                        j = -1;
                        answer++;
                    }
                } else if (isC(i, j)) {
                    if (drop(i + 2, j + 1, board[i][j])) {
                        remove(i, j, i + 1, j, i + 2, j, i + 2, j + 1);
                        j = -1;
                        answer++;
                    }
                } else if (isD(i, j)) {
                    if (drop(i + 1, j - 1, board[i][j]) && drop(i + 1, j - 2, board[i][j])) {
                        remove(i, j, i + 1, j, i + 1, j - 1, i + 1, j - 2);
                        j = -1;
                        answer++;
                    }
                } else if (isE(i, j)) {
                    if (drop(i + 1, j - 1, board[i][j]) && drop(i + 1, j + 1, board[i][j])) {
                        remove(i, j, i + 1, j, i + 1, j - 1, i + 1, j + 1);
                        j = -1;
                        answer++;
                    }
                }
            }
        }
        return answer;
    }

    private void remove(int x1, int y1, int x2, int y2, int x3, int y3, int x4, int y4) {
        board[x1][y1] = 0;
        board[x2][y2] = 0;
        board[x3][y3] = 0;
        board[x4][y4] = 0;
    }

    private boolean drop(int x, int y, int value) {
        for (int i = 0; i < x; i++) {
            if (board[i][y] == 0) continue;
            if (board[i][y] != value) return false;
        }
        return true;
    }

    private boolean isA(int x, int y) {
        int num = board[x][y];
        if (y + 2 >= n || x + 1 >= n) return false;
        return board[x + 1][y] == num && board[x + 1][y + 1] == num && board[x + 1][y + 2] == num;
    }

    private boolean isB(int x, int y) {
        int num = board[x][y];
        if (x + 2 >= n || y - 1 < 0) return false;
        return board[x + 1][y] == num && board[x + 2][y] == num && board[x + 2][y - 1] == num;
    }

    private boolean isC(int x, int y) {
        int num = board[x][y];
        if (x + 2 >= n || y + 1 >= n) return false;
        return board[x + 1][y] == num && board[x + 2][y] == num && board[x + 2][y + 1] == num;
    }

    private boolean isD(int x, int y) {
        int num = board[x][y];
        if (x + 1 >= n || y - 2 < 0) return false;
        return board[x + 1][y] == num && board[x + 1][y - 1] == num && board[x + 1][y - 2] == num;
    }

    private boolean isE(int x, int y) {
        int num = board[x][y];
        if (x + 1 >= n || y - 1 < 0 || y + 1 >= n) return false;
        return board[x + 1][y] == num && board[x + 1][y + 1] == num && board[x + 1][y - 1] == num;
    }
}
```

<br>



##### 깨달은 것

구현 아이디어를 떠올리는 것까진 괜찮지만 실제 코딩하는 부분에 있어 끈기, 꼼꼼함이 부족하다.

문제를 많이 풀어보기 + 오래 잡고있기(구현 문제 한정)를 연습해야겠다.

<br>
