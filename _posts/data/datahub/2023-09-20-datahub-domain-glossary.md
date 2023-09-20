---
title: [DataHub] domain 기능을 통한 data mesh 강화
author: yoongyoonge
date: 2023-09-20 21:30:00 +0900
categories: [Data Engineering, DataHub]
tags: [study, data, datahub, catalog]
render_with_liquid: false
---

> [Supercharge Data Mesh with Domains in DataHub](https://www.youtube.com/watch?v=CyvujJWV-8A&t=2s) / [DataHub 201: Business Glossary](https://www.youtube.com/watch?v=pl9zx0CtdiU)를 보고 정리한 글입니다.


**Domain**
- Data Asset의 논리적인 컬렉션
- 외부 플랫폼으로 부터 오는 것이 아닌 데이터허브 자체에서 Asset의 큐레이팅된 폴더
- 중앙 집중 식으로 관리될 수도 있고 특정 도메인 소유자가 분배해서 관리할 수 있음

<br>

**Glossary**
- 조직이 중앙 집중식으로 자산을 정의하고 조직할 수 있는 것
- 비즈니스에서 사용되는 용어를 관리함으로써 산업이나 회사의 특정 용어들을 하나의 출처에 모을 수 있게 함
- 많은 사람들이 협력하면 공용 어휘를 사용하여 데이터 자산을 구성할 수 있음
- 조직 내의 다양한 자산을 정확하게 설명할 수 있음
- 비즈니스 용어를 각 비즈니스 요구에 맞추어 덜 모호하게 설명할 수 있고 이를 다른 자산에 할당하여 그룹화 할 수도있음

- 정의 방법
  - YAML 파일: GitHub와 같이 버전 관리를 통해 변경 사항을 추적하는 이점이 있으나 비기술적인 사용자는 접근하기 어려움이 있음
  - UI: 데이터 허브의 메뉴를 클릭하여 생성할 수 있고 배포 제어의 이점은 없지만 소규모 변경이나 즉각 반영할 수 있는 장점이 있고 비기술적인 사용자도 쉽게 생성할 수 있다는 이점이 있음

- 데이터를 가져올 때 transformers를 사용해서 용어를 추가할 수 있음
  - https://datahubproject.io/docs/metadata-ingestion/docs/transformer/intro/


<br>

**Tags vs. Terms vs. Domains 비교**
- Tags
  - 형식이나 규격이 없음
  - 하나의 asset이 여러 태그를 가질 수 있음
  - 중앙 관리 되지 않음
  
- Glossay Terms
  - 계층적으로(옵션) 관리되는 단어
  - 스키마 필드처럼 리프 레벨 속성을 표준화하여 비즈니스에서 관심을 가지는 표준 개념을 정의하고 수집한 컬럼과 표준 단어와 연관 짓는 부분 (예. EMAIL_PLAINTEXT)

- Domain
  - 가장 상위 레벨의 카테고리
  - 비즈니스(금융, 마케팅 등) 유닛 또는 팀과 연결
  - 중앙 관리
  - asset 당 하나의 도메인