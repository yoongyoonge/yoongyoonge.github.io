---
title: 시간 복잡도와 공간 복잡도
author: yoongyoonge
date: 2023-09-18 17:30:00 +0900
categories: [Data Engineering, Algorithm]
tags: [algorithm, study, time complexity, space complexity]
render_with_liquid: false
---

> **_NOTE:_** 이 포스트는 알고리즘의 성능을 평가할 수 있는 척도인 시간 복잡도와 공간 복잡도에 대한 내용을 정리합니다.


## 1. 시간 복잡도

- 알고리즘의 수행 시간 분석
- 최악의 경우를 계산하는 방식을 빅-오(Big-O) 표기법이라고 함

### 종류 

**(1) O(1) (Constant)** 
- 입력 데이터의 크기에 상관없이 언제나 일정한 시간이 걸리는 알고리즘  
- 데이터의 증가는 성능에 영향을 거의 미치지 않음 
- stack의 push, pop

```python 
def constant_time(n):
    print("cool")
```

<br>

**(2) O(log2 n) (Logarithmic)**
- 입력 데이터의 크기가 커질수록 처리 시간이 로그만큼 짧아지는 알고리즘 
- ex. 데이터가 10배가 되면 처리시간은 2배가 되는 경우 
- 이진탐색, 재귀가 순기능으로 이루어 지는 경우 

```python 
def logarithmic_time(n):
    i = 1
    while i <= n:
        print("cool")
        i *= 2
```
<br>

**(3) O(n) (Linear)**
- 입력 데이터의 크기에 비례해 처리 시간이 증가하는 알고리즘 
- ex. 데이터가 10배가 되면 처리 시간도 10배인 경우 
- 1차원 for문 

```python 
def linear_time(n):
    for i in range(n + 1):
        print("cool")
```
<br>


**(4) O(n log2 n) (Linear-Logarithmic)** 
- 데이터가 많아질 수록 처리시간이 로그 배만큼 늘어나는 알고리즘 
- ex. 데이터가 10배가 되면, 처리 시간은 약 20배가 되는 경우 
- 병합 정렬, 퀵 정렬, 힙 정렬 


**(5) O(n^2) (quadratic)** 
- 데이터가 많아질수록 처리시간이 급수적으로 늘어나는 알고리즘 
- ex. 데이터가 10배가 되면, 처리 시간이 약 100배가 되는 경우 
- 이중 루프(n^2 matrix)가 대표적, 삽입정렬, bubble sort, selection sort 
- 단, m이 n보다 작을 때는 반드시 O(nm)으로 표기해야 함 

```python 
def linear_time(n):
    for i in range(n + 1):
        print("cool")
```
<br>

**(6) O(2^n) (exponential)** 
- 데이터량이 많아질수록 처리시간이 기하급수적으로 늘어나는 알고리즘 
- 피보나치 수열이 대표적, 재귀가 역기능을 할 경우도 해당 

```python 
def exponential_time(n):
    if n <= 1:
        return 1
    return exponential_time(n - 1) + exponential_time(n - 2)
```
<br>

<br>

### 빅오 표기법간 비교

<span style="display:block; color:orange; font-weight:bold"> faster O(1) < O(log n) < O(nlog n) < O(n²) < O(2ⁿ) slower </span>

- 오른쪽으로 갈 수록 효율성이 떨어짐

<br>

### 측정 방법

```python
import time
start_time = time.time() # 측정 시작

###
# 프로그램 소스코드
###

end_time = time.time() # 측정 종료
print("time:", end_time - start_time) # 수행 시간 출력
```

<br>

### 시간 복잡도를 줄이는 방법
1. 반복문의 숫자를 줄이기
2. 적절한 자료 구조의 사용
3. 적절한 알고리즘의 이용

<br>

## 2. 공간 복잡도
- 알고리즘의 메모리 사용량 분석
- 작성한 프로그램이 얼마나 많은 메모리를 차지하는지 분석
- 시간과 공간은 반비례적 경향이 있음

### 공간의 종류

**(1) 고정 공간** 
- 알고리즘과 무관한 공간 
- 코드가 저장되는 공간, 알고리즘 실행을 위해 시스템이 필요로 하는 공간 


**(2) 가변 공간** 
- 알고리즘과 밀접한 공간 
- 문제를 해결하기 위해 알고리즘이 필요로 하는 공간 
- 변수를 저장하는 공간 
- 순환 프로그램일 경우 순환 스택 등

### 종류 

**(1) O(1)** 
- 일반적으로 공간이 하나 생성되는 것 
- 반복문이 N번만큼 반복해도 for 문안에서의 선언된 지역변수에 대한 변경만 일어난다면 이에 대한 공간 복잡도는 여전히 O(1) 

```python
def factorial(n):
    result = 1

    for i in range(1, n + 1):
        result *= i

    return result
```

<br>

**(2) O(n)** 
- 재귀함수의 경우 함수의 매개변수 n의 값에 따라 공간 복잡도가 달라지는데, 함수를 계속 호출될 때마다 stack에 모두 쌓이게 되어 공간복잡도는 늘어남 

```python
def factorial(n):
    if n > 1:
        return n * factorial(n - 1)
    else:
        return n
```

<br>


### 공간복잡도를 줄이는 방법
- 주로 배열의 크기가 몇인지, 동적 할당인지, 몇 번의 호출을 하는 재귀 함수인지 등이 공간 복잡도에 영향 <br>
- 시간적인 측면을 무시하고 공간 복잡도만 고려한다면 고정 공간 보다는 가변 공간을 사용할 수 있는 자료구조가 더 효율적


<br>
<br>


## 3. python 자료형 별 시간복잡도

[파이썬: 자료형 별 연산자의 시간복잡도(Big-O) 정리](https://duri1994.github.io/python/algorithm/python-time-complexity/)


<br>

참고 <br>
[[Algorithm] 시간 복잡도, 공간 복잡도](https://velog.io/@cha-suyeon/Algorithm-%EC%8B%9C%EA%B0%84-%EB%B3%B5%EC%9E%A1%EB%8F%84-%EA%B3%B5%EA%B0%84-%EB%B3%B5%EC%9E%A1%EB%8F%84) <br>
[시간복잡도(Time Complexity) 와 공간 복잡도(Space Complexity)](https://dev-cool.tistory.com/19) <br>
[공간복잡도](https://coding-factory.tistory.com/609)