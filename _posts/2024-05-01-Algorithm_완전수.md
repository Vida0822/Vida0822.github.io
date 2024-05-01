---
published: true
title: "Math-완전수" 
categories: Algorithm 
tag: [algorithm,Implementation, 카카오, String] 
toc: true
author_profile: false 
  
---



##### 💡 핵심 아이디어

완전수 : 자기자신 제외 자신의 약수들을 합한 값이 자기자신과 같은 수이다. 

대표적으로  6,28,496,8128가 있다. 

<br>

<br>



##### ✅ 코드 

```c
#include <stdio.h>
int test(int n){
    int i , sum = 0 ; 

    for(i = 1 ; i <= n/2 ; i++){ // 자기자신을 제외한 약수 중 최댓값은 절반값만 될 수 있다. 
        if(n%i == 0)
            sum+= i; 
    }
    if(n==sum)
        return 1 ; 
    return 0 ; 
}

int main(){
    int sum = 0; 
    for(int i = 2 ; i <= 100 ; i++){
        if(test(i)) // i가 완전수이면 
            sum+= i ; 
    }
    printf("%d", sum) ; 
    return 0 ; 
}
```

<br>

<br>

