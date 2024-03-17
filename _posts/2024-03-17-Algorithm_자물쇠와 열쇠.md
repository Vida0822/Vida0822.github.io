---
published: true
title: "Implementation-자물쇠외 열쇠" 
categories: Algorithm 
tag: [algorithm, Programmers, Implementation, String] 
toc: true
author_profile: false 
  
---



##### 문제

( 링크 : https://school.programmers.co.kr/learn/courses/30/lessons/60059#qna )  

잠겨있는 자물쇠는 격자 한 칸의 크기가 **`1 x 1`**인 **`N x N`** 크기의 정사각 격자 형태이고 특이한 모양의 열쇠는 **`M x M`** 크기인 정사각 격자 형태.

열쇠는 회전과 이동이 가능하며 열쇠의 돌기 부분(1)을 자물쇠의 홈 부분(0)에 딱 맞게 채우면 자물쇠가 열리게 되는 구조. 

자물쇠 영역을 벗어난 부분에 있는 열쇠의 홈과 돌기는 자물쇠를 여는 데 영향을 주지 않지만, 자물쇠 영역 내에서는 열쇠의 돌기 부분과 자물쇠의 홈 부분이 정확히 일치해야 하며 열쇠의 돌기와 자물쇠의 돌기가 만나서는 안됩니다. 

자물쇠의 모든 홈을 채워 비어있는 곳이 없어야 자물쇠를 열 수 있습니다.

열쇠를 나타내는 2차원 배열 key와 자물쇠를 나타내는 2차원 배열 lock이 매개변수로 주어질 때, 열쇠로 자물쇠를 열수 있으면 true를, 열 수 없으면 false를 return 하도록 solution 함수를 완성해주세요.

<br>



##### 핵심 아이디어 

> Implementation (구현)

* simulation 중 격자 문제 --> 정교한 indexing이 중요 : 알고리즘 자체는 문제(사고) 그대로 따르면 됨 구현이 어렵        

* '회전과 이동'을 코드로 구현하는게 어려운 문제      

* 자물쇠외 열쇠의 크기가 20x20 이하로 매우 작기 때문에, 완전 탐색을 이용해 열쇠를 이동이나 회전시켜서 자물쇠에 끼워보는 작업을 전부 시도한다 

  

<br>



##### 로직 

> 회전 -> 이동 & 확인 

1. 회전이 먼저(반복문1) 

2. 이동(반복문2) : 그 키를 격자칸 하나하나 이동시켜서 

    	ㄴ 격자 크기가 굉장히 작음 : 완전 탐색

3. 열리는지 확인 (반복문3)    

<br> 

 

##### Point 

>  자물쇠를 전체 격자 '가운데'에 놓는 것이 핵심

단순히 키가 자물쇠의 오른쪽&아래에만 걸치는거면 키 위치 배열(keyAt[] []) 을 n+m 의 크기로만 둬도 괜찮. 

하지만 자물쇠의 왼쪽 & 위에도 키가 걸칠 수 있기 때문에 자물쇠가 전체 배열의 왼쪽 위에 위치하면 안됨.

자물쇠를 가운데 둔 전체 격자 배열 newLock를 선언하고, 

해당 격자위를 key가 걸치게끔 (n ~ 2*n 내에 속하도록) 이동,

 즉 key 배열 첫번째 격자가 m-n ~ 2*n 까지 이동하도록 한다.  



<br>



##### 구조 

* key : 키 모양 자체
* lock : 자물쇠 모양 자체 
* keyAt : 전체 격자 중 키가 놓인 위치
* newLock : 전체 격자 중 자물쇠가 놓인 위치 

<br>







##### 코드 

```java
import java.util.*;

class Solution {
    public static int n, m;

    public boolean solution(int[][] key, int[][] lock) {
        n = lock.length;
        m = key.length;

        // 새로운 자물쇠의 중앙 부분에 기존의 자물쇠 넣기 ** 
        int[][] newLock = new int[n * 3][n * 3];
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                newLock[i + n][j + n] = lock[i][j];
            }
        }

        // 회전
        for (int i = 0; i < 4; i++) {
            key = rotation(key);

            // 이동
            for (int j = n-m ; j < n * 2; j++) { 
                for (int k = n-m; k < n * 2; k++) {
                    int[][] keyAt = move(j, k, key);
                    
                    // 확인 
                    if (check(keyAt, newLock))
                        return true;
                }
            }
        }
        return false;
    }

    public int[][] rotation(int[][] key) {
        int[][] newKey = new int[m][m];

        for (int i = 0; i < m; i++) {
            for (int j = 0; j < m; j++) {
                newKey[j][m - 1 - i] = key[i][j];  // 암기 
            }
        }
        return newKey;
    }

    public int[][] move(int nx, int ny, int[][] key) {
        int[][] keyAt = new int[n * 3][n * 3];

        for (int i = 0; i < m; i++) {
            for (int j = 0; j < m; j++) {
                keyAt[nx + i][ny + j] = key[i][j];
            }
        }
        return keyAt;
    }

    public boolean check(int[][] keyAt, int[][] lock) {
        for (int i = n; i < n * 2; i++) {
            for (int j = n; j < n * 2; j++) {
                if (lock[i][j] + keyAt[i][j] != 1)
                    return false;
            }
        }
        return true;
    }
}

```

<br>



##### '회전' 구현

현재 행렬의 행을 역순으로 취하고, 각 행의 원소들을 열 인덱스에 대응하여 새로운 열에 배치합니다. 

=> 행 역순 + 행, 열 바꾸기 

```java
for (int i = 0; i < m; i++) {
	for (int j = 0; j < m; j++) {
        newKey[j][m - i - 1] = key[i][j];  // 암기 
    }
}
```

<br> 

##### 해설 아이디어 

자물쇠 위치 배열과 키 위치 배열을 따로 두지 않고 

자물쇠 배열에 키를 바로 꽂아(더해) 그 값이 1인지 확인한다

```python
for i in range(m) : 
    for j in range(m):
        new_lock[x+i][y+j] += key[i][j]

if check(new_lock) :
    # if new_lock[a][b] != 1 : return False (a : n ~ 2n)
    return True 

# 들어 맞지 않으면 열쇠 다시 빼기 
for i in range(m) : 
    for j in range(m):
        new_lock[x+i][y+i] -= key[i][j]
                                 
```

<br>

