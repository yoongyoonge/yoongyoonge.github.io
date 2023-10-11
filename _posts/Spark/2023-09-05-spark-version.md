---
title: spark 버전 별 차이
author: yoongyoonge
date: 2023-09-05 21:30:00 +0900
categories: [Data Engineering, Apache Spark]
tags: [spark, study, version]
render_with_liquid: false
---

> **_NOTE:_** 이 포스트는 spark의 버전 별 차이가 궁금해서 기록해 두는 포스트 입니다.

## 1. SPARK ver. 1.x
(1) RDD를 이용한 인메모리 처리로 기존 방식 대비 빠른 속도로 처리 가능  <br>
(2) v1.3: 데이터 프레임 추가  <br>
(3) v1.6: 데이터셋 추가(데이터 타입체크, 인코더, 카탈리스트 옵티마이저 지원)  <br>


## 2. SPARK ver. 2.x
(1) RDD 기능 개선 <br>
(2) dataset + dataframe  <br>
  : Spark 2.0 부터 DataFrame이 Dataset과 병합되어 Dataset이 되었음  <br>
(3) 데이터를 스키마 형태로 추상화하여 제공 <br>
(4) 쿼리 최적화 기능 추가 <br>

## 3. SPARK ver. 3.x

(1) Spark SQL

- spark 2.x -> 3.x 패치 중 약 46%가 spark SQL

- 일부 예약된 키워드를 식별자로 사용할 수 없음

    ```sql
    select * from some_table create where create.some_column = 1
    ```

  - 기존에는 create라는 키워드를 문제없이 사용가능 했음
  - ANSI SQL 표준을 준수할 수 있게 되면서 ```spark.sql.ansi.enabled``` 속성을 true로 설정하면 일부 키워드에 대한 사용을 막을 수 있음

<br>

- 엄격한 데이터 품질 관리

    ``` sql
    CREATE TABLE assignment_policy_check(col INT);
    INSERT INTO assignment_policy_check VALUES("123")
    SELECT * FROM assignment_policy_check
    
    >>> 1, 123
    ```

  - 기존에는 정수형 컬럼에 문자열을 넣는 작업이 수행가능했음
  - ```spark.sql.storeAssign,entPolicy```를 ANSI로 설정하면 데이터 타입에 맞지 않는 값을 입력되지 않게 할 수 있음
  
<br>

- string을 날짜형(Date/Timestamp)으로 변환

    ```sql
    SELECT timestamp '{시간}'
    ```

    ```python
    spark.sql("SELECT timestamp 'now' as current_time").show(1, False)
    
    >> 2023-09-06 09:16:33.843
    ```

  - {시간}에는 다음과 같은 값들이 올 수 있음
    - epoch: 1970-01-01 00:00:00+00 (Unix system time의 0)
    - today: 오늘 자정
    - yesterday: 어제 자정
    - tomorrow: 내일 자정
    - now: 쿼리 하는 순간의 시간

<br>

- 서브 쿼리 안의 WITH 절
  - CTE(common table expression)을 서브 쿼리 내에서 사용할 수 있음

    ```sql
    select * 
    from (
        with inner_cte as (
            select * from some_table where num = 0
        )
        select num from inner_cte
    )
    ```

<br>

- filter (where ...)

    ```sql
    SELECT
     count(*) filter (WHERE num > 4) as filtered
        count(*) as no_filtered
    FROM 
     some_table
     
     // 필더링된 카운트와, 안된 카운트가 각각 출력된다.
     >> filtered, no_filtered
     >> 3, 6
    ```

<br>

- count_if
  - filter (where ...)와 비슷하며 만족하는 레코드 수를 반환

    ```sql
    SELECT count_if(num % 2 == 0) FROME some_table
    >> num % == 0를 만족하는 레코드 수를 반환
    ```

  - 불린 값에 대한 연산 결과 제공

    ```sql
    -- boolean_column에 true, false, true 값이 있다고 할 경우
    
    SELECT bool_and(boolean_column) FROM some_table
    >> false // (= true and false and true)
    
    SELECT bool_or(boolean_column) FROM some_tabel
    >> true // (= true or false or true)
    ```

