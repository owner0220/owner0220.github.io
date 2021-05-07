---
title: 하둡 Installation
date: 2021-03-02 11:00:00
categories:
 - PRODUCT
tag:
 - hadoop
---

### 하둡 설치

자바 설치

- Java 8 설치

  ```bash
  $ yum install openjdk-1.8.0*
  ```

하둡 설치 파일 다운로드

- https://hadoop.apache.org/releases.html



### 설치 전 역할 구성

- 하둡 설치 전 물리적인 하드웨어를 기능 단위로 나눠 놓는 것을 추천한다.



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



### 설치 전 역할 구성

- **Master** 

  - NameNode
  - ResourceManager

- **Web App Proxy Server**

- **MapReduce Job History Server**

  ※ 웹앱프록시 서버와 맵리듀스 잡 히스토리 서버는 서버 리소스를 보면서 나눠 분배한다.

- **rest of the machines**

  - DataNode
  - NodeManager

