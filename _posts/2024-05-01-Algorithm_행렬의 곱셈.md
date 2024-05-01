---
published: true
title: "Math-행렬의 곱셈" 
categories: Algorithm 
tag: [algorithm,Implementation, 카카오, String] 
toc: true
author_profile: false 
  
---



##### 💡 핵심 아이디어

 두 개의 [행렬](https://ko.wikipedia.org/wiki/행렬)에서 한 개의 행렬을 만들어내는 [이항연산](https://ko.wikipedia.org/wiki/이항연산)이다. 이 때 첫째 행렬의 열 개수와 둘째 행렬의 행 개수가 동일해야한다.

행렬 ![{\displaystyle A}](https://wikimedia.org/api/rest_v1/media/math/render/svg/7daff47fa58cdfd29dc333def748ff5fa4c923e3)와 의 곱은 간단히 ![{\displaystyle AB}](https://wikimedia.org/api/rest_v1/media/math/render/svg/b04153f9681e5b06066357774475c04aaef3a8bd)로 나타낸다.

![{\displaystyle c_{ij}=a_{i1}b_{1j}+a_{i2}b_{2j}+\cdots +a_{in}b_{nj}=\sum _{k=1}^{n}a_{ik}b_{kj},}](https://wikimedia.org/api/rest_v1/media/math/render/svg/ee372c649dea0a05bf1ace77c9d6faf051d9cc8d)

C행렬의 (i,j) 번째 값은 **A**의 *i*번째 행과 **B**의 *j*번째 열의 성분들을 각각 곱해 더한 것과 같다. 

<br>

<br>



##### ✅ 코드 

```java
class Solution {
    public int[][] solution(int[][] arr1, int[][] arr2) {
        int n = arr1.length ; // A 배열의 행갯수 
        int m = arr2[0].length ; // B 배열**의 열갯수 (A배열 X) 
        
        int[][] answer = new int[n][m];
        for(int i = 0 ; i < n ; i++){ 
            for(int j = 0 ; j < m ; j++){  
                answer[i][j] = getResult(arr1, arr2, i, j) ; 
            }
        }
        return answer ; 
    }
    
    public int getResult(int[][] arr1, int[][] arr2, int row, int col){
        int sum = 0;
        for(int i = 0 ; i < arr1[0].length ; i++){
            sum += (arr1[row][i] * arr2[i][col]) ; 
        }
        return sum ; 
    }
}
```

<br>

<br>

