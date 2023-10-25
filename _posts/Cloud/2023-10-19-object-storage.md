---
title: Object Storage와 AWS S3
author: yoongyoonge
date: 2023-10-19 21:00:00 +0900
categories: [Data Engineering, Cloud]
tags: [study, cloud, storage, aws, s3]
render_with_liquid: false
---

> object storage가 무엇인지 알아보고 aws의 s3가 무엇인지도 알아봅시다.


# Object Storage란?

- 객체(Object)라고 하는 비정형 형식으로 데이터를 저장하고 관리하는 기술
    - 기존의 파일 시스템이나 블록 스토리지와는 다름
    - 계층구조 없이 평면(flat) 구조로 데이터를 저장 -> 접근이 쉽고 빠르며 확장성이 높음

- 데이터 조작에 HTTP/HTTPS를 통한 API가 사용됨

<br>

- 클라우드 객체 스토리지 시스템은 데이터를 여러 대의 물리적인 장치에 분산시키지만 사용자는 단일의 가상 스토리지 레포에서 효율적으로 데이터에 액세스 할 수 있음

<br>

- 객체 
    - 데이터와 함께 메타데이터를 포함하며 고유한 식별자를 가짐
    - 객체는 비디오, 오디오뿐 아니라 모든 데이터를 포괄하는 유형

<br>

- 대표적인 object storage 로는 Amazon S3, Azure Blob Storage, Google Cloud Storage 등이 있음

<br>

- 블록, 파일, 오브젝트 스토리지의 비교
![image](https://github.com/yoongyoonge/yoongyoonge.github.io/assets/20895661/e3d6b34f-bb3d-470b-aa27-c49b57b385f9)


<br><br><br>

# Object Storage 주요 개념

![image](https://github.com/yoongyoonge/yoongyoonge.github.io/assets/20895661/f75c53c6-7422-4174-b6c2-b694b58aab57)

### Region, 리전
- 효율적인 서비스를 위해 주요 거점에 구축한 서버의 물리적인 위치
- 리전 간 객체 공유는 불가능
- 리전을 어디에 지정하냐에 따라 지연 시간, 비용 등이 달라짐

### Bucket, 버킷
- 객체를 담는 컨테이너
- 객체를 저장하고 관리하는 역할
- 최상위 디렉토리, 모든 객체는 버킷에 포함
- 버킷 안에 다른 버킷을 둘 수 없음


### Object, 객체
- 모든 파일이 저장되는 형태
- 데이터와 메타데이터를 구성하고 있는 저장 단위
- 모든 파일은 객체의 형태(key-value 구조)로 버킷에 저장
- S3 내에서는 버킷, 키, 버전ID를 통해 객체를 파악할 수 있음

### Key, 키
- 파일의 이름
- 객체에 대한 고유 식별자
- 버킷 내 모든 객체는 하나의 키를 갖음
- 버킷의 파일 위치
- 사용자 편의 차원에서 디렉토리 계층 구조로 표시
- 실제 데이터 저장 / 조회는 평면 구조로 동작
- 계층 관계없이 키에 데이터가 매핑되는 구조

### Value, 값
- 파일의 데이터
- s3의 경우 key-value 형태이지만 key의 접두어 및 슬래시를 이용하여 폴더의 개념을 적용 할 수 있음

### Version ID, 버전 아이디
- 파일의 버전 아이디
- S3의 고유 특징
- 같은 파일이라 하더라도 다른 버전으로 올릴 수 있게 돕는 인식표
- 이전 버전으로 돌아가고싶다면 이 값을 이용하면 됨


### Metadata, 메타데이터 
- 파일의 정보를 담은 데이터
- 최종 수정일, 파일 타입, 파일 소유자, 사이즈 등...




<br>

참고자료: <br>
[AWS-객체스토리지란 무엇인가](https://aws.amazon.com/ko/what-is/object-storage/) <br>
[스토리지 종류 비교 - 블록, 파일, 오브젝트 스토리지 쉽게 이해하기](https://www.dknyou.com/blog/?q=YToxOntzOjEyOiJrZXl3b3JkX3R5cGUiO3M6MzoiYWxsIjt9&bmode=view&idx=10474168&t=board)
[Object Storage](https://portal.gov.kakaoicloud.com/docs/posts/storage/object%20storage/2022-03-02-storage_object/storage_object)
[[AWS] S3 개념 & 버킷 · 권한 설정 방법](https://inpa.tistory.com/entry/AWS-%F0%9F%93%9A-S3-%EB%B2%84%ED%82%B7-%EC%83%9D%EC%84%B1-%EC%82%AC%EC%9A%A9%EB%B2%95-%EC%8B%A4%EC%A0%84-%EA%B5%AC%EC%B6%95)