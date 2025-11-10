---
published: true
title: "ADSP 4) 상관 분석" 
categories: Data/AI
tag: [] 
toc: false
author_profile: false 
  
---

![JSP_page-0019](https://vida0822.github.io/images/2024-08-04-Data_Analysis4/KakaoTalk_20251028_093050766_15.jpg)



#### 종류 : Pearson VS Spearman 

##### Pearson 

두 **연속형 변수** 간의 **선형(linear) 관계**를 측정하는 상관계수이다.
즉, **직선 형태의 관계**가 얼마나 강한지를 나타낸다.
$$
r = \frac{\text{Cov}(X, Y)}{\sigma_X \sigma_Y}
$$


##### Spearman

**순위(rank)** 를 기준으로 두 변수 간의 **단조(monotonic) 관계**를 측정하는 비모수적 상관계수이다.

즉, “한 변수가 커질 때 다른 변수도 일관되게 커지거나 작아지는가?” 를 보는 방법으로 꼭 직선일 필요는 없습니다.





| 구분            | **Pearson (r)**   | **Spearman (ρ)**       |
| --------------- | ----------------- | ---------------------- |
| **데이터 형태** | 연속형 (정규분포) | 순서형 또는 비정규     |
| **관계 형태**   | 선형 관계만       | 단조 관계(비선형 포함) |
| **가정**        | 정규성, 선형성    | 없음 (비모수적)        |
| **이상치 영향** | 큼                | 작음                   |
| **대표 예시**   | 키와 몸무게       | 등수 간의 관계         |
| **검정 방법**   | 모수적            | 비모수적               |

