---
title: Airflow Custom Component
author: yoongyoonge
date: 2023-10-11 20:00:00 +0900
categories: [Data Engineering, Apache Airflow]
tags: [study, airflow, data, pipeline, custom, operator]
render_with_liquid: false
---

> 이 문서는 도서 'Apache Airflow 기반의 데이터 파이프라인'의 chapter8 커스텀 컴포넌트 빌드를 공부하고 작성한 글입니다.

<br>

# 1. Custom Hook

### 필요한 경우
- API 연동 등의 복잡한 작업의 처리가 필요할 때

### 효과
- 코드를 캡슐화하고 재활용 가능
- 데이터베이스와 UI를 통해 자격 증명과 연결된 관리를 사용할 수 있음

### 예시
```python
# 훅생성
hook = MovielensHook(conn_id="movielens")
# 생성된 훅을 사용하여 특정 작업 수행
ratings = hook.get_ratings(start_date, end_date)
# 훅을 닫고, 사용된 리소스 해제
hook.close()
``` 

### 설계
- Airflow의 모든 훅은 추상 클래스인 BaseHook 클래스의 서브클래스로 생성

- Airflow 버전 1에서는 BaseHook 클래스의 생성자에 source라는 argument를 반드시 전달해야함, 사용하지 않을 경우 source=None으로 전달

- 연결정보(자격증명정보)의 경우 하드코딩보다는 Airflow 자격 인증 저장소에서 가져오는게 좋음
    - Admin > Connection > Create to add a new connection
    - 연결 세부 정보를 가져와야 할 시 BaseHook의 get_connection 메서드 활용
    - 단, 연결 관련 함수를 호출 할 때마다 Airflow 메타스토어에 작업을 요청해야하므로 이 방법이 **단점**으로 작용할 수도 있는데 이를 해결하기 위해서는 인스턴스에 세션과 base_url을 protected 변수에 캐싱할 수 있음

- 커스텀 패키지를 가지는 DAG 디렉터리 구조의 경우 dags 하위에 custom 패키지를 만들어 아래에 훅을 위치시키는 방안이 있음 
    ```python 
    # 사용 예시
    from custom.hooks import MovielensHook
    ```

<br>

# 2. Custom Operator

### 필요한 경우**
- 수 많은 단순 반복적인 코드가 필요한 경우
- 여러 DAG에서 재사용 해야 할 경우

### 효과
- 반복적인 태스크 수행에 대해 코드의 반복을 최소화 할 수 있음

### 예시
```python
from airflow.models import BaseOperator
from airflow.utils.decorators import apply_defaults

# BaseOperator 클래스 상속
class MyCustomOperator(BaseOperator):
    # 기본 DAG 인수를 커스텀 오퍼레이터에게 전달하기 위한 데코레이터
    @apply_defaults 
    # BaseOperator 생성자에게 추가 키워드 인수를 전달
    
    def __init__(self, conn_id, **kwargs):
        super.__init__(self, **kwargs)
        self._conn_id = conn_id
        ...
```

### 설계
- 인수는 오퍼레이터마다 다르지만, 일반적으로 커넥션ID(원격 시스템을 포함하는 오퍼레이터의 경우)와 작업에 필요한 세부사항(예: 시작/종료 날짜, 쿼리 등)이 포함됨

- BaseOperator 클래스는 generic 인수들을 많이 가지고 있는데, task_id, retries, retry_delay 등과 같고 이를 모두 나열하지 않도록 **kwargs를 사용
    - Airflow에서 사용되는 특정 인수를 전체 DAG의 기본인수로 정의할 수 있고 이는 DAG의 default_args를 사용하면 됨

- 커스텀 오퍼레이터를 정의할 때 의도치 않게 동작이 중단되는 것을 방지하기 위해 apply_defaults를 항상 포함해야 함

- Airflow에서는 템플릿 가능한 오퍼레이터 변수를 만들 수 있으며, 실행 날짜와 같은 context 변수들을 참조할 수 있음
    - 특정 인스턴스 변수를 템플릿으로 만들려면, templates_field 클래스 변수에 해당 변수명을 지정하여 Airflow에 알려줘야 함
    - 예시
    ``` python
    class MovielensFetchRatingsOperator(BaseOperator):
        ...

        template_fields = ("_start_date", "_end_date", "_output_path")

        @apply_defaults
        def __init__(
            self,
            conn_id,
            output_path,
            start_date="{{ds}}",
            end_date="{{next_ds}}",
            batch_size=1000,
            **kwargs,
        ):
            super(MovielensFetchRatingsOperator, self).__init__(**kwargs)

            self._conn_id = conn_id
            self._output_path = output_path
            self._start_date = start_date
            self._end_date = end_date
            self._batch_size = batch_size
    ```
    - 만약, 문자열 파라미터에 Jinja 템플릿을 사용하면, Airflow는 execute 메서드를 호출하기 전에 이 값들을 템플릿화
    - 예시
    ```python
    from custom.operators import MovielensFetchRatingsOperator

    fetch_ratings = MovielensFetchRatingsOperator(
        task_id="fetch_ratings",
        conn_id="movielens",
        start_date="{{ds}}",
        end_date="{{next_ds}}",
        output_path="/data/custom_operator/{{ds}}.json"
    )
    ```

# 3. Custom Sensor

### 예시
```python
from airflow.sensors.base import BaseSensorOperator

class MyCustomSensor(BaseSensorOperator):

    def poke(self, context):
        ...

    ...
```

### 설계
- BaseSensorOperator 클래스를 상속해야하고 오퍼레이터의 execute 메서드 대신 poke 메서드를 구현해야 함
- Airflow가 context를 포함하는 단일 인수만을 사용하는 측면에서 센서의 poke 메서드와 오퍼레이터의 execute는 매우 유사
    - 다른점은 poke는 센서 상태를 True/False로 나타내는 boolean 값을 반환
    - 센서 상태가 False 면 해당 프로세스는 상태 값이 True가 되거나 타임 아웃 될 때까지 대기 상태로 들어감

- 센서는 오퍼레이터의 특정 유형이기 때문에, 오퍼레이터를 구현할 때 사용했던 것과 같이 template_fields, @apply_defaults 등의 같은 설정을 사용



# 4. 요약
1. 사용자에 맞추어 Custom Component를 구현하여 Airflow의 기본 내장 기능을 확장할 수 있고 효과적으로 적용될 수 있는 두 가지 사례는 다음과 같다.
    - Airflow에서 기본적으로 제공하지 않는 시스템에서 태스크를 실행 (ex. 새로운 클라우드 서비스나 데이터베이스 등)
    - 공통적으로 수행되는 작업에 대한 오퍼레이터/센서/훅을 제공함으로써 한팀의 여러 개발자들이 여러 DAG에 구현하기 쉽게 함
2. Custom Hook을 사용하여 Airflow가 지원하지 않는 시스템과 연동할 수 있다.
3. 개별 워크플로에 특화되거나 Airflow 기본 내장 오퍼레이터로 처리할 수 없는 태스크를 수행하기 위해 커스텀 오퍼레이터를 만들어 사용할 수 있다.

<br><br>

### 예제 실습 환경 조성(WSL에서 docker세팅하기) <br>
참고자료: <br>
[Ubuntu Docker 설치하기 및 WSL2 Docker 설치](https://dodo1054.tistory.com/227) <br>
-> 이 블로그 글 하나로 오류 한번에 해결가능! <br>
[WSL에서 ip관련 설정 해주기](https://github.com/microsoft/WSL/issues/6655)