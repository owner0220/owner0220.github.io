---
title: Hadoop | The Definitive Guide 15
date: 2021-03-30 10:52:00
categories:
 - HADOOP
tag:
 - hadoop
---

# 4장 관련 프로젝트

## Part 15 스쿱

HDFS 외부 스토리지 저장소에 있는 데이터를 접근해 옮길 수 있도록 해주는 스쿱의 작동 방식과 데이터 처리 파이프라인에서 스쿱을 활용하는 방법을 살펴보자.

<!-- more -->

### 15.1 스쿱 얻기

- tar 파일 압축 해제 시
  - $SQOOP_HOME/bin/sqoop 환경 변수 등록
- 하둡 벤더 배포판 설치 시
  - /usr/bin/sqoop 위치에 스쿱 스크립트가 저장되어 있다.

※ 스쿱1과 달리 스쿱2는 CLI, 웹 UI, REST API, 자바 API 등 다양한 클라이언트를 제공한다.

```bash
$ sqoop help
usage: sqoop COMMAND [ARGS]

Available commands:
	codegen				Generate code to interact with database records
	create-hive-table	Import a table definition into Hive
	eval				Evaluate a SQL statement and display the results
	export				Export an HDFS directory to a database table
	help				List available commands
	import				Import a table from a database to HDFS
	Import-all-tables	Import tables from a database to HDFS
	job					Work with saved jobs
	list-databases		List available databases on a server
	list-tables			List available tables in a database
	merge				Merge results of incremental imports
	metastore			Run a standalone Sqoop metastore
	version				Display version information
	
See 'sqoop help COMMAND' for information on a specific command.
```

※ 특정 인자에 대한 사용법 알기

```bash
$ sqoop help import
usage: sqoop import [GENERIC-ARGS] [TOOL-ARGS]

Common arguments:
	--connect <jdbc-uri>	Specify JDBC connect string
	--driver <class-name>	Manually specify JDBC driver class to use
	--hadoop-home <dir>		Override $HADOOP_HOME
	--help					Print usage instructions
 -P							Read password from console
 	--password <password>	Set authentication password
 	--username <username>	Set authentication username
 	--verbose				Print more information while working
...
```



### 15.2 스쿱 커넥터

스쿱이 임포트와 익스포트하게 해주는 모듈식 컴포넌트이다.

- **RDBMS**
  - MySQL
  - PostgreSQL
  - 오라클
  - SQL 서버
  - DB2
  - 네티자(Netezza) 등

- **JDBC**
  - JDBC 프로토콜을 지원하는 어떤 데이터베이스에도 연결 가능한 제네릭 JDBC 커넥터 제공
- **엔터프라이즈 데이터 웨어하우스**
  - NoSQL
    - Couchbase



### 15.3 임포트 예제

스쿱 연결 대상 : RDBMS (MySQL)

스쿱 연결 대상 DB명 : hadoopguide

스쿱 연결 대상 table명 : widgets

**사전 준비사항**

1. MySQL JDBC 드라이버 JAR 파일 (Connector/J) 다운로드

2. 스쿱 lib 디렉터리에 저장

※ 스쿱으로 RDBMS(MySQL)의 **hadoopguide** 디비 속 **widget** 테이블 맵 태스크 1 적용

```bash
$ sqoop import --connect jdbc:mysql://192.168.100.111/hadoopguide --table widget -m 1
```

※ 적재 확인

```bash
$ hadoop fs -cat /widgets/part-m-00000
```

#### 15.3.1 텍스트와 바이너리 파일 포맷

스쿱은 텍스트 파일 외에 다른 파일 포맷으로 임포트한 데이터를 저장할 수 있다. 더불어 스쿱은 시퀀스 파일, 에이브로 데이터 파일, 파케이 파일을 지원한다.



### 15.4 생성된 코드

