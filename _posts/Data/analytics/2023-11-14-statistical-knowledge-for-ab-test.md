---
title: A/B 테스트를 위한 통계지식
author: yoongyoonge
date: 2023-11-14 21:00:00 +0900
categories: [Data, Analytics]
tags: [study, data analysis, data science, abtest, statistics, hypothesis testing]
render_with_liquid: false
---

> A/B테스트를 이해하기 위한 기초 통계 지식을 정리합니다. 기본적인 간단한 용어 설명이나 개념 설명정도의 포스트입니다.


## 표본크기(Sample Size)
- AB테스트는 통계적으로 의미있는 차이를 확인하는 테스트이기 때문에 충분한 표본 크기가 필요
- 표본 크기는 실험의 통계적 강도에 영향 
- Baseline Conversion Rate, Minimum Detectable Effect, Statistical Significance 를 기반으로 계산

- 표본이 얼마나 필요하냐라는 질문은 두 방안의 결과가 몇 % 정도 차이가 날 것이라고 기대하는가와 같음 
    - 표본이 많으면 많을수록 A안과 B안의 결과 차이가 작더라도 실험 결과가 통계적으로는 유의미할 수 있음
    - 표본이 작더라도 A안과 B안의 결과 차이가 크다면 마찬가지로 통계적으로 유의미 할 수 있음 

    - 예시 
        - 고객 100,000명 중 전환한 사람 1,000명 => 전환율 1%, 평균 객단가 100만원 => 매출액 10억원 차이 발생 
        - 고객 1,000명 중 전환한 사람 50명 => 전환율 5%, 평균 객단가 10,000원 => 매출액 50만원 차이 발생

참고자료: <br>
[A/B 테스트에 적정한 표본의 크기와 주의사항](https://brunch.co.kr/@539insight/143) <br>
[A/B 테스트 제대로 이해하기 시리즈](https://yozm.wishket.com/magazine/@platter/)<br>
[표본 크기 계산기](https://www.evanmiller.org/ab-testing/sample-size.html) <br>
[A/B 테스트 계산기 세팅 & 해석하기](https://brunch.co.kr/@539insight/140) <br>
[AB Testguide](https://abtestguide.com/calc/)

## 가설검정(Hypothesis Testing)
- 귀무 가설(Null Hypothesis, H0) 
    - 효과가 없다는 가설 
    - 차이는 우연 때문이라는 가설
    - 대립 가설의 반대 가설
- 대립 가설(Alternative Hypothesis, H1) 
    - 효과가 있다는 가설 
    - 귀무가설이 틀렸다는 것을 입증해서 A 그룹과 B그룹의 차이가 우연이 아니라는 사실을 보이기 위한 가설
    - 입증하여 주장하고자 하는 가설
- 귀무 가설과 대립 가설은 모든 상황을 커버해야하며 이 두 상황이 아닌 상황은 없어야 함
- 단, 두 개 이상의 실험이 필요할 경우 MAB(multi armed bandit) 등의 실험 설계가 필요 

- 1종 오류
    - 참인 귀무 가설을 기각하는 오류 
    - 보수적인 의사결정을 하귀 위해서는 유의 수준을 낮춰 1종 오류를 범할 확률을 낮춰야 함  
    - 귀무 가설이 참인 것을 전제로 함
- 2종 오류
    - 거짓인 귀무 가설을 기각하지 않는 오류 
    - 세운 가설이 맞음에도 맞다고 결론 짓지 않는 오류 
    - 대립 가설이 참인 것을 전제로 함

참고자료 <br>
[가설 검정이란?](https://wikidocs.net/196998)

## 통계 테스트 
- 실험 데이터를 사용하여 귀무 가설을 기각할지 또는 기각하지 않을지 결정하는 통계적 방법을 이해해야 함 
- 일반적으로 t-검정 또는 z-검정이 사용됨 

## 과도한 검정(Multi Testing)
- 여러 가설을 동시에 테스트할 때 과도한 검정 문제에 주의해야 함 
- Bonferroni 보정과 같은 방법을 사용할 수 있음 

## 신뢰구간(Confidence Interval) 
- 결과를 해석할 때 신뢰 구간을 사용하여 효과의 범위를 파악할 수 있음 


## 유의수준(Significance Level)
- 실험에서 어느 정도의 통계적 신뢰 수준을 원하는지 결정하는 것
- 일반적으로 0.05 (5%) 수준이 사용됨 


## p-value 
- 주어진 데이터에서 특정 검정 통계량(t-값 또는 z-값)을 얻을 확률
- 귀무가설을 구체화한 모델이 있을 때 관측된 결과가 특이하거나 극단적인 결과를 얻을 확률을 말함
- 작은 p값은 귀무 가설이 거부될 가능성이 크다는 것을 의미
- p값이 0.05라는 것은 우연히 얻은 결과의 5%가 관찰한 것만큼 극단적이거나 그 이상 극단적인 결과를 얻을 것으로 기대함 
- 따라서 p값이 낮을수록 좋다는 의미로 쓰며 이는 결과가 우연에서 비롯될 확률이 낮은 것을 뜻함 
- 하지만, 이 값이 유의미 하다고 해서 바로 증거로 사용하면 안되며 의사결정을 좌우하는 도구로 생각하면 안됨
    - p값은 데이터가 특정 통계 모델과 얼마나 상반되는지 나타낼 수 있음 
    - p 값은 연구 가설이 사실일 확률이나, 데이터가 랜덤하게 생성되었을 확률을 측정하는 것이 아님
    - p값 또는 통계적 유의성은 효과의 크기나 결과의 중요성을 의미하지 않음 

- p-value 를 관찰하면서 실험 기간을 무한정 늘릴 필요가 없고, 실험을 너무 일찍 끝내서도 안됨 

#### p-value와 유의 수준의 관계 
- 유의 수준은 귀무 가설을 기각하는 기준을 설정하는 것 
- p-value는 실제로 데이터에서 얻은 결과가 우연이 아닌지를 나타내는 지표 
- A/B테스트에서는 일반적으로 p값이 유의 수준인 0.05보다 작으면 귀무가설을 기각


참고자료
[A/B 테스트에서 p-value에 휘둘리지 않기](https://datarian.io/blog/dont-be-overwhelmed-by-pvalue) <br> 
[가설 검정이란?](https://wikidocs.net/196998)

<br><br>

참고자료 <br>
[AB 테스트 알아보기 - 1. 샘플 사이즈 계산 (with alpha, beta, power, critical value)](https://jinwoo1990.github.io/statistics/ab-test-sample-size/)