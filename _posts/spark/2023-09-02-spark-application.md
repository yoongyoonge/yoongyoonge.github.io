---
title: Spark Application의 구조와 흐름
author: yoongyoonge
date: 2023-09-02 16:45:00 +0900
categories: [Data Engineering, Apache Spark]
tags: [spark, study, structure]
render_with_liquid: false
---

> **_NOTE:_** 이 포스트는 spark application의 개요에 대한 내용을 담고 있습니다.


## 1. Spark Components
> 전반적인 spark component는 다음과 같이 구성됩니다.

![image](https://github.com/yoongyoonge/yoongyoonge.github.io/assets/20895661/2086c0ef-6f6a-4959-bf4d-b856e31433db)

### Spark Application
- Spark에서 수행되는 프로그램, Driver와 여러 Executor로 구성


### Spark Driver
- SparkContext 및 SparkSession을 생성하는 프로그램
    - SparkContext: Driver를 통해 Cluster Manager에 연결하여 처리에 필요한 executor를 획득, 그 다음 직렬화된 결과를 실행할 executor에게 전송
- Spark 작업을 제출하자마자 Driver는 Application의 main()을 실행하고 내부적으로 데이터 흐름을 나타내는 DAG를 생성
    - DAG를 생성 함으로써 application 코드를 Task(transformation & action)로 변환 
    - DAG에는 실행 계획, 논리 계획, 물리 계획이 있음
- Cluster Manager에게 처리에 필요한 리소스(executor) 할당을 요청
    - Spark Application이 필요한 Executor 수와 Executor에서 사용 가능한 CPU core 수를 확보
    - Application이 종료될 때, SparkContext가 자원을 해제하면서 Executor  프로세스 종료
- 리소스가 할당되면 직렬화된 결과(코드+데이터)를 Worker에 전송

### Spark Worker
- 클러스터 전체에 분산되어 있음
- 데이터 처리를 위한 core라는 대역폭이 있음
- 사용 가능한 core 크기에 따라 driver에서 작업을 선택하여 데이터에 대한 코드 논리를 처리하고 데이터를 메모리나 디스크 저장소에 보관
- cluster manager에게 리소스 가용성을 알림

### Spark Executor
- executor는 Worker 노드의 프로세스
- 수명 주기는 Spark Application과 같으며 application이 시작할 때 시작되어 종료될 때 종료
- 작업을 실행한 후에는 결과를 드라이버에 전송
- Block Manager를 통해 사용자 프로그램에 의해 캐시되는 RDD에 대한 메모리 내 저장소를 제공


### Cluster Manager
- 클러스터의 다양한 작업을 실행 관리
- 작업에 리소스를 할당하는 작업을 수행
- 여러 종류가 존재
    - Standalone: 클러스터를 쉽게 설정할 수 있도록 Spark에 포함된 간단한 클러스터 관리자
    - Apache Mesos: 하둡 MR 및 서비스 application을 실행할 수 있는 일반 클러스터 관리자 (Deprecated)
    - Hadoop YARN: 하둡 2, 3의 리소스 관리자
    - Kubernetes: 컨테이너화된 application의 배포, 확장 및 관리를 자동화하기 위한 오픈 소스 시스템 (Spark 2.3 ~)

<br>
<br>

## 2. Spark Application 흐름 정리
> spark-submit을 하게 되면 다음과 같은 흐름으로 실행됩니다.

1. SparkContext 인스턴스를 시작하고 인스턴스화 <br> (Driver가 main()을 실행하여 SparkSession 실행)
2. Driver program(SparkContext)은 executor를 시작하기 위해 cluster manager에게 리소스를 요청
3. cluster manager가 executor를 시작
4. Driver program은 사용자 애플리케이션을 통해 수행. RDD 작업의 action과 transformation에 따라 executor에 전송 <br>
SparkContext가 작업할 내용(Job)을 task 단위로 분할하여 Executor로 전송
5. executor는 task를 실행하고 결과를 저장
6. 만약 어떤 worker가 이상하면 다른 executor에게 task가 보내지고 다시 처리됨
7. driver에서 SparkContext.stop()을 하거나 main이 종료되거나 이상이 날 경우 모든 executor가 종료되고 cluster manager가 클러스터 리소스를 해제


<br>
<br>

참고: <br>
[Spark-Cluster Mode Overview](https://spark.apache.org/docs/latest/cluster-overview.html) <br>
[What is Apache Spark Driver?](https://sparkbyexamples.com/spark/what-is-apache-spark-driver/) <br>
[클러스터 리소스 최적화를 위한 Spark 아키텍처](https://www.samsungsds.com/kr/insights/spark-cluster-job-server.html)