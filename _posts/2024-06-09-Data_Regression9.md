---
published: true
title: "Regression 9) Binary Dependent Vairable" 
categories: Data/AI
tag: [Limited Depent Vairable, Linear Probabiity Model, Probit, Logit] 
toc: false
author_profile: false 
  
---

![계량경제학 정리_page-0052](https://github.com/Vida0822/Algorithm_Study/assets/132312673/25cef730-8b55-4960-acbd-1de9ce95fe6f)

![계량경제학 정리_page-0053](https://github.com/Vida0822/Algorithm_Study/assets/132312673/cee1f960-54c6-41f5-8295-85c2a7fb5929)
![계량경제학 정리_page-0054](https://github.com/Vida0822/Algorithm_Study/assets/132312673/eab9252e-9c79-418f-8e7d-492ceba2ef93)

* cdf (Cumulative Distribution Funcion, 누적 분포 함수)

  : '정규 분포의' 특정 값 이하의 누적 확률

  > 확률값은 대칭적인 관계 : 표준정규분포 N(0, 1)에 대해
  > $$
  > \Phi(-x) = 1 - \Phi(x)
  > $$



![계량경제학 정리_page-0054](https://vida0822.github.io/images/2024-06-09-Data_Regression9/cdf.png)


![계량경제학 정리_page-0055](https://github.com/Vida0822/Algorithm_Study/assets/132312673/28689bbf-e970-43e5-8b35-b2ffc0a166d5)
![계량경제학 정리_page-0056](https://github.com/Vida0822/Algorithm_Study/assets/132312673/24c3b936-fbba-4dc2-bcd2-e0f436400139)

#### MLE (Maximum Likelihood Estimation, 최대우도법) 

> 파라미터 θ=(θ1,⋯,θm) 로 구성된 확률밀도함수  P(x|θ) 에서 관측 표본 데이터 집합을 x=(x1,x2,⋯,xn) 일 때, 표본 -> 파라미터 추정 
>
> * x=(x1,x2,⋯,xn)  : 이미 관측된 고정값  
> * θ=(θ1,⋯,θm) : 아직 모르는 변수 (target) 



##### Likelihood 

관측 데이터가 특정 분포로부터 나왔을 가능도   <br> 

1. 표본 데이터 집합 관측 

```math
x = {1, 4, 5, 6, 9} 

```

2. 이 데이터가 추출되었을 것으로 생각되는 후보 분포 추출 

   ![계량경제학 정리_page-0054](https://vida0822.github.io/images/2024-06-09-Data_Regression9/likelihood_후보분포.png)

   

3. Likelihood(가능도) 구하기 

   

   

   : 

   





<br> 

##### Likelihood Function 









































