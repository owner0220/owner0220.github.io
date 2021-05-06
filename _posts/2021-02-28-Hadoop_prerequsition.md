---
title: 하둡 Prerequisition
date: 2021-02-28 17:35:00
categories:
 - PRODUCT
tag:
 - hadoop
---

### 하둡 설치 전 체크 및 준비 내용

- 하둡 버전에 따른 지원 자바 버전 체크
  - JAVA 8을 써라 (Hadoop 2.7.x ~ 3.3 까지 커버)
  - 1.8.0_242, 1.8.0_191, 1.8.0_171 지원

- 하둡 설치 파일 준비

<!-- more -->

### 하둡 지원 자바 버전 체크

**Apache Hadoop 3.3**

- Java 8 and Java 11 (runtime only)
- compile 시 Java 8 버전만 지원 (java 11은 현재 문제: HADOOP-16795 java 11 compile support 확인)

**Apache Hadoop 3.0.x ~ 3.2.x**

- Java 8 만 지원

**Apache Hadoop 2.7.x ~ 2.10.x** 

- Java 7, 8 지원



### 하둡 설치 파일 준비

https://hadoop.apache.org/releases.html