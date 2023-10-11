---
title: spark + yarn 옵션
author: yoongyoonge
date: 2023-09-25 21:30:00 +0900
categories: [Data Engineering, Apache Spark, Yarn]
tags: [spark, hive, study, option]
render_with_liquid: false
---

> 스파크와 하이브를 함께 사용할 경우 고려해야하는 옵션들에 대해 정리합니다.

## 1. spark.sql.hive.convertMetastoreOrc

설명
- hive ORC 형식의 데이터를 내부 최적화를 위해 spark의 고유 형식으로 변환할지 여부를 지정
- 기본값은 true

영향
- true인 경우 복수의 spark 어플리케이션이 같은 table의 다른 partition에 동시 작업을 할 수 없는 문제가 있음
    - 하위 디렉토리를 읽을 수 없음: https://issues.apache.org/jira/browse/SPARK-28098
    - dynamic partitioning을 사용했을 때 필요 이상으로 많은 hive partition 정보를 hive metastore로부터 얻음: https://issues.apache.org/jira/browse/SPARK-38230
    
    해결
    - spark.sql.hive.convertMetastoreOrc를 false로 설정
    - true로 설정하고 싶은 경우
        - spark.sql.sources.partitionOverwriteMode를 dynamic으로 설정하고
        - INSERT 시 SELECT 한 데이터로부터 동적으로 partitioning이 가능한 SQL을 작성
        ```SQL  
        ex) INSERT OVERWRITE ... PARTITION (dt, hour)
        ```