---
title: Airflow Executor
author: yoongyoonge
date: 2023-09-04 20:00:00 +0900
categories: [Data Engineering, Apache Airflow]
tags: [study, airflow, data, pipeline, executor]
render_with_liquid: false
---

> **_NOTE:_** 이 포스트는 airflow의 executor에 대한 내용을 담고 있습니다.

## 1. Airflow Executor

- Executor는 작업 인스턴스가 실행되는 메커니즘
<br>
- 한 번에 하나의 executor만 설정 가능  
    - configuration file에 core 부분에 설정

<br>

- 설정예시  
    ```
    [core]
    executor = KubernetesExecutor
    ```
- 현재 설정된 executor 확인
    ```
    airflow config get-value core executor
    ``` 
<br>
    
- executor의 유형
    1) 스케줄러 프로세스 안에서 로컬로 작업하는 executor
    2) worker의 pool을 이용하여 원격으로 작업하는 executor
    
<br>

## 2. executor의 종류

## Local Executors

### Sequential Executor (default):
- sqlite를 사용하는 유일한 executor
- sqlite는 다중 연결을 지원하지 않기 때문
- 한 번에 하나의 작업만 수행할 수 있음

<br>

### Local Executor
- BaseExecutor에는 프로세스 생성 수를 제한할 수 있는 parallelism 파라미터가 있는데, 이를 0으로 제한하면LocalExecutor는 무제한으로 프로세스를 생성할 수 있음 

    ```text  
    - Unlimited Parallelism (self.parallelism == 0)
        - LocalExecutor는 execute_async가 호출될 때마다 프로세스를 생성하고 자체 프로세스에서 실행됨 
        - 작업이 실행되고 result_queue에 결과가 저장되면 프로세스는 종료됨
        - 이 방법에서는 task_queue를 위한 것들이 필요하지 않은데 그 이유는 task가 수신되자마자 새로운 프로세스가할당되기 때문
        - 이 전략에 사용되는 프로세스는 LocalWorker 클래스 

    - Limited Parallelism (self.parallelism > 0)
        - 시작할 때 parallelism 값과 동일한 프로세스를 LocalExecutor가 생성
        - task_queue를 사용하여 worker 사이의 task 작업을 조정하고 준비가 되자마자 작업을 수행
        -  LocalExecutor의 lifecycle 동안 worker 프로세스들이 task를 기다림
        - LocalExecutor가 shutdown 명령을 받으면 poison 토큰들을 worker에 전달해 종료시킴
        - 이 전략에 사용되는 프로세스는 QueuedLocalWorker
    ```

<br>

### Testing DAGs with dag.test()
- IDE에서 DAG를 디버깅 하려면 dag file에서 dag.test를 설정하고 단일로 직렬화된 Python 프로세스에서dag를 통해 실행할 수 있음
- 이 방식은 지원되는 모든 데이터베이스에서 사용할 수 있고 모든 작업이 단일 프로세스에서 실행되므로 빠르게 실패할수 있음
- execution_date와 같은 파라미터를 추가할 수 있음
- 사용방법
    ```python 
    if __name__ == "__main__":
        dag.test()
    ```

<br>
<br>

## Remote Executors

### Celery Executor
> airflow 2.7.0 부터 이 executor를 사용하기 위해서는 celery provider 패키지를 설치해야 합니다.

- CeleryExecutor는 worker의 수를 scale out하는 방법 중 하나
- 이를 작동시키려면 celery 백엔드 (RabbitMQ, Redis, Redis Sentinel, ...)을 설정해야함
- 또한, airflow.cfg가 executor 파라미터가 CeleryExecutor를 가르키도록 변경해주고 celery 세팅들을제공해야 함 

<br>