<br>

- Every / Any / Some
  - 표현식을 만족하는지에 대한 boolean 값을 리턴
  - Every는 모든 값이 표현식을 만족해야 true, Any와 Some은 하나라도 만족하면 true

    ```sql
    // some_table에 1, 2 두개의 값이 있다고 가정
    SELECT Every(num % 2 = 0) FROM some_table
    >> false
    
    SELECT Any(num % 2 = 0) FROM some_table
    >> true
    
    SELECT Some(num % 2 = 0) FROM some_table
    >> true
    ```

<br>

#### (2) json 옵션

- FAILFAST/PERMISSIVE
  - 해당 모드가 추가되면서 json이 잘못되거나 파싱 할 수 없을 때 에러를 발생 시킬 수 있음

    ```python  
    df.withColumn("colname", from_json(df['colname'], jsonSchema, {'mode':FAILFAST'})).show()
    >> 파싱할 수 없을경우 에러 발생
    ```

<br>

#### (3) 지수표기법

- spark에서의 지수 표기법이 추가 되었음

    ```python
    # 스파크 2.4
    df = spark.sql("SELECT 1E10")
    df.printSchema()
    
    >> root
    >> |-- 1.0E10 : double (nullable = false)
    
    
    # 스파크 3.0 이면서 spark.sql.legacy.exponentLiteralAsDecimal.enabled’ 이 true로 설정.
    # 예전처럼 해석하고 싶다면 spark.sql.legacy.exponentLiteralAsDecimal.enabled를 false로 설정 필요
    df = spark.sql("SELECT 1E10")
    df.printSchema()
    
    >> root
    >> |-- 10000000000 : decimal(11,0) (nullable = false)
    ```

<br>

#### (5) -0.0 과 0.0

- spark 2.4 버전 이하에서는 float/double 형 -0.0과 0.0이 의미적으로 동일함에도 불구하고 groupby, partition, join에서 키 값으로 사용될 경우 다른 값으로 간주되었음
- spark 3.0에서 해당 버그를 수정하였음

    ```python
    df.show()
    >>
    +----+------+
    | num|   val|
    +----+------+
    | 0.0|   one|
    |-0.0|   two|
    +----+------+
    
    # 스파크 2.4 이하
    df.groupBy('num').agg(count('*')).show()
    >>
    +----+--------+
    | num|count(1)|
    +----+--------+
    | 0.0|       1|
    |-0.0|       1|
    +----+--------+
    
    #스파크 3.0
    df.groupBy('num').agg(count('*')).show()
    >>
    +----+--------+
    | num|count(1)|
    +----+--------+
    | 0.0|       2|
    +----+--------+
    ```

<br>

#### (6) AQE, Adaptive Query Execution

- 쿼리가 수행되는 과정에서 계획 초기에 부정확하거나 누락된 통계, 잘못 추정된 비용으로 다른 Query를 계획해야하는 경우 발생
- AQE는 runtime에 더 나은 실행계획을 생성(reoptimizing and adjusting)해 성능을 향상하고 튜닝을 단순화

<br>

- spark catalust optimizer history
  - spark 1.x: rule-based optimizer
  - spark 2.x: rule + cost-based optimizer
  - spark 3.x: rule + cost + runtime based optimizer (AQE)

<br>

- 구체화(materialization)지점: suffle, broadcast와 같은 동작을 수행하기 전(다음 stage로 가기 전), Data pipeline이 끊기고 실행을 멈추는 포인트
- 중간 결과물을 구체화하고, 해당 stage의 병렬처리가 완벽하게 끝나야 다음 스테이지 동작, 이 때 reoptimization을 할 기회가 생김
- 최적화는 통계치를 바탕으로 runtime에 진행

<br>

