---
title: DataHub - Dataset
author: yoongyoonge
date: 2023-10-04 22:10:00 +0900
categories: [Data Engineering, DataHub]
tags: [study, data, datahub, catalog]
render_with_liquid: false
---

> datahub의 공식 문서를 번역하고 정리한 글입니다.


## 1. Data Platform

데이터 세트, 대시보드, 차트 및 모든 메타데이터 그래프의 데이터 자산들에 대한 시스템이나 도구를 뜻함

예시로는 redshift, hive, bigquery, looker, tableau 등이 있으며 지원되는 전체 목록은 다음과 같음


| 번호 | name               | displayName             | type            |
| ---- | ------------------ | ----------------------- | --------------- |
| 1    | adlsGen1           | Azure Data Lake (Gen 1) | FILE_SYSTEM     |
| 2    | adlsGen2           | Azure Data Lake (Gen 2) | FILE_SYSTEM     |
| 3    | airflow            | Airflow                 | OTHERS          |
| 4    | ambry              | Ambry                   | OBJECT_STORE    |
| 5    | clickhouse         | ClickHouse              | RELATIONAL_DB   |
| 6    | couchbase          | Couchbase               | KEY_VALUE_STORE |
| 7    | external           | External Source         | OTHERS          |
| 8    | hdfs               | HDFS                    | FILE_SYSTEM     |
| 9    | hana               | SAP HANA                | RELATIONAL_DB   |
| 10   | hive               | Hive                    | FILE_SYSTEM     |
| 11   | iceberg            | Iceberg                 | FILE_SYSTEM     |
| 12   | s3                 | AWS S3                  | FILE_SYSTEM     |
| 13   | kafka              | Kafka                   | MESSAGE_BROKER  |
| 14   | kafka-connect      | Kafka Connect           | OTHERS          |
| 15   | mode               | Mode                    | KEY_VALUE_STORE |
| 16   | mongodb            | Mode                    | KEY_VALUE_STORE |
| 17   | mysql              | MySQL                   | RELATIONAL_DB   |
| 18   | mariadb            | MariaDB                 | RELATIONAL_DB   |
| 19   | openapi            | OpenAPI                 | OTHERS          |
| 20   | oracle             | Oracle                  | RELATIONAL_DB   |
| 21   | pinot              | Pinot                   | OLAP_DATASTORE  |
| 22   | presto             | Presto                  | QUERY_ENGINE    |
| 23   | tableau            | Tableau                 | OTHERS          |
| 24   | teradata           | Teradata                | RELATIONAL_DB   |
| 25   | voldemort          | Voldemort               | KEY_VALUE_STORE |
| 26   | snowflake          | Snowflake               | RELATIONAL_DB   |
| 27   | redshift           | Redshift                | RELATIONAL_DB   |
| 28   | mssql              | SQL Server              | RELATIONAL_DB   |
| 29   | bigquery           | BigQuery                | RELATIONAL_DB   |
| 30   | druid              | Druid                   | OLAP_DATASTORE  |
| 31   | looker             | Looker                  | OTHERS          |
| 32   | feast              | Feast                   | OTHERS          |
| 33   | sagemaker          | SageMaker               | OTHERS          |
| 34   | mlflow             | MLflow                  | OTHERS          |
| 35   | glue               | Glue                    | OTHERS          |
| 36   | redash             | Redash                  | OTHERS          |
| 37   | athena             | AWS Athena              | RELATIONAL_DB   |
| 38   | spark              | Spark                   | OTHERS          |
| 39   | dbt                | dbt                     | OTHERS          |
| 40   | elasticsearch      | Elasticsearch           | OTHERS          |
| 41   | Great Expectations | Great Expectations      | OTHERS          |
| 42   | powerbi            | Power BI                | OTHERS          |
| 43   | presto-on-hive     | Presto on Hive          | FILE_SYSTEM     |
| 44   | metabase           | Metabase                | OTHERS          |
| 45   | nifi               | NiFi                    | OTHERS          |
| 46   | superset           | Superset                | OTHERS          |
| 47   | trino              | Trino                   | QUERY_ENGINE    |
| 48   | pulsar             | Pulsar                  | MESSAGE_BROKER  |
| 49   | salesforce         | Salesforce              | OTHERS          |
| 50   | Unknown Platform   | N/A                     | OTHERS          |
| 51   | delta-lake         | Delta Lake              | OTHERS          |
| 52   | databricks         | Databricks              | OTHERS          |
| 53   | vertica            | Vertica                 | OLAP_DATASTORE  |
| 54   | gcs                | Google Cloud Storage    | FILE_SYSTEM     |
| 55   | dynamodb           | DynamoDB                | KEY_VALUE_STORE |