- airflow 구성 설정은 클러스터 전체에서 동일해야 함
- worker에서 실행되는 operator들은 해당 context의 종속성을 충족해야 함
- 예를 들어, HiveOperator를 사용하는 경우 해당 box에 Hive CLI를 설치해야하고 MysqlOperator를 사용하는경우 필요한 Python 라이브러리를 어떻게든 PYTHON_PATH에서 사용할 수 있어야 함
- worker는 DAGS_FOLDER에 access할 수 있어야 하고 사용자는 자체적으로 파일 시스템을 동기화 해야함
- 일반적인 설정은 DAGS_FOLDER를 Git repository에 저장하고 Chef, Puppet, Ansible 등 환경에 맞추어동기화를 하는 것
- 모든 box들에 공통 mount 포인트가 있는 경우, 그곳에서 파이프라인을 공유하는 것도 잘 작동
- Celery 위에 구축된 웹 UI는 Celery Flower로 worker들을 모니터 할 수 있음

<br>

- 주의사항
    - 백업 결과용 데이터베이스 백엔드를 사용해야 함
    - 'celery_broker_transport_options'에서 가장 오래 실행 중인 작업의 ETA를 초과하는 작업에 대한 시간 제한을 설정해야함
    - Redis Sentinel을 브로커로 사용하고 Redis 서버에 의해 비밀번호가 보호되고 있는 경우 'celery_broker_transport_options'에 Redis 서버의 암호를 지정해야 함
    - 작업자가 새로 만든 파일에 대한 권한을 설정하려면 'worker_umask'에서 umask를 설정해야 함
    - task는 리소스를 소비할 수 있기 때문에 worker_concurrency 작업을 실행할 수 있는 리소스가 충분히 worker에 있는지 확인해야 함
    - 각 브로커 백엔드에는 큐 이름에 제한이 있을 수 있음

<br>

- 아키텍처
    > 블로그 글 중 [Airflow 기본 구조](https://yoongyoonge.github.io/airflow-architecture/) 포스트 '2. Celery Executor가 적용된 airflow 아키텍처'를 참고하면 됩니다.

<br>

- 큐
    - CeleryExecutor를 사용할 때 task가 전송되는 Celery 큐를 지정할 수 있음
    - 큐는 BaseOperator의 속성이므로 모든 task을 모든 큐에 할당 할 수 있음
    - airflow.cfg의 default 큐는 operators -> default_queue에 정의되어있고 이는 지정되지 않은 경우 task가 할당되는 대기열과 airflow worker가 수신 대기하는 큐를 정의
    - airflow celery worker를 사용할 경우 큐는 하나 이상의 큐를 수신할 수 있음
    - 공백없이 쉼표로 구분된 큐 이름 집합을 제공할 수 있으며
    (ex. airflow celery worker -q spark, quark)이 경우 해당 worker는 지정된 큐에 연결된 작업만 선택
    - 이는 리소스 관점에서 전문적인 작업자가 필요한 경우나 환경 관점에서 볼 때 유용할 수 있음


### CeleryKubernetesExecutor
> airflow 2.7.0 부터 이 executor를 사용하려면 celery 및 cncf.kubernetes provider 패키지를 모두설치해야 함

- 사용자가 CeleryExecutor와 KubernetesExecutor를 동시에 실행할 수 있음
- executor는 task 큐 기반으로 task를 실행
- CeleryExecutor의 확장성을 상속하여 피크 시간의 높은 로드를 처리하고 KubernetesExecutor의 런타임격리를 처리
- 특정한 경우일 때 사용
    - 피크 시 예약해야 하는 task 수가 kubernetes 클러스터가 쉽게 처리할 수 있는 규모를 초과할 경우
    - 작업 중 비교적 작은 부분에는 런타임 격리가 필요
    - Celery worker에서 실행할 수 있는 작은 작업이 많지만 사전 정의된 환경에서 실행하는 것이 더 나은 리소스가 많이 필요한 작업이 있는 경우


### Kubernetes Executor
> 쿠버네티스 executor는 내용이 많아 링크로 첨부합니다.
> [Kubernetes Executor](https://airflow.apache.org/docs/apache-airflow/stablecore-concepts/executor/kubernetes.html) <br>
> (테스트 할 시간이 되거나 실무에 적용하게 된다면 정리예정)


### LocalKubernetes Executor
- LocalExecutor와 KubernetesExecutor를 동시에 실행
- task 큐를 기반으로 task를 실행
        
    



참고사이트:

[Airflow Executor](https://airflow.apache.org/docs/apache-airflow/stable/core-concepts/executor/index.html)
