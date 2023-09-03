---
title: YARN cluster mode에서의 spark application 흐름
author: yoongyoonge
date: 2023-09-03 18:45:00 +0900
categories: [Data Engineering, Apache Spark]
tags: [spark, study, deploy, spark-submit, yarn, cluster]
render_with_liquid: false
---

> **_NOTE:_** 이 포스트는 spark 배포 모드 중 YARN 클러스터 모드로 수행했을 때의 spark application의 흐름에 대한 내용을 담고 있습니다.

## 0. 기본 Spark Application 흐름
> 기본 [Spark Application 정리](https://yoongyoonge.github.io/spark-application/)는 링크된 글 내용 중 '2. Spark Application 흐름 정리'을 참고하면 됩니다. <br>


## 1. YARN Cluster Mode에서의 Spark Application 동작 흐름
> 많이 사용하는 cluster manager인 yarn cluster의 경우 어떻게 동작하는지 좀 더 자세히 살펴보겠습니다. <br>
> 위 링크 글과 이 글에서 나오지 않았던 yarn의 구성에 대한 용어가 나오기 때문에 yarn에 대해 배경지식을 알면 이해하기 좋습니다. <br>
> spark on yarn 에 대한 아주 좋은 ppt가 [여기](https://www.slideshare.net/FerranGalReniu/yarn-by-default-spark-on-yarn) 있습니다 이 부분은 해당 ppt를 참고하여 작성되었습니다. <br>

1) spark job code 및 아키텍처 <br>

- spark code
<pre>
    <code lang="scala">
    def main(args: Array[String]): Unit = 
    {
        val sparkConf = new SparkConf()
        val sc = new SparkContext(sparkConf)
    
        sc.rdd(...).action()
    
        sc.stop()
    }
    </code>
</pre>

- spark job 이 나누어지는 과정
<img width="1600" alt="image" src="https://github.com/yoongyoonge/yoongyoonge.github.io/assets/20895661/4324da81-8691-4629-ad25-1dfefd05d19d">
    - 하나의 spark application은 크게 driver에서 수행될 main과 executor에서 수행될 rdd(Dataset)부분으로 나눌 수 있음
    - executor에서 수행될 작업은 하나 이상의 Job으로 변환되고, 각 Job은 하나 이상의 Spark Stage를 가짐
    - Stage는 물리적인 실행 계획을 담은 DAG로, 여러 task를 가짐

<br>
<br>

- spark job이 spark application 수행 시 할당되는 과정
<img width="1600" alt="image" src="https://github.com/yoongyoonge/yoongyoonge.github.io/assets/20895661/5630f0d2-c68b-4eb6-b4d4-963319c567c4">
    - executor에서 실행되는 가장 작은 실행 단위인 task는 여러 executor에 분산되어 실행되며 여러 executor를 거쳐 stage task들이 조정됨


<br>
2) spark on yarn 

- yarn의 옵션 중 executor 관련한 configuration
    ![image](https://github.com/yoongyoonge/yoongyoonge.github.io/assets/20895661/8a5c3381-7214-4961-82bf-687db3e36df6)
    - 추가 옵션들은 추후 별도의 포스트로 작성예정!

<br>

> 사실 client모드와 cluster 모드의 차이점은 driver가 어디에 뜨는지에 큰 차이가 있고 이외의 작업은 아주 비슷하게 흘러갑니다.


- --master yarn --deploy-mode client    
    <img width="1300" alt="image" src="https://github.com/yoongyoonge/yoongyoonge.github.io/assets/20895661/fc03d181-8f40-4007-9984-9c7f71ca94fe">
    - (1) client에서 spark-submit을 통해 작업이 제출됨
        - 실행 코드와 환경 정보를 resource manager에게 넘기는데 이때 실행할 파일들은 application ID에 해당하는 HDFS 폴더에 미리 복사됨
    - (2) <span style="color:orange">client에서 driver가 실행</span>되고 driver는 resource manager에게 작업에 필요한 리소스를 요청
    - (3) resource manager는 node manager에 작업가능한 공간인 container를 전달받음
        - 이 때 node manager는 container를 대신 시작
        - resource manager는 컨테이너에 application master를 실행
        - application master가 리소스를 ressource manager에게 요청하고 resource manager는 데이터 지역성을 고려하여 리소스를 할당
    - (4) (1)에서 복사되었던 파일들을 HDFS에서 container로 가져와 작업 수행
        - node manager에 떠있는 application master가 task(executor)를 실행
        - task들은 작업 상황을 주기적으로 application master에 전달


- --master yarn --deploy-mode cluster   
    <img width="1014" alt="image" src="https://github.com/yoongyoonge/yoongyoonge.github.io/assets/20895661/9e3136bf-7072-4d13-acc1-3f56f3d1d505">
    - (1) client에서 spark-submit을 통해 작업이 제출됨
        - 실행 코드와 환경 정보를 resource manager에게 넘기는데 이때 실행할 파일들은 application ID에 해당하는 HDFS 폴더에 미리 복사됨
    - (2) <span style="color:orange">resource manager로부터 지정받은 임의의 node manager가 driver(container)를 수헹</span>
    - (3) driver는 resource manager에 작업에 필요한 리소스를 요청
    - (4) resource manager는 node manager에 작업가능한 공간인 container를 전달받음 
        - 이 때 node manager는 container를 대신 시작
        - resource manager는 컨테이너에 application master를 실행
        - application master가 리소스를 ressource manager에게 요청하고 resource manager는 데이터 지역성을 고려하여 리소스를 할당
    - (5) (1)에서 복사되었던 파일들을 HDFS에서 container로 가져와 작업 수행
        - node manager에 떠있는 application master가 task(executor)를 실행
        - task들은 작업 상황을 주기적으로 application master에 전달





참고: <br>
[Spark Submitting Applications](https://spark.apache.org/docs/latest/submitting-applications.html) <br>
[spark-in-action-Chapter10](https://livebook.manning.com/book/spark-in-action/chapter-10/1) <br>
[Spark local mode와 Cluster Manager 및 deploy mode](https://wooono.tistory.com/140) <br>
[Yarn by default (Spark on YARN)](https://www.slideshare.net/FerranGalReniu/yarn-by-default-spark-on-yarn)