데이터베이스 테이블의 내용을 HDFS에 저장하는 것 외에도 스쿱은 로컬 디렉터리에 자바 소스 파일(widget.java)을 생성한다. 스쿱은 HDFS에 쓰기 전에 데이터베이스 원본의 특정 테이블 데이터를 역직렬화하기 위해 미리 생성된 자바 코드를 사용한다.

※ codegen 도구는 전체 임포트를 하지 않고 단순히 코드만 생성한다.

```bash
$ sqoop codegen --connect jdbc:mysql://localhost/hadoopguide --table widgets --class-name Widget
```

#### 15.4.1 추가적인 직렬화 시스템

스쿱은 에이브로 기반의 직렬화와 스키마 생성을 지원하므로 생성된 코드를 통합하는 과정을 굳이 거치지 않아도 프로젝트에서 스쿱을 사용할 수 있다.



### 15.5 임포트 자세히 살펴보기

스쿱은 데이터베이스에 접속할 때 사용된 접속 문자열의 URL 기반으로 어떤 드라이버를 로드할지 예측합니다. 스쿱이 어떤 JDBC 드라이버를 사용할지 모를 때는 --driver 인자에 명시적으로 지정해주어야 합니다.

※ 스쿱이 구동시킨 맵리듀스 잡은 JDBC를 통해 데이터베이스의 테이블을 읽을 때 InputFormat을 이용합니다. 하둡에 포함된 DataDrivenInputFormat은 쿼리 결과를 여러 개의 맵 태스크로 나눈다.

쿼리를 다수의 노드에 분산 시키면 임포트 성능을 높일 수 있다. 이때 분할 기준 컬럼(splitting column)이 중요한데 스쿱은 테이블 메타데이터 정보를 기반으로 분할에 적합한 컬럼을 예상한다. 테이블에 기본 키 (Primary key)가 있으면 컬럼 분할 기준으로 사용한다.

※ id 가 0부터 99,999  (100,000 개)있다고 가정할 때

```sql
SELECT min(id), max(id) FROM widgets;
```

위의 쿼리는 -m 5 지정 시

```sql
SELECT min(id), max(id) FROM widgets WHERE id >= 0 AND id < 2000;
SELECT min(id), max(id) FROM widgets WHERE id >= 2000 AND id < 4000;
SELECT min(id), max(id) FROM widgets WHERE id >= 4000 AND id < 6000;
SELECT min(id), max(id) FROM widgets WHERE id >= 6000 AND id < 8000;
SELECT min(id), max(id) FROM widgets WHERE id >= 8000 AND id < 10000;
```

으로 분할 되어 쿼리를 수행할 것이다. (핵심은 병렬 작업을 효율적으로 수행하도록 분할 컬럼을 선택하는 것=> 작업이 한쪽으로 치우치지 않도록 하는 것에 핵심)



#### 15.5.1 임포트 제어하기

전체 테이블을 한번에 임포트할 수 있지만 테이블 일부 컬럼만 지정하여 임포트 할 수 있고, -where 인자에 WHERE 절을 지정하면 원하는 행의 범위를 제한할 수 있다.

```bash
$ sqoop import --connect jdbc:mysql://192.168.100.111/hadoopguide --table widgets --where "id >= 100000"
```

※ 사용자가 지정한 WHERE 절은 태스크 분할이 수행되기 전에 적용되며, 각 태스크에서 실행이 되는 쿼리에 모두 반영된다.

#### 15.5.2 임포트와 일관성

HDFS에 데이터를 임포트할 때 원본 데이터의 일관된 스냅숏에 대한 접근을 보장하는 것은 매우 중요하다. 임포트를 수행하는 동안 테이블의 기존 행을 수정하는 프로세스 실행을 막는 것이 제일 좋다. (적재 시 원본 데이터 트랜젝션 stop 중요)

#### 15.5.3 증분 임포트

