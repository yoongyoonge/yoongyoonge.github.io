---
title: Spark Deploy Mode
author: yoongyoonge
date: 2023-09-02 23:00:00 +0900
categories: [Data Engineering, Apache Spark]
tags: [spark, study, deploy, spark-submit]
render_with_liquid: false
---

> **_NOTE:_** 이 포스트는 spark 배포 모드에 대한 내용을 담고 있습니다.


## 1. Spark Deploy Mode
> spark의 배포 모드에는 다음의 세 가지 모드가 있습니다.

### Local Mode
- 로컬 단일 머신에서 spark application 실행
- 주로 spark application 테스트용으로 사용
- 클라이언트 JVM에 driver 1개, executor 1개를 생성
    - 단, executor에서 여러 개의 core(thread)를 사용하여 task를 병렬로 실행할 수 있음
- 참고그림  

    ![image](https://github.com/yoongyoonge/yoongyoonge.github.io/assets/20895661/94c0d76d-3212-4304-b4d3-3a7411e3d81b)
    

- 작동방법
    - 명령어
        ```shell  
        spark-submit --master {master-url} ...
        ```
    - {master-url}
        - local: 하나의 worker thread를 이용해서 로컬에서 실행 (병렬처리 하지 않음)
        - local[k]: k개의 worker thread를 이용해서 로컬에서 실행(k는 컴퓨터의 core를 고려하여 부여)
        - local[*]: 로컬 머신의 논리 core 수만큼의 worker thread를 가지고 로컬에서 실행
    - 예시
        ```shell  
        # 로컬의 가능한 모든 코어를 사용
        spark-submit --master local[*] ... 
        # 하나의 executor만 사용
        spark-submit --master local ... 
        ```


### Local Cluster Mode
- 로컬 단일 머신에서 spark application 실행
- 단위 테스트에만 적용
- local 모드와 다른점은 master와 worker가 있다는 점이며 master와는 다른 JVM에서 worker가 실행
- 작동방법
    - 명령어
        ```shell  
        spark-submit --master {master-url} ...
        ```
    - {master-url}
        - local-cluster[N,C,M]: N개의 worker, worker의 M(Mib) 메모리, worker 당 C(Core)를 가지고 작업을 실행

### Client Mode
- Spark Application을 실행(spark-submit을 수행)하는 노드에서 Driver 실행
- Cluster 모드와는 Driver를 어디에서 실행시키느냐에 따라 다름
- Driver와 Spark application은 모두 application을 실행시킨 서버에 있음
- 주로 개발과정에서 spark-shell 수행 시 디버깅 등의 용도로 사용됨
    - 따로 지정하지 않으면 기본으로 선택되는 모드
- 참고 그림

    ![image](https://github.com/yoongyoonge/yoongyoonge.github.io/assets/20895661/6ed6ae84-71dc-4f85-acfb-ce042c6f5be0)

- 작동방법
    - 명령어
        ```shell  
        spark-submit --master {master-url} --deploy-mode {deploy-mode} ...
        ```
    - {master-url}
        - 클러스터 매니저
            - ex: yarn, k8s://xx.yy.zz.ww:443, spark://207.184.161.138:7077, ...
    - {deploy-mode}
        - client: 작성하지 않아도 default 모드
    - 예시
        ```shell  
        # yarn을 이용하여 executor에 20G메모리를 부여하고 client 모드로 배포
        spark-submit --master yarn --executor-memory 20G ... 
        # spark standalone 클러스터에 client 모드로 배포
        spark-submit --master spark://207.184.161.138:7077 ... 
        ```


### Cluster Mode
- Cluster에 속하는 노드 중 임의의 노드에서 Driver 실행
- spark application은 클러스터에서 독립적인 프로세스들의 집합으로 실행
- driver의 sparkContext를 통해 조정
- 참고 그림

![image](https://github.com/yoongyoonge/yoongyoonge.github.io/assets/20895661/d94def77-c8ec-43d2-a7ed-d9b0e3826fc3)
- 작동방법
    - 명령어
        ```shell  
        spark-submit --master {master-url} --deploy-mode {deploy-mode} ...
        ```
    - {master-url}
        - 클러스터 매니저
            - ex: yarn, k8s://xx.yy.zz.ww:443, spark://207.184.161.138:7077, ...
    - {deploy-mode}
        - cluster: cluster 모드로 수행하고 싶은 경우 작성, 작성하지 않으면 client 모드로 수행
    - 예시
        ```shell  
        # yarn cluster에 executor 20G메모리를 부여하고 executor를 50개를 사용하는 spark application을 cluster모드로 배포
        spark-submit --master yarn --deploy-mode cluster --executor-memory 20G --num-executors 50 ... 
        # kubernete 클러스터에 cluster 모드로 배포
        spark-submit --master k8s://xx.yy.zz.ww:443 --deploy-mode cluster ... 
        ```

!) yarn cluster 모드에서의 spark application 흐름은 다음의 포스트에 자세히 작성 해두었습니다.
[YARN cluster mode에서의 spark application 흐름](https://yoongyoonge.github.io/spark-deploy-mode/)


참고: <br>
[Spark Submitting Applications](https://spark.apache.org/docs/latest/submitting-applications.html) <br>
[spark-in-action-Chapter10](https://livebook.manning.com/book/spark-in-action/chapter-10/1) <br>
[Spark local mode와 Cluster Manager 및 deploy mode](https://wooono.tistory.com/140) <br>
