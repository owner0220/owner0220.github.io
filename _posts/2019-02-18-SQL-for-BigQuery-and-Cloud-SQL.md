---
title: SQL for BigQuery and Cloud SQL
date: 2019-02-18 00:00:00
categories:
 - Google
tags:
 - Google_Studygem
 - BigQuery
 - SQL
---

> 연관된 교육
>
> - Baseline: Data, ML, AI  https://google.qwiklabs.com/quests/34
> - Data Science on the Google Cloud Platform https://google.qwiklabs.com/quests/43
> - Scientific Data Processing  https://google.qwiklabs.com/quests/28
> - Cloud SQL  https://google.qwiklabs.com/quests/52
> - BigQuery For Data Analysis https://google.qwiklabs.com/quests/55
> - 기타 구글 교육 리스트(353개 이상) https://google.qwiklabs.com/catalog



# 수집

# SQL for BigQuery and Cloud SQL

> GCP qwiklabs 교육자료

### 도커를 이용한 SQL 을 사용할 때 방법

#### 1. 사용할 데이터를 모두 수집한다.

- 이때 데이터는 정제되지 않은 상태이다. (다양한 칼럼에 모두 때려 박은 상태)
- 이때 주로 CSV파일이나 sql을 사용한다.(호환성이 좋고 주로 많이 사용하기 때문)

#### 2. 이 파일을 프로젝트에서 사용할 공간에 업로드(공유) 한다.

- 이때 파일은 목적에 맞게 뽑은 데이터이며 이 자료를 업로드 한다.

- 이 공간은 예를 들면 구글 클라우드 스토리지 같은 저장 장소이다.(상황에 따라 외부서비스나 내부용량장치를 이용한다.)

#### 3. SQL서버에 업로드 한다. (DB 트래픽 분석을 위해서)

- 분류된 데이터를 불러오기 쉽도록 SQL에 DB를 만들고 TABLE을 만들어 임포트 시킨다.

#### 4. SQL을 보기 쉽게 통계로 확인 할 수 있는 그림(그래프)로 지원

- 구글 클라우드 데이터랩 ; 쥬피터 노트북 환경처럼 웹으로 개발 및 확인 가능





# BigQuery: Qwik Start - Console

### 도커를 이용한 SQL 사용 방법 숙달

#### 1. 사용할 데이터를 SQL로 확인한다.

#### 2. SQL에 dataset을 저장할 database를 만든다.

#### 3. 데이터 베이스에 저장할 자료를 가져온다.

- 이때 파일은 구글 클라우드 스토리지 같은 저장소에 올려서 가져오거나 다른 방법을 통해서 일단 개발환경안에 집어 넣는다.

#### 4. 가져온 데이터를 넣을 Table 을 SQL에 만들고 그안에 3번 자료를 넣는다(임포트).

- 스키마 선언 및 데이터 임포트

#### 5. 데이터가 잘 들어갔는지 SQL로 확인한다.



# BigQuery: Qwik Start - Command Line

### 도커를 이용한 SQL 사용 방법 숙달_CLI환경

- 기본적으로 CLI에서 SQL을 사용하려면 관련된 툴을 설치해야 사용이 가능한데 구글에서는 이러한 툴을 자체 개발하여 구글 서비스인 빅쿼리를 사용할 수 있도록 한다.

- 구글클라우드플랫폼에서는 CLI에서도 SQL을 사용할 수 있도록 **bq** 명령어를 지원한는데

  - **bq** is a python-based, command-line tool for BigQuery

    **bq**는 파이썬기반의 구글빅쿼리를 사용할 수 있는 CLI 툴이다.

#### 1. 현재 가지고 있는 Dataset(데이터베이스)을 확인한다.

- 확인 명령어 (지금 로그인한 아이디의 프로젝트 아이디가 가진 데이터베이스를 확인한다.)

  ```bash
  bq ls
  ```

- 별도로 프로젝트 아이디를 입력해야 그 프로젝트가 가지고 있는 데이터셋을 확인 할 수 있다.

  프로젝트 아이디 -> bigquery-public-data

  ```bash
  bq bigquery-public-data:
  ```



#### 2. Dataset 생성

- 생성 명령어

  ```bash
  bq mk
  ```