데이터베이스에  저장된 데이터의 동기화를 유지하기 위해 주기적으로 임포트 작업을 수행한다. 스쿱은 --check-column으로 지정된 컬럼의 값이 특정 값 (--last-value로 지정한)보다 큰 행을 임포트하는 기능을 제공한다.  (ex 행의 순서를 기록하는 id 값의 경우 마지막 기록이 10000, 그 값보다 커졌을 경우 증분 임포트)  주기적으로 할 경우

```bash
# 스쿱의 잡 저장기능을 통해 수행하도록 하면 된다. 설명을 읽고 필요한 기능으로 수행하자.
$ sqoop job -help
```

#### 15.5.4 직접 모드 임포트

RDBMS에서 mysqldump 와 같은 별도 기능을 제공하는 경우 JDBC 보다 더 빠르게 테이블을 읽을 수 있다. 이러한 외부 도구의 사용법은 스쿱의 직접 모드(direct mode)를 참고하길 바란다.

- MySQL, PostgreSQL, 오라클, 네티자에서 해당 기능 지원

※ 데이터베이스의 내용에 접근하는 데 직접 모드를 사용하더라도 메타데이터 정보는 JDBC를 통해 조회된다.



### 15.6 불러온 데이터로 작업하기

HDFS에 데이터가 저장되면 일반 맵리듀스 프로그램으로 처리할 수 있다.

#### 15.6.1 임포트한 데이터와 하이브

데이터를 분석할 때 관계형 연산을 지원하는 하이브와 같은 시스템을 활용하면 분석 파이프라인의 개발이 매우 쉬워진다.

**SQOOP으로 적재 완료 한 상태**

- hive 테이블 만들고, hive로 데이터 불러오기

```sql
hive> CREATE TABLE sales(widget_id INT, qty INT
    > street STRING, city STRING, state STRING,
    > zip INT, sale_date STRING)
    > ROW FORMAT DELIMITED FIELDS TERMINATED BY ',';
    
hive> LOAD DATA LOCAL INPATH "ch15-sqoop/sales.log" INTO TABLE sales;
```

- sqoop으로 테이블 만들고 hive로 데이터 불러오기

```bash
$ sqoop create-hive-table --connect jdbc:mysql://192.168.100.111/hadoopguide --table widgets --fields-terminated-by ','

$ hive
hive> LOAD DATA INPATH "widgets" INTO TABLE widgets;
```



※ HDFS에 데이터 임포트, 하이브 테이블 생성, HDFS에 저장된 데이터 하이브로 로드

-> RDBMS에서 하이브로 임포트까지 Sqoop 하나로 한방에 작업 가능

```bash
$ sqoop import --connect jdbc:mysql://192.168.100.111/hadoopguide --table widgets -m 1 --hive-import
```

--hive-import 인자를 통해서 한방에 작업 가능



### 15.7 대용량 객체 임포트하기



### 15.8 익스포트 수행하기

스쿱에서 임포트(import)는 데이터베이스 시스템에서 HDFS로의 데이터 이동을 의미한다. 반면 익스포트(export)는 데이터 소스로 HDFS를 목적지인 원격 데이터베이스로 이동을 의미한다. 해당 익스포트 작업을 수행하기 위해서는 먼저 목적지인 원격 데이터베이스에 미리 테이블 선언을 해둬야 한다.

```bash
# RDBMS에 테이블이 만들어져 있음을 확인하고 아래 명령어를 수행한다.
$ sqoop export --connect jdbc:mysql://localhost/hadoopguide -m 1 --table sales_by_zip --export-dir /user/hive/warehouse/zip_profits --input-fields-terminated-by '\0001'
```

하이브에서 zip_profits 테이블 생성 시 어떤 구분자도 지정하지 않았다. 그래서 하이브는 필드 기본 구분자인 Ctrl-A 문자 (유니코드 0x0001)를 사용했고 레코드의 끝에는 개행문자를 사용했다.



### 15.9 익스포트 자세히 살펴보기

#### 15.9.1 익스포트와 트랜잭션

#### 15.9.2 익스포트와 시퀀스 파일

### 15.10 참고 도서

