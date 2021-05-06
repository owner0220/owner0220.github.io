---
title: Architecting Modern Data Platforms 09
date: 2021-03-08 10:00:00
categories:
 - HADOOP
tag:
 - hadoop
---

### 9장 보안

보안에 대한 관점은 크게 인증(Authentication), 권한(Authorization), 감사(Auditing), 기밀성(Confidentiality)의 4가지 도메인으로 분류할 수 있다. 그중에서도 네트워크를 통한 인증과 권한 메커니즘의 수행 과정을 보호하는 차원에서 중요한 것이 기밀성 제어이므로, 우선 전송 중 암호화(In-fight encryption)부터 살펴본다. 그 후 인증과 권한에 대해 알아보고 마지막으로 유휴 시 암호화(at-rest encryption)적용 옵션들에 대해 설명한다.

<!-- more -->

#### 전송 중 암호화

##### TLS 암호화

- TLS와 자바
- TLS와 비 자바 프로세스
- X.509
  - Subject name 필드
  - Issuer name 필드
  - Validity 필드
  - Subject alternative name 필드

##### SASL 보호 품질

##### 전송 중 암호화 활성화

#### 인증

##### 커버로스

- 프린시펄
- 서비스에 접근하기
- 키탭
- HTTP 기반 커버로스
- 렐름 간 신뢰

##### LDAP 인증

##### 위임 토큰

##### 위장 메커니즘

#### 권한 부여

- 서비스 수준
- 객체 수준

##### 그룹 해석

- 운영체제 조회
- LDAP 조회
- 다중 소스
- 정적 정의

##### 슈퍼유저와 슈퍼그룹

- 슈퍼유저 권한
  - 네트워크 방화벽
  - 전용 커버로스 렐름
  - 프린시펄 매핑 규칙
  - 사용자 정의 프린시펄 이름
- 슈퍼그룹

##### 하둡 서비스 수중 권한 관리

##### 중앙식 보안 관리

##### HDFS

##### 얀

- min.user.id
- allowed.system.users
- banned.users

##### 주키퍼

##### 하이브

##### 임팔라

##### HBase

##### 솔라

##### 쿠두

##### 우지

##### 휴

- business_analysts
- data_engineers

##### 카프카

##### 센트리

#### 유휴 시 암호화

##### 클라우데라 내비게이터 인크립트와 키 트러스티 서버를 이용한 볼륨 암호화

##### HDFS 투명 데이터 암호화(TDE)

- 암호화 존 안의 파일을 암호화 및 복호화하기
- 키 작업에 대한 권한 관리
- KMS 구현체
  - 클라우데라 내비게이터 키 트러스티 서버(Cloudera Navigator Key Trustee Server)
  - 호튼웍스 레인저 KMS(Hortonworks Ranger KMS)

##### 임시 파일의 암호화

