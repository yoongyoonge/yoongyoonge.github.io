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