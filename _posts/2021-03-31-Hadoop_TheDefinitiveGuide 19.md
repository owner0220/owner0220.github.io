---
title: Hadoop | The Definitive Guide 19
date: 2021-03-31 19:02:00
categories:
 - PRODUCT
tag:
 - hadoop
---

# 4장 관련 프로젝트

## Part 19 스파크

아파치 스파크는 대용량 데이터 처리를 위한 클러스터 컴퓨팅 프레임워크이다. 스파크는 실행 엔진으로 맵리듀스를 사용하지 않는다. 대신 스파크는 클러스터 기반으로 작업을 실행하는 자체 분산 런타임 엔진이 있다. 스파크는 하둡과 밀접하게 통합되어 있어서 YARN 기반으로 실행할 수 있고, 하둡 파일 포맷과 HDFS 같은 기반 저장소를 지원한다.

<!-- more -->

### 19.1 스파크 설치



### 19.2 예제

#### 19.2.1 스파크 애플리케이션, 잡, 스테이지, 태스크

#### 19.2.2 스칼라 독립 애플리케이션

#### 19.2.3 자바 예제

#### 19.2.4 파이썬 예제



### 19.3 탄력적인 분산 데이터셋 RDD

#### 19.3.1 생성

#### 19.3.2 트랜스포메이션과 액션

##### 집계 트랜스포메이션

#### 19.3.3 지속성

##### 지속성 수준

#### 19.3.4 직렬화

##### 데이터 직렬화

##### 함수 직렬화



### 19.4 공유 변수

#### 19.4.1 브로드캐스트 변수

#### 19.4.2 어큐뮬레이터



### 19.5 스파크 잡 수행 분석

#### 19.5.1 잡 제출

#### 19.5.2 DAG 구성

- 셔플 맵 태스크
- 결과 태스크

#### 19.5.3 태스크 스케줄링

#### 19.5.4 태스크 수행



### 19.6 익스큐터와 클러스터 매니저

- 로컬
- 독립
- 메소스
- YARN

#### 19.6.1 YARN에서 스파크 실행

##### YARN 클라이언트 모드

##### YARN 클러스터 모드



### 19.7 참고 도서

