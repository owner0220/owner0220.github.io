---
title: Hadoop | The Definitive Guide 07
date: 2021-03-25 10:31:00
categories:
 - PRODUCT
tag:
 - hadoop
---

# 2장 맵리듀스

## Part 7 맵리듀스 작동 방법

맵리듀스의 작동 방법을 자세히 살펴본다.

<!-- more -->

### 7.1 맵리듀스 잡 실행 상세분석

- 클라이언트
- YARN 리소스 매니저
- YARN 노드 매니저
- 맵리듀스 애플리케이션 마스터
- 분산 파일시스템



#### 7.1.1 잡 제출

#### 7.1.2 잡 초기화

#### 7.1.3 태스크 할당

#### 7.1.4 태스크 실행

##### 스트리밍

#### 7.1.5 진행 상황과 상태 갱신

#### 7.1.6 잡 완료

### 7.2 실패

#### 7.2.1 태스크 실패

#### 7.2.2 애플리케이션 마스터 실패

#### 7.2.3 노드 매니저 실패

#### 7.2.4 리소스 매니저 실패

### 7.3 셔플과 정렬

#### 7.3.1 맵 부분

#### 7.3.2 리듀스 부분

#### 7.3.3 설정 조정

### 7.4 태스크 실행

#### 7.4.1 태스크 실행 환경

##### 스트리밍 환경변수

#### 7.4.2 투기적 실행

#### 7.4.3 출력 커미터

##### 태스크 부차적인 파일