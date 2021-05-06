---
title: 하둡 구성 시 개괄 정리 목차
date: 2021-02-28 13:35:00
categories:
 - PRODUCT
tag:
 - hadoop
 - prerequsition
---

### 하둡 설치 전 체크 내용

**Hadoop Cluster 설치**

**목적**

- 하둡을 적게는 2대 많게는 1000대가 넘는 큰 클러스터를 설치하며 참고 하도록 만든 문서 입니다. 여기서는 보안 문제나 HA는 다루지 않습니다.

**준비내용**

**설치**

**비보안 모드에서 하둡 설정**

- 하둡 데몬이 설치되는 환경 설정
- 하둡 데몬 내부 설정

**노드매니저들의 헬스 체크 모니터링**

**Slaves File**

**하둡 렉 확인사항**

**로그**

**하둡 클러스터 운영**

- 하둡 시작 명령어
- 하둡 종료 명령어

**웹 인터페이스**



<!-- more -->

### 하둡 지원 자바 버전 체크

**Apache Hadoop 3.3**

- Java 8 and Java 11 (runtime only)
- compile 시 Java 8 버전만 지원 (java 11은 현재 문제: HADOOP-16795 java 11 compile support 확인)

**Apache Hadoop 3.0.x ~ 3.2.x**

- Java 8 만 지원

**Apache Hadoop 2.7.x ~ 2.10.x** 

- Java 7, 8 지원



#### Java 8

- 1.8.0_242 지원
- 1.8.0_191 지원
- 1.8.0_171 지원