- 데이터베이스 이름을 start 라고 했을때

  ```bash
  bq mk start
  ```



#### 3. 데이터베이스에 저장할 데이터를 가지고 온다.

- 외부에서 파일을 CLI 환경의 폴더위치에 가져오는 명령 (원래 GNU 에서 지원하는 명령 wget 사용)

  ```bash
  wget http://www.ssa.gov/OACT/babynames/names.zip
  ```

- 다운이 잘 됬는지 다음 명령어로 확인

  ```bash
  ls
  ```

- 받은 파일을 현재 폴더에 압축 해제 (폴더 위치는 상관없고 자료를 열람하기 위해 압축해제)

  ```bash
  unzip names.zip
  ```



#### 4. 가져온 데이터를 넣을 TABLE을 만들고 임포트 시킨다.

- 명령은 다음과 같고

  ```bash
  bq load start.names2010 yob2010.txt name:string,gender:string,count:integer
  ```

- 이 명령은 다음과 같이 생각하면 된다.

  datasetID: start
  tableID: names2010
  source: yob2010.txt
  schema: name:string,gender:string,count:integer



#### 5. 만들어진 데이터셋 확인

- 명령어

  ```bash
  bq ls
  ```

- 데이터셋 이름 start

  ```bash
  bq ls start
  ```



#### 6. 만들어진 테이블의 스키마 확인

- 명령어

  ```bash
  bq show start.names2010
  ```

  

#### 7. 쿼리 실행으로 테이블 내 실제 데이터 확인

- 명령어

  ```bash
  bq query
  ```

- SQL 질의어 사용

  ```bash
  bq query "SELECT * FROM start.names2010"
  ```

  

※ Big Query는 WebUI, BigQuery REST API, Command line tool 을 이용해서 사용할 수 있다.

※ NoSQL과 Hbase Shell 을 사용하기 위해서는 Cloud Bigtable instance 을 생성하고 사용한다.

#### **NoSQL 사용 위한 세팅 과정**

1. 환경 변수 설정 (사용 인증을 위한 프로젝트아이디, 인스턴스 변수)
   - project = 프로젝트아이디 > ~/.cbtrc
   - instance = quickstart-instance >> ~/.cbtrc
2. cbt 명령을 reference를 찾아 보고 사용하면 된다.



#### **Hbase Shell 사용 위한 세팅 과정**

(정확한 과정은 없어서 구글에서 제공한 예시 파일을 뜯어야 알수 있을듯)

1. 구글 클라우드 빅테이블 예제 복사

   - 깃 클론

     ```bash
     git clone https://github.com/GoogleCloudPlatform/cloud-bigtable-examples.git
     ```

   - 클론 파일 위치로 이동

     ```bash
     cd cloud-bigtable-examples/quickstart
     ```

   - HBase shell 실행

     ``` bash
     ./quickstart.sh
     ```

     - HBase 프롬프트 창에서 my-table, 칼럼 페밀리 이름은 cf1

       ```bash
       create 'my-table', 'cf1'
       ```

     - 테이블 리스트 확인

       ```bash
       list
       ```

     - 데이터 입력

       ```bash
       put 'my-table', 'r1', 'cf1:c1', 'test-value'
       ```

     - 테이블 데이터 확인

       ```bash
       scan 'my-table'
       ```

     - 테이블 삭제

       ```bash
       disable 'my-table'
       drop 'my-table'
       ```

       

- Cloud Bigtable instance 생성

※ NoSQL 관련된 사용은 **cbt** 명령 이용 



# 분석

# Dataproc: Qwik Start - Console

> 아파치 스파크, 아파치 하둡을 지원한다.
>
> 연산 결과는 본인이 만들어 놓은 클러스터 성능에 따라 결과 시일이 달라지며 사용한 만큼 요금부과

#### 1.  Dataproc을 실행할 클러스터 생성

- 서버의 위치와 처리할 데이터 양을 고려해서 위치와 클러스터 노드수를 신중히 설정해야 한다. (요금때문)

#### 2. 할일(Job)을 클러스터에 분배(Submit)하고 결과를 확인한다.





# Dataproc: Qwik Start - Command Line

