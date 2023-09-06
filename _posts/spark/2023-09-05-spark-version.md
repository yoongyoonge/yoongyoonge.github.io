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

(1) RDD를 이용한 인메모리 처리로 기존 방식 대비 빠른 속도로 처리 가능 <br>
(2) v1.3: 데이터 프레임 추가 <br>
(3) v1.6: 데이터셋 추가(데이터 타입체크, 인코더, 카탈리스트 옵티마이저 지원) <br>

## 2. SPARK ver. 2.x

(1) RDD 기능 개선 <br>

(2) dataset + dataframe

- Spark 2.0 부터 DataFrame이 Dataset과 병합되어 Dataset이 되었음

(3) 데이터를 스키마 형태로 추상화하여 제공 <br>
(4) 쿼리 최적화 기능 추가 <br>

## 3. SPARK ver. 3.x

(1) 딥러닝 지원 강화(gpu 지원 추가) <br>
(2) 바이너리 파일 지원 <br>
(3) 쿠버네티스 지원 강화 <br>
(4) 다이나믹 파티션 프루닝 지원 <br>
(5) aqe <br>

(6) Spark SQL

- 일부 예약된 키워드를 식별자로 사용할 수 없음

    ``` SQL
    select * from some_table create where create.some_column = 1
    ```

  - 기존에는 create라는 키워드를 문제없이 사용가능 했음
  - ANSI SQL 표준을 준수할 수 있게 되면서 spark.sql.ansi.enabled 속성을 true로 설정하면 일부 키워드에 대한 사용을 막을 수 있음

<br>

- 엄격한 데이터 품질 관리

    ``` SQL
    CREATE TABLE assignment_policy_check(col INT);
    INSERT INTO assignment_policy_check VALUES("123")
    SELECT * FROM assignment_policy_check
    
    >>> 1, 123
    ```

  - 기존에는 정수형 컬럼에 문자열을 넣는 작업이 수행가능했음
  - spark.sql.storeAssign,entPolicy를 ANSI로 설정하면 데이터 타입에 맞지 않는 값을 입력되지 않게 할 수 있음
  
<br>

- string을 날짜형(Date/Timestamp)으로 변환

    ```SQL
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

(7) json 옵션

- FAILFAST/PERMISSIVE
  - 해당 모드가 추가되면서 json이 잘못되거나 파싱 할 수 없을 때 에러를 발생 시킬 수 있음

    ```python  
    df.withColumn("colname", from_json(df['colname'], jsonSchema, {'mode':FAILFAST'})).show()
    >> 파싱할 수 없을경우 에러 발생
    ```

<br>

(8) 지수표기법

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

(9) -0.0 과 0.0

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

#### Spark 3.0 요약
- Spark 2.4 보다 약 2배 속도 향상
    - 적응 쿼리 지원
    - 동적 파티션 pruning
    - 이외의 최적화 도입

- ANSI SQL 준수
- Pandas API 개선
- Python Error 핸들링 개선
- structured streaming UI
- R UDF 호출 시 최대 40배 속도 향상
- Accelerator 키 인식 스케줄러
- SQL 참조 문서


s
<br>

참고:

[spark-release-3.0.0](https://spark.apache.org/releases/spark-release-3-0-0.html)
[spark 버전 별 특징](https://wikidocs.net/80412) <br>
[spark 3.0 한번에 정리하기](https://dalsacoo-log.tistory.com/entry/Spark-30-%ED%95%9C-%EB%B2%88%EC%97%90-%EC%A0%95%EB%A6%AC%ED%95%98%EA%B8%B0) <br>
[Apache Spark 3.0 변경점/변화 총 정리](https://dining-developer.tistory.com/28) <br>
[Spark2.0 New Features DataSet](https://www.popit.kr/spark2-0-new-features1-dataset/) <br>