- Three major adaptive optimizations
  1. Dynamically coalescing shuffle partitions (동적 셔플 파티션 통합)
  
      - 셔플은 비용이 높은 연산으로 쿼리 성능에 매우 큰 영향을 미치므로 적절한 수의 partition 을 결정하는 것이 중요
        - 너무 적은 셔플 파티션은 GC 부하가 있고 disk spill이 일어나며 쿼리가 느려지는 결과를 초래
        - 너무 많은 셔플 파티션은 I/O가 비효율적으로 동작하고 스케줄 부하가 생길 수 있음

      <br>

      - AQE는 shuffle이 끝난 다음 partition을 병합
      - 너무 많은 partition들로 인해 비효율적인 task 및 I/O가 발생하는 것을 방지
      - 관련옵션: ```spark.sql.adaptive.enabled, spark.sql.adaptive.coalescePartitions.enabled``` (spark 3.2부터 default값은 true)
      - AQE 기능이 기본적으로 병합을 실행하므로 충분히 많은 수의 partition을 설정해야 하는데 ```spark.sql.adaptive.coalescePartitions.initialPartitionNum```으로 설정할 수 있으며 설정되어있지 않는 경우 ```spark.sql.shuffle.partitions```를 따라감
      > !!) 두 값이 동시에 설정되어있다면 후자의 옵션이 우선적으로 적용되기 때문에, AQE가 중간에 partition을 병합했다 하더라도 다음 shuffle이 일어날 때 다시 해당 값에 맞추어 partition 개수가 조정됨
      -> 따라서, 최초 한 번은 spark.sql.shuffle.partitions의 값을 크게 부여하고 실행해본 다음 shuffle partition 수 중 가장 큰 값을 반올림한 값으로 다시 설정하는 것을 추천
      - output partition이 데이터 크기에 맞지않게 많이 생성되어 있을 수 있는데 이는 ```spark.sql.adaptive.coalescePartitions.parallelismFirst```의 옵션의 영향
      > !!) 기본값은 true인데 이 경우, 병렬성을 우선적으로 고려해서 할당한 코어를 최대한 많이 사용하려고 하여 ```spark.sql.adaptive.advisoryPartitionSizeInBytes```로 설정되는 최종 partition의 크기는 무시되며, ```spark.sql.adaptive.coalescePartitions.minPartitionSize```(default 1MB)로 최종 partition 크기가 결정됨
      > 따라서 spark 공식 문서에서는 이 값을 false로 설정할 것을 권장하고 있으며, false로 설정할 경우, 최종 partition 크기는 ```spark.sql.adaptive.advisoryPartitionSzieInBytes```가 됨

      - 관련옵션 정리
      ![image](https://github.com/yoongyoonge/yoongyoonge.github.io/assets/20895661/28af2929-198c-426f-9a6d-d12c2159da93)

      <br>

추가할 항목

(1) 딥러닝 지원 강화(gpu 지원 추가) <br>
(2) 바이너리 파일 지원 <br>
(3) 쿠버네티스 지원 강화 <br>
(4) 다이나믹 파티션 프루닝 지원 <br>

## Spark 3.0 요약

- Spark 2.4 보다 약 2배 속도 향상
  - 적응 쿼리 지원
  - 동적 파티션 pruning
  - 이외의 최적화 도입  
  
- Pyspark 기능 및 사용성 개선
  - Pandas UDF API 개선
  - Python Error 핸들링 개선

- structured streaming UI
- R UDF 호출 시 최대 40배 속도 향상
- Accelerator 키 인식 스케줄러
- SQL 참조 문서
- ANSI SQL 준수

<br>

참고:

[spark-release-3.0.0](https://spark.apache.org/releases/spark-release-3-0-0.html)
[spark 버전 별 특징](https://wikidocs.net/80412) <br>
[spark 3.0 한번에 정리하기](https://dalsacoo-log.tistory.com/entry/Spark-30-%ED%95%9C-%EB%B2%88%EC%97%90-%EC%A0%95%EB%A6%AC%ED%95%98%EA%B8%B0) <br>
[Apache Spark 3.0 변경점/변화 총 정리](https://dining-developer.tistory.com/28) <br>
[Spark2.0 New Features DataSet](https://www.popit.kr/spark2-0-new-features1-dataset/) <br>
[AQE: Coalescing Post Shuffle Partitions](https://tech.kakao.com/2022/01/18/aqe-coalescing-post-shuffle-partitions/)