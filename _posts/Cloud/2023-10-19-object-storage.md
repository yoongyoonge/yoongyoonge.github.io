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


<br>

참고자료: <br>
[AWS-객체스토리지란 무엇인가](https://aws.amazon.com/ko/what-is/object-storage/) <br>
[스토리지 종류 비교 - 블록, 파일, 오브젝트 스토리지 쉽게 이해하기](https://www.dknyou.com/blog/?q=YToxOntzOjEyOiJrZXl3b3JkX3R5cGUiO3M6MzoiYWxsIjt9&bmode=view&idx=10474168&t=board)