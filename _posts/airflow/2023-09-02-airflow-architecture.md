---
title: Airflow 기본 구조
author: yoongyoonge
date: 2023-09-03 20:45:00 +0900
categories: [Data Engineering, Apache Airflow]
tags: [study, airflow, data, pipeline]
render_with_liquid: false
---

> **_NOTE:_** 이 포스트는 airflow의 기본 구조에 대한 내용을 담고 있습니다.


## 1. Airflow 구조
> airflow의 일반적인 설치는 다음과 같은 구조입니다.

* 공식홈페이지 아키텍처
![image](https://github.com/yoongyoonge/yoongyoonge.github.io/assets/20895661/d25dd5b8-1663-4a48-b961-1e9e9954b5fc)

* 상세 아키텍처
![image](https://github.com/yoongyoonge/yoongyoonge.github.io/assets/20895661/645a90f9-1267-40ac-b6f2-14422b3fce2f)


- **Scheduler**
    - 스케줄된 workflow(DAG)들을 트리거 하고 제출된 task들을 실행
    - 생성된 DAG를 지속적으로 모니터링하고 종속성 및 타이밍 구성에 따라 실행할 작업을 예약

<br>

- **Executor**  
    - task를 수행
    - 기본 airflow 설치에서는 스케줄러의 모든 것이 실행되지만, 대부분의 executor들은 task 실행을 worker에 push
    - 스케줄러와 통신하여 실행할 작업에 대한 정보를 받은 다음 작업을 실행하는 데 필요한 프로세스나 컨테이너를 시작
    - 인프라 및 요구 사항에 따라 LocalExecutor, CeleryExecutor, KubernetesExecutor 등 다양한 유형의 실행자가 있음

<br>

- **Message Broker**
    - CeleryExecutor가 사용되는 분산 설정에서는 스케줄러와 Worker의 통신을 관리하기 위한 메시지 브로커가 필요
    - RabbitMQ 또는 Redis와 같은 메시지 브로커는 작업 정보를 스케줄러에서 작업자로 전달하는 데 도움
    - 분산 환경에서 안정적이고 효율적인 작업 실행을 보장

<br>

- **Worker**
    - Executor가 할당한 작업을 수행하는 구성 요소
    - 선택한 executor에 따라 별도의 프로세스 또는 컨테이너일 수 있음
    - 작업에 정의된 실제 코드나 스크립트를 실행하고 해당 상태를 executor에게 다시 보고

<br>

- **Webserver**
    - DAG와 task의 동작을 트리거하고 디버그 하기 위한 사용자 인터페이스 제공
    - DAG를 시각화, 모니터링, 관리할 수 있음
    - 이슈 해결을 위한 대시보드 제공  

<br>

- **DAG Directory**  
    - 스케줄러와 executor가 읽는 DAG 파일이 있는 폴더

<br>

- **Metadata database**
    - 스케줄러, executor, 웹서버에서 상태를 저장하기 위해 사용하는 데이터베이스
    - 작업 및 실행 기록에 대한 정보를 저장
    - PostgreSQL, MySQL, SQLite, ... 등 지원

<br>
<br>


## 2. Celery Executor가 적용된 airflow 아키텍처
![image](https://github.com/yoongyoonge/yoongyoonge.github.io/assets/20895661/deeed307-27ba-43e8-bab1-1a5947dfed70)

airflow component
- worker: 할당된 작업을 실행
- scheduler: 필요한 작업을 큐에 추가
- web server: http 서버는 dag/task 상태 정보 제공
- database: task 상태, dag, 변수, 연결 등에 대한 정보 관리
- celery: 큐 매커니즘

celery component
- broker: 실행을 위한 명령 저장
- result backend: 완료된 명령 상태 저장

<br>
번호 별 흐름 <br>

(1) Web server –> Workers - task 실행로그 가져옴<br>
(2) Web server –> DAG files - dag 구조 전달<br>
(3) Web server –> Database - task 상태 가져옴<br>
(4) Workers –> DAG files - dag 구조를 전달하고 task 실행<br>
(5) Workers –> Database - 연결 정보, 변수, xcom에 대한 정보를 가져오고 저장<br>
(6) Workers –> Celery’s result backend - task 상태 저장<br>
(7) Workers –> Celery’s broker - 실행을 위한 명령 저장<br>
(8) Scheduler –> DAG files - dag 구조 전달 및 task 실행<br>
(9) Scheduler –> Database - dag 실행 및 관련 task 저장<br>
(10) Scheduler –> Celery’s result backend - 완료된 task 상태에 대한 정보를 가져옴<br>
(11) Scheduler –> Celery’s broker - 실행할 명령 입력<br>

추가 참고 자료:  
[Task execution process](https://airflow.apache.org/docs/apache-airflow/stable/core-concepts/executor/celery.html)


## 3. DAG의 기본 요소

1) Operator: DAG의 대부분을 차지하는 요소이며 빠르게 구성할 수 있는 사전 정의된 작업 <br>
2) Sensor: 외부 이벤트가 발생하기를 기다리는 operator의 특별한 하위 클래스 <br>
3) TaskFlow (@task): Task로 패키지된 사용자 정의 함수 <br>

---

- 내부적으로 이들은 모두 BaseOperator의 하위 클래스이며 Task와 Operator의 개념은 어느정도 상호 교환이 가능
- 하지만, 별도의 개념으로 생각하는 것이 유용
- 기본적으로 Operator와 Sensor는 템플릿이며 DAG 파일에서 이를 호출하면 작업을 만드는 일

- DAG는 여러 번 실행되도록 설계됨


<br>
<br>

**참고** <br>
[Airflow Architecture Overview](https://airflow.apache.org/docs/apache-airflow/stable/core-concepts/overview.html) <br>
[Airflow Architecture](https://medium.com/@binayalenka/airflow-architecture-667f1cc613e8)