---
title: Architecting Modern Data Platforms 08
date: 2021-03-11 10:51:00
categories:
 - PRODUCT
tag:
 - hadoop
---

### 8장 플랫폼 검증

하드웨어를 랙에 설치하고 필요한 소프트웨어를 설치했다면 이제 설치가 정상적으로 이뤄졌는지 검증해야 한다. 관련 검증 방법에 대해 알아보자

<!-- more -->

#### 하드웨어, 플랫폼 검증 방법

- 스모크 테스트
- 기준 테스트
- 스트레스 테스트



#### 테스트 방법론



#### 유용한 도구들

- 설정 관리 소프트웨어
- 모니터링 소프트웨어



#### 하드웨어 검증

##### CPU

###### 검증 방법

##### 디스크

###### 순차 입출력 성능

###### 디스크의 정상상태 여부

##### 네트워크

###### 응답지연의 측정

###### 부하가 있는 경우의 응답지연

###### 처리량 측정

###### 부하 상태의 처리량

- 동일 랙 안에서의 처리량
- 랙 사이의 대역폭



#### 하둡의 검증

##### HDFS 검증

###### 단일 쓰기 및 읽기

###### 분산 쓰기 및 읽기

##### 일반적인 검증

- 테라젠(TeraGen)
- 테라소트(TeraSort)
- 테라밸리데이트(TeraValidate)

###### 테라젠

###### 테라소트



#### 다른 컴포넌트의 검증

- YCSB(Yahoo! Cloud Serving Benchmark)
- TPC-DS
- 부하 테스트
- Hbase
- 임팔라

##### 운영 검증