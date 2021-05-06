---
title: Hadoop | The Definitive Guide 08
date: 2021-03-25 10:36:00
categories:
 - HADOOP
tag:
 - hadoop
---

# 2장 맵리듀스

## Part 8 맵리듀스 타입과 포맷

맵과 리듀스 함수의 입력과 출력은 키-값 쌍으로 되어 있다. 특히 단순한 텍스트부터 구조화된 바이너리 객체까지 다양한 포맷의 데이터를 맵리듀스 모델에서 어떻게 처리하는지 살펴보겠다.

<!-- more -->

### 8.1 맵리듀스 타입

#### 8.1.1 기본 맵리듀스 잡

##### 기본 스트리밍 잡

##### 스트리밍의 키와 값

### 8.2 입력 포맷

#### 8.2.1 입력 스플릿과 레코드

##### FileInputFormat

##### FileInputFormat 입력 경로

##### FileInputFormat 입력 스플릿

##### 작은 파일과 CombineFileInputFormat

##### 스플릿 방지

##### 매퍼의 파일 정보

##### 파일의 전체 내용을 하나의 레코드로 처리하기

#### 8.2.2 텍스트 입력

##### TextInputFormat

##### KeyValueTextInputFormat

##### NLineInputFormat

##### XML

#### 8.2.3 바이너리 입력

##### SequenceFileInputFormat

##### SequenceFileAsTextInputFormat

##### SequenceFileAsBinaryInputFormat

##### FixedLengthInputFormat

#### 8.2.4 다중 입력

#### 8.2.5 데이터베이스 입력과 출력

### 8.3 출력 포맷

#### 8.3.1 텍스트 출력

#### 8.3.2 바이너리 출력

##### SequenceFileOutputFormat

##### SequenceFileAsBinaryOutputFormat

##### MapFileOutputFormat

#### 8.3.3 다중 출력

##### 예제: 데이터 파티셔닝

##### MultipleOutputs

#### 8.3.4 느린 출력

#### 8.3.5 데이터베이스 출력