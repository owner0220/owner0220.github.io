---
title: Hadoop | The Definitive Guide 06
date: 2021-03-24 15:35:00
categories:
 - HADOOP
tag:
 - hadoop
---

# 2장 맵리듀스

## Part 6 맵리듀스 프로그래밍

맵리듀스 애플리케이션을 개발하는 방법을 살펴본다.

1. 단위 테스트를 작성
2. 잡을 실행하는 드라이버 프로그램 작성
3. 정상 동작 검증을 위해 IDE에서 작은 데이터로 실행
4. 실패 시 IDE 디버거를 통해 문제 원인 파악

잘 됐다면

1. 위 프로그램이 더 빠르게 수행 되도록 표준검사 실행
2. 태스크 프로파일링(성능 분석)을 수행
3. 분산 프로그램 프로파일링 지원으로 하둡 훅(hook)을 활용

<!-- more -->

### 6.1 환경 설정 API

하둡 컴포넌트는 하둡 자체의 환경 설정 API를 이용해 설정할 수 있다. Configuration은 **리소스**라 불리는 이름-값 쌍의 단순한 구조로 정의된 **XML 파일**로부터 속성 정보를 읽는다.

**파일**  `configuration-1.xml` 

```xml
<?xml version="1.0"?>
<configuration>
	<property>
	    <name>color</name>
        <value>yellow</value>
        <description>Color</description>
    </property>

    <property>
	    <name>size</name>
        <value>10</value>
        <description>Size</description>
    </property>

   	<property>
	    <name>weight</name>
        <value>heavy</value>
        <final>true</final>
        <description>Weight</description>
    </property>
    
	<property>
	    <name>size-weight</name>
        <value>${size},${weight}</value>
        <description>Size and weight</description>
    </property>    
    
</configuration>
```



**Java 코드**

```java
Configuration conf = new Configuration();
conf.addResource("configuraion-1.xml");

assertThat(conf.get("color"),is("yellow"));
assertThat(conf.getInt("size",0),is(10));
assertThat(conf.get("breadth","wide"), is("wide"));
```



#### 6.1.1 리소스 결합하기



#### 6.1.2 변수 확장

### 6.2 개발환경 설정하기

#### 6.2.1 환경 설정 파일 관리하기

#### 6.2.2 GenericOptionsParser, Tool, ToolRunner

### 6.3 엠알유닛으로 단위 테스트 작성하기

#### 6.3.1 매퍼

#### 6.3.2 리듀서

### 6.4 로컬에서 실행하기

#### 6.4.1 로컬 잡 실행하기

#### 6.4.2 드라이버 테스트하기

### 6.5 클러스터에서 실행하기

#### 6.5.1 잡 패키징

#### 6.5.2 잡 구동하기

#### 6.5.3 맵리듀스 웹 UI

#### 6.5.4 결과 얻기

#### 6.5.5 잡 디버깅

#### 6.5.6 하둡 로그

#### 6.5.7 원격 디버깅

### 6.6 잡 튜닝하기

#### 6.6.1 태스크 프로파일링

### 6.7 맵리듀스 작업 흐름

#### 6.7.1 맵리듀스 잡으로 문제 분해하기

#### 6.7.2 JobControl

#### 6.7.3 아파치 오지

