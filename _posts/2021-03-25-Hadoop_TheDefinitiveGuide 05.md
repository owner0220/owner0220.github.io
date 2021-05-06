---
title: Hadoop | The Definitive Guide 05
date: 2021-03-25 10:44:00
categories:
 - PRODUCT
tag:
 - hadoop
---

# 1장 하둡기초

## Part 5 하둡 I/O

하둡은 데이터 I/O를 위한 프리미티브(Primitive) 내장된 기본 기능을 제공한다. 멀티테라바이트의 데이터셋을 처리할 때는 특정 내장된 기능을 잘 활용할 만한 가치가 있다. ex) 직렬화 프레임워크 디스크 기반 데이터 구조 API

<!-- more -->

### 5.1 데이터 무결성

#### 5.1.1 HDFS의 데이터 무결성

#### 5.1.2 LocalFileSystem

#### 5.1.3 ChecksumFileSystem

### 5.2 압축

#### 5.2.1 코덱

##### CompressionCodec을 통한 압축 및 해제 스트림

##### CompressionCodecFactory를 사용하여 CompressionCodec 유추하기

##### 원시 라이브러리

#### 5.2.2 압축과 입력 스플릿

#### 5.2.3 맵리듀스에서 압축 사용하기

##### 맵 출력 압축

### 5.3 직렬화

- 간결성
- 고속화
- 확장성
- 상호운용성

#### 5.3.1 Writable 인터페이스

##### WritableComparable과 비교자

#### 5.3.2 Writable 클래스

##### 자바 기본 자료형을 위한 Writable 래퍼

##### 텍스트

##### BytesWritable

##### NullWritable

##### ObjectWritable과 GenericWritable

##### Writable 컬렉션

#### 5.3.3 커스텀 Writable 구현하기

##### 성능 향상을 위해 RawComparator 구현하기

##### 커스텀 비교자

#### 5.3.4 직렬화 프레임워크

##### 직렬화 IDL

### 5.4 파일 기반 데이터 구조

#### 5.4.1 SequenceFile

##### SequenceFile 만들기

##### SequenceFile 읽기

##### 명령행 인터페이스로 SequenceFile 출력하기

##### SequenceFile 정렬하고 병합하기

##### SequenceFile 포맷

#### 5.4.2 MapFile

##### MapFile의 변형

#### 5.4.3 기타 파일 포맷과 컬럼 기반 파일 포맷

