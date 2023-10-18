---
title: 클린 코드 udemy 강의 - 좋은 코드에 대한 『클린코드』 역자의 생각들
author: yoongyoonge
date: 2023-10-17 20:30:00 +0900
categories: [Coding, Clean Code]
tags: [code, study, coding, clean code]
render_with_liquid: false
---

> **_NOTE:_** 이 포스트는 [클린코드 - 좋은 코드에 대한 '클린코드' 역자의 생각들](https://www.udemy.com/course/yeipvrxp/) 을 보고 정리한 포스트 입니다.


# Section 1: 1부 코드 리뷰

```text
코드를 잘 작성하는 방법

!) 코드를 돌게 만든다 > 코드를 제대로 돌게 만든다 > 코드를 빠르게 돌게 만든다
```

## 개발자가 신경을 써야하는 것 

### 1. commit
- commit은 하나의 작업 단위
- 해당 작업을 왜 했는지 명시하여 기록하기 위해 commit을 잘 써야함
- commit을 작성할 때에는
    - commit 메세지 맨 처음에는 동사 원형이 와야 한다.
    - commit에 묶인 작업이 독립적이라면 commit도 모두 분리를 해야한다.
    - 구현, 리팩터링, 단위 테스트, 설계 개선사항, document 등의 주요 일련의 행위들은 모두 개별 commit으로 작성해야한다.
    - commit은 SRP를 지키는 것이 좋다.
        - 단, 코드는 how이기 때문에 commit에는 why를 적을 것
        - !) SRP란?
            - Single Responsibility Principle, 단일 책임 원칙
    - commit의 본문은 한글로 적어도 되나 주제를 영어로 적는 연습을 하자


### 2. coding 시 Tip

- 즉시반환
    - 더 이상 계산할 필요가 없거나 오류 조건을 반환하거나의 작업이라면 즉각적으로 해당 코드 부분에서 즉시 반환 하도록 한다.
    - 뒤에서 쓸모없는 일을 하지 않게 되어 전체 속도를 떨어뜨리지 않고 CPU를 낭비하지 않게 된다.

- TODO
    - TODO로 표기하는 방식을 통해서 TODO를 통합적으로 관리(IDE 기능 사용)할 것
    - 추후 TODO 개발 시 관련있는 작업끼리 볼 수 있어 전반적으로 적용될 수 있는 통합 개발을 할 수 있다.
    - 임시로 사용하고 추후에 쓰지 않는 것을 표시하려면 XXX 키워드를 사용하도록 한다.

- 파라미터 선언 순서
    - 전달해야하는 파라미터를 앞에서 선언해야한다면 파라미터가 전달되는 순서대로 이전에 선언할 수 있도록 한다.

    ```java
    AuthInfoDto authInfoDto = transform(user, AuthInfoDto.class);
    String sessionId = createSessionId(userReqDto);
    sessionStorage.put(sessionId, authInfoDto)
    
    -> 다음과 같이 변경
    
    String sessionId = createSessionId(userReqDto);
    AuthInfoDto authInfoDto = transform(user, AuthInfoDto.class);
    sessionStorage.put(sessionId, authInfoDto)
    ```

- 주석을 작성할 때
    - class, method에 대한 전체 주석은 위에 작성하도록 한다.
    - 상수의 경우 그룹핑 할 경우 위에 적고 개별 상수는 옆에 적는다.
    - 코드 한 줄 자체가 짧을 때 옆에 적어도 상관 없다.
    - 코드에 대한 주석은 하지 않도록 한다. 중요도를 모르기 때문이다.

- 불필요한 주석
    - 오류 메세지 등으로 바로 알 수 있는 코드에 대해서는 굳이 주석을 달지 않아도 된다.


- 삼항 연산자
    - 기본적으로 클린코드 책에서는 삼항 연산자 사용을 지양한다.
    - 단, 삼항 연산 자체가 null 체크 등 명확할 경우 사용해도 괜찮다.

- 일관성
    - 반복적인 코드를 작성해야할 때 그 형태나 코드 자체에 다름이 없도록 해야한다.
    - 경로 등을 지정할 때 어떤 부분은 /a/b/c, 또 어떤부분은 a/b/c 이렇게 작성하면 혼란을 야기할 수 있다.
    - 변수명의 경우에도 같은 특성을 나타내는 변수들은 변수명도 일관적으로 생성해야한다.
    - 일관성이 없는 경우 사람은 코드를 해석 할 때 패턴을 찾으려고 노력하기 때문에 불필요한 시간을 쓰게 된다.

- 변수
    - 상수는 대문자로 선언하자.
    - 임의 지정 변수에 대한 주석은 확실하게 임의 지정의 사유를 주석으로 남기도록 한다.

- 함수
    - 함수 정의 시 위에 2줄의 공백을 만들고 작성하도록 한다.

- 객체지향에서
    - method를 함부로 추가하면 안된다.
        - method를 선언하면 해당 클래스를 상속받는 모든 코드에 영향이 있다.
        - 해당 클래스를 사용하는 모든 코드가 재컴파일이 필요하다. -> 새로 배포해야한다.


강의링크: <br>
[클린 코드 udemy 강의 - 좋은 코드에 대한 『클린코드』 역자의 생각들](https://www.udemy.com/share/1099Ng3@4FR55rWAAFHklQBRBOkBEVNXrv98JKYEugFz6PIi3n4Uivwz4Gphn9Hc6KnEEaQYkQ==/)