#### 1. Dataproc을 실행할 클러스터 생성

- 명령어

  ```bash
  gcloud dataproc clusters create example-cluster
  ```

  dataproc 서비스 안 example-cluster 이름의 클러스터를 만들어라.

#### 2. Job(할일)을 Submit(설정)한다.

- 명령어

  ```bash
  gcloud dataproc jobs submit spark --cluster example-cluster \
    --class org.apache.spark.examples.SparkPi \
    --jars file:///usr/lib/spark/examples/jars/spark-examples.jar -- 1000
  ```

  gcloud 서비스 중 dataproc 서비스의 job을 할당하는데

  - job : spark
  - main method를 포함한 class :  org.apache.spark.examples.Sparkpi
  - 실행할 코드를 포함한 jar 위치 : file:///usr/lib/spark/examples/jars/spark-examples.jar
  - 몇번 실행 할 것인가? : 1000

  ※ 구체적인 명령은 래퍼런스 확인! 

  https://cloud.google.com/sdk/gcloud/reference/dataproc/jobs/submit/spark

#### 3. 클러스터 업데이트

- 명령어

  ```bash
  gcloud dataproc clusters update example-cluster --num-workers 4
  ```

  클러스터를 업데이트 하는데 example-cluster 이름의 클러스터 워크 노드를 4개로 설정해라.



# 시각화

# Dataprep: Qwik Start

- Dataprep 서비스를 이용하게 되면 Trifacta 에 정보를 제공하며 사용조건을 동의해야 사용이 가능하다.

- 데이터를 올려 놓은(올려 놓을) 스토리지 설정 하고 시작한다.

#### 1. 데이터 셋 업로드

#### 2. Add New Recipe를 통해 뷰 활성화

기타 사용에 필요한 정보는 구글 Dataprep 래퍼런스를 통해서 확인







# 머신러닝

# Google Cloud Datalab: Qwik Start

- 파이썬, SQL 파일을 이용할 수 있고

- 구글의 

  - BigQuery

  - Cloud Machine Learning Engine

  - Google Compute Engine

  - Google Cloud Storage using Python

  - SQL

  - JavaScript (for BigQuery user-defined functions)

    들을 같이 활용 할 수 있다.

#### 1. Datalab repository 생성

- /content/datalab/notebooks

  위치의 깃 레포지토리가 Docker container에 만들어지고 Cloud Datalab VM instance 환경안에서 실행된다.

  그래서 VM instance는 'Cloud Datalab VM repo'로 불린다.

- 명령어

  ```bash
  datalab create my-datalab
  ```

  my-datalab 이름으로 기본 환경 생성

  여기서 VM 지역을 선택할 수 있다.  (Y를 눌러 계속 진행하면서 SSH접속 암호를 설정하면 10분안에 설정이 만들어 진다.)

#### 2. 포트 8081로 쥬피터와 유사한 개발환경 접속

- VM이 만들어지면 8081포트로 GCP에서 제공하는 쥬피터와 유사한 개발 환경을 오픈 할 수 있다. 사용 방법은 jupyter notebook과 거의 유사하며 특이한 것은 여기서 작성하고 저장하면 구글의 git으로 버전 관리가 된다는 것이다. 커밋과 개발 기록들을 남길 수 있으며  관련 git log를 Compute Engine의 VMinstances에서 SSH를 브라우저로 오픈해 확인 가능하다.

- **VM instances 에서 SSH 브라우저를 사용해 커밋 기록 확인 방법**

- 명령어

  ```bash
  sudo docker ps
  ```

  실행결과로 나오는 정보에서

  /datalab/run.sh 와 관련된 ID를 복사

- 명령어

  ```bash
  docker exec -it <container-id> bash
  ```

  <container-id>를 대신해 ID를 입력하면 그 컨테이너 아이디 주소로 bash 창 접속이 된다.

- **쥬피터로 작성한 파일들은**

  **cd /content/datalab/notebooks 위치에 존재**

  **폴더 레벨을 저 위치로 바꾸자~!**

- 명령어

  ```bash
  cd /content/datalab/notebooks
  ```

- **git commit 기록 확인**

- 명령어

  ```bash
  git log
  ```

  

# Cloud ML Engine: Qwik Start

