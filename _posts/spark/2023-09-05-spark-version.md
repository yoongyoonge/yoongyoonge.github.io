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
1) RDD를 이용한 인메모리 처리로 기존 방식 대비 빠른 속도로 처리 가능 <br>
2) v1.3: 데이터 프레임 추가 <br>
3) v1.6: 데이터셋 추가(데이터 타입체크, 인코더, 카탈리스트 옵티마이저 지원) <br>


## 2. SPARK ver. 2.x
1) RDD 기능 개선 <br>
2) dataset + dataframe <br>
- Spark 2.0 부터 DataFrame이 Dataset과 병합되어 Dataset이 되었음 <br>
3) 데이터를 스키마 형태로 추상화하여 제공 <br>
4) 쿼리 최적화 기능 추가 <br>


## 3. SPARK ver. 3.x
1) 딥러닝 지원 강화(gpu 지원 추가) <br>
2) 바이너리 파일 지원 <br>
3) 쿠버네티스 지원 강화 <br>
4) 다이나믹 파티션 프루닝 지원 <br>
5) aqe <br>



참고:

[spark 버전 별 특징](https://wikidocs.net/80412)
[유용한 팁](https://1ambda.blog/2022/01/02/practical-spark-12/)
[spark 3.0 한번에 정리하기](https://dalsacoo-log.tistory.com/entry/Spark-30-%ED%95%9C-%EB%B2%88%EC%97%90-%EC%A0%95%EB%A6%AC%ED%95%98%EA%B8%B0)
[Apache Spark 3.0 변경점/변화 총 정리](https://dining-developer.tistory.com/28)
[Spark2.0 New Features DataSet](https://www.popit.kr/spark2-0-new-features1-dataset/)