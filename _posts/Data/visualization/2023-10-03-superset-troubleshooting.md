---
title: Superset 3.0.0 설치 시 troubleshooting
author: yoongyoonge
date: 2023-10-03 22:10:00 +0900
categories: [Data Engineering, Apache Superset]
tags: [study, data, visualization, superset, troubleshooting]
render_with_liquid: false
---

> superset 3.0.0에 대한 트러블슈팅 기록을 남긴 글입니다.

## 1. superset 3.0.0 최초 구동시 Refusing to start due to insecure SECRET_KEY

**상태**
- docker container log

```
--------------------------------------------------------------------------------
                                    WARNING
--------------------------------------------------------------------------------
A Default SECRET_KEY was detected, please use superset_config.py to override it.
Use a strong complex alphanumeric string and use a tool to help you generate 
a sufficiently random sequence, ex: openssl rand -base64 42
--------------------------------------------------------------------------------
--------------------------------------------------------------------------------
Refusing to start due to insecure SECRET_KEY
```

<br>

**원인**
- superset에서는 보안 구성을 강제하기 위해 2.1.0 버전부터 사용자가 지정한 SECRET_KEY가 필요

<br>

**해결방안**
- superset_config.py 또는 docker compose 파일에 다음의 값을 추가
 SUPERSET_SECRET_KEY={openssl rand -base64 42 값 (무작위 랜덤 key)}

<br>

**참고자료**

[configuring-superset](https://superset.apache.org/docs/installation/configuring-superset/)

<br>

## 2. 로그인 시도 이후에도 로그인 화면에 계속 머무르는 이슈

**상태**
- superset 메인페이지에서 로그인 시도 해도 메인 로그인 페이지만 로드됨

<br>

**원인**
- superset은 talisman extension을 사용하여 교차 사이트 스크립팅 및 데이터 주입 공격 등 특정 유형의 공격을 탐지하고 완화하는 콘텐츠 보안 정책(CSP, Content Security Policy)을 구현
- CSP는 브라우저가 실행 가능한 스크립트의 유효한 소스로 간주해야하는 도메인을 지정하여 공격이 발생할 수 있는 벡터를 줄이거나 제거할 수 있도록 함
- CSP 호환 브라우저는 허용된 도메인에서 수신된 소스 파일에 로드된 스크립트만 실행하고 다른 모든 스크립트(인라인 스트립트 및 이벤트 처리 HTML 특성 포함)은 무시
- CSP가 올바르게 구성되어있지 않아 LDAP을 활용한 로그인 불가

<br>

**해결방안**
- CSP를 구성하는데 공수가 들기 때문에 임시로 비활성화
- 환경에 따라 다음의 변수 값을 재정의(테스트 환경의 경우 docker compose 파일 내 변수에 정의했음)

```Dockerfile
# 환경변수에 다음을 재정의
WTF_CSRF_ENABLED: "False"
TALISMAN_ENABLED: "False"
```

<br>

**참고자료**

[Superset security](https://superset.apache.org/docs/security/)


<br>

## 3. Dataset - table schema 이슈

**상태**

- SQL 랩은 잘 동작하나 Dataset에 테이블을 추가할 때 hive와 impala를 데이터베이스 원천으로 잡고 스키마나 테이블 버튼을 클릭하면 테이블을 불러 올수 없음

<br>

### 3.1 Hive
- superset이 pyhive로 실행시키는 구문이 CDH가 추가 한 hive와 맞지않은 구문을 사용하여 에러를 뱉음
- superset이 수행하려는 show views 는 hive 2.2.0 부터 지원하는데 설치되어있는 hive는 2.1.1라서 기능을 쓸수 없음
    - hive 버전업 또는 superset 2.0.x 버전 사용해야 함
    - [Hive DDL](https://cwiki.apache.org/confluence/display/hive/languagemanual+ddl)

<br>

### 3.2 impala
- SQL Lab은 잘 동작하는데 table schema를 볼수가 없음
    - impyla의 sqlalchemy 드라이버는 get_view_names를 구현하지 않기 때문에 사용할 수 없음
    - 경로: /usr/local/lib/python3.9/site-packages/impala/sqlalchemy.py 참고
    - 임시로 다음의 코드를 sqlalchemy에 추가하면 동작은 가능하도록 만들 수 있음

    ```python 
    # imsi
    def get_view_names(self, connection, schema=None, **kw):
        # superset 기능 작동을 위해 임시 추가
        vl = []
        return [v[0] for v in vl]
    Inline-image-2023-10-10 16.45.11.220.png
    Inline-image-2023-10-10 16.45.31.093.png
    ```

- 위 코드를 단순 적용한다면 table목록이 view로 출력되어 목록에 같은 테이블이 두 번씩 반복되어 나타나게 됨
- impala 연결이 필요하다면 dockerfile 빌드 시 해당 코드 부분에 대한 수정된 코드파일을 패치하여 위 코드 반영될 수 있게 해야함

<br>

참고 자료:

[Superset Issue 23850](https://github.com/apache/superset/issues/23850) <br>
[Superset Issue 24040](https://github.com/apache/superset/issues/24040)