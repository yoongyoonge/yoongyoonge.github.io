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
- 귀무 가설(Null Hypothesis, H0): 효과가 없다는 가설 
- 대립 가설(Alternative Hypothesis, H1): 효과가 있다는 가설 

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



<br><br>

참고자료 <br>
[AB 테스트 알아보기 - 1. 샘플 사이즈 계산 (with alpha, beta, power, critical value)](https://jinwoo1990.github.io/statistics/ab-test-sample-size/)