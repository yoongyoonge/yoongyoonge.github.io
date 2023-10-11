---
title: Airflow Operator
author: yoongyoonge
date: 2023-10-10 20:00:00 +0900
categories: [Data Engineering, Apache Airflow]
tags: [study, airflow, data, pipeline, operator]
render_with_liquid: false
---

> **_NOTE:_** 이 포스트는 airflow의 operator에 대한 내용을 담고 있습니다.


## 0. Operator란?

- 사전 정의된 Task에 대한 템플릿으로 DAG에 정의할 수 있음
- 다양한 operator들이 있으며, 일부는 코어에 내장되어있고 이외의 operator들은 Provider 패키지를 통해 별도로 설치하여 사용할 수 있음

## 1. Operator의 종류
#### 기본제공 Operator
- BashOperator

    ```python
    #BashOperator 예시
    run_this = BashOperator(
        task_id="run_after_loop",
        bash_command="echo 1",
    )
    ```

- PythonOperator
    ```python
    # PythonOperator 예시
    for i in range(5):
    
        @task(task_id=f"sleep_for_{i}")
        def my_sleeping_function(random_base):
            """This is a function that will run     within the DAG execution"""
            time.sleep(random_base)
    
        sleeping_task = my_sleeping_function    (random_base=i / 10)
    
        run_this >> log_the_sql >> sleeping_task
    ```
    (참고) 데코레이터(@)란?
    - 기존 함수를 입력 받아서 기능이 추가된 새로운 함수 객체로 만들어주는 역할

        ```python
    
        # hello 함수 정의
        def hello():
            print("hello")
    
        # fn를 출력할 때 꾸며주는 deco 함수 정의
        def deco(fn):
            def deco_hello():
                print("*" * 20)    #기능 추가
                fn()               #기존 함수 호출
                print("*" * 20)    #기능 추가
            return deco_hello
    
        hello = deco(hello)   # hello 변수는 기능이 추가된 deco_hello 함수 객체를 바인딩
        hello()

        """ 결과
        ********************
        hello 
        ********************
        """

        # 이 때, @deco를 사용하면 객체를 바인딩 할 필요 없이 쉽게 위 처럼 작동 하게 할 수 있다.
        @deco
        def hello2():
            print("hello 2")

        hello2()

        """ 결과
        ********************
        hello 2
        ********************
        """

        ```
    - [데코레이터 참고 자료](https://wikidocs.net/160127)

- EmailOperator

#### Provider package를 통해 사용할 수 있는 주요 Operator

- MysqlOperator
- JdbcOperator
- DockerOperator
- HiveOperator
- ...


참고자료 <br>
[Airflow Operator](https://airflow.apache.org/docs/apache-airflow/stable/core-concepts/operators.html) <br>
[Airflow Operator and Hooks Reference](https://airflow.apache.org/docs/apache-airflow/stable/operators-and-hooks-ref.html)