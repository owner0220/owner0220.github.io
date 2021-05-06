---
title: Hadoop | The Definitive Guide 10
date: 2021-03-26 08:51:00
categories:
 - PRODUCT
tag:
 - hadoop
---

# 3장 하둡 운영

## Part 10 하둡 클러스터 설정

다수의 머신으로 구성된 클러스터 환경에서 하둡을 구동하기 위한 설정 방법 및 구축 방법을 설명한다.

<!-- more -->

#### 구축 방법

- 아파치 타르볼 (.tar)
  - 사용자가 설치 파일, 설정 파일, 로그 파일의 위치를 직접 결정하고 별도로 권한 설정을 해야 한다.
- 패키지 (.rpm / .deb)
  - 일관된 파일시스템 레이아웃 제공(정해진 위치에 설치)
  - 퍼펫(puppet)과 같은 설정 관리 도구 함께 사용 가능
- 하둡 클러스터 관리 도구
  - 웹 사용자 인터페이스를 제공, 하둡 클러스터를 구축 시 사용자와 운영자에게 사용 권장
  - 고가용성(HA), 복잡한 구성 지원 기능(wizard), 통합 모니터링, 로그 검색, 롤링 업그레이드 추가 기능 제공
    - 클라우데라 매니저(Cloudera Manager)
    - 아파치 암바리(Apache Ambari)



### 10.1 클러스터 명세

범용 하드웨어에서 구동되도록 설계되어있다.(비싼 전용 장비에 종속될 필요가 없음을 의미)

```markdown
프로세서 : 헥사/옥토-코어 3GHZ CPU 2EA
메모리 : 64~512GB ECC RAM
스토리지 : 1~4TB SATA 디스크 12~24개
네트워크 : 링크 통합 기능을 지원하는 기가비트 이더넷
```

[ [Yahoo JBOD vs RAID BENCHMARK](https://markmail.org/message/xmzc45zi25htr7ry) ]

※ ECC RAM 이 아니면 체크섬 오류가 발생했던 이력이 있다. 더불어 JBOD, RAID 사용에 있어 JBOD를 사용하는 것을 권장한다. (Yahoo에서 수행한 벤치마크 성능비교에서 JBOD는 RAID 0 보다 성능이 좋은 기록이 있다.)

#### 10.1.1 클러스터 규모 결정

클러스터 규모를 결정할 때는 "얼마나 빨리 클러스터가 커질 것인가?"에 기준을 두고 고려한다. 예를 들어 하루에 데이터가 1TB 씩 증가하고 replication 설정(HDFS의 복제인수)이 3으로 설정되어 있다면 하루에 3TB의 저장소가 필요할 것이다. 임시 파일과 로그를 위한 공간(약 30%)를 추가로 고려하면 대략 1주일에 한 대의 컴퓨터(28TB ; 4TB * 7)가 필요하다.

##### 마스터 노드 시나리오

네임노드, 보조 네임노드, 리소스 매니저, 히스토리 서버와 같은 마스터 데몬을 어떤 노드에서 실행할지 결정하는 방법은 다양하며 클러스터 규모에 따라서 다르다. (사업소 마다 필요한 기능이 다르고 구성도 다르기 때문)

- **10대 이내의 클러스터에서는...**
  - 단일 마스터 컴퓨터에 **네임노드, 리소스 매니저** 구동 가능
    - 단, 네임노드의 메타데이터 사본이 원격 파일시스템에 최소 한 개 이상 저장되어 있어야 한다.

- **그 이상의 클러스터에서는...**
  - 네임노드와 리소스 매니저를 각각 다른 컴퓨터로 분리한다.



※ 네임노드 전체 네임스페이스의 파일 및 블록에 대한 메타데이터를 모두 메모리에 보관해야 하므로 상당한 용량의 메모리가 필요하다. 보조 네임노드는 가끔씩 실행되지만 체크포인트 작업을 수행할 때 네임노드와 거의 비슷한 용량의 메모리가 필요하다.





#### 10.1.2 네트워크 토폴로지

##### 랙 인식 기능

하둡의 성능을 위해 하둡에 네트워크 토폴로지를 설정해야 한다.

방법 : 

- 스크립트 작성

  - 자바 인터페이스 구현 (ScriptBasedMapping ; 매핑 정보 기술 사용자 정의 스크립트 구현체)

    - 입력되는 매핑정보 ex) hadoop_conf / `topology.data`

      ```bash
      hadoopdata1.ec.com     /dc1/rack1
      hadoopdata1            /dc1/rack1
      10.1.1.1               /dc1/rack2
      ```

      별도 매핑 정보를 작성하지 않으면 모든 노드를 /default-rack 하나로 간주

    - 스크립트 작성 `topology.sh`

      ```bash
      HADOOP_CONF=/etc/hadoop/conf 
      
      while [ $# -gt 0 ] ; do
        nodeArg=$1
        exec< ${HADOOP_CONF}/topology.data 
        result="" 
        while read line ; do
          ar=( $line ) 
          if [ "${ar[0]}" = "$nodeArg" ] ; then
            result="${ar[1]}"
          fi
        done 
        shift 
        if [ -z "$result" ] ; then
          echo -n "/default/rack "
        else
          echo -n "$result "
        fi
      done 
      ```

      

- 스크립트 파일 위치 정의

  - topology.script.file.name = /topology.sh

  

네임노드는 각 블록의 복제소를 결정할 때 네트워크 위치 정보를 사용한다. 이때 노드와 랙 같은 네트워크 위치 정보는 트리 형태로 표현되며 맵리듀스의 스케줄러는 맵 태스크의 입력으로 사용될 가장 가까운 데이터를 찾을 때 네트워크 위치 정보를 이용한다.



### 10.2 클러스터 설치 및 설정

클러스터를 설치할 경우 클라우데라 매니저나 암바리와 같은 하둡 클러스터 관리 도구 중 하나를 고려할 것을 권장한다.

#### 10.2.1 자바 설치

하둡은 자바가 설치된 유닉스 및 윈도우 운영 체제에서 실행된다.

> Test 완료 된 설치 내용과 JDK version [Hadoop Java Vesions](https://cwiki.apache.org/confluence/display/HADOOP2/HadoopJavaVersions)

#### 10.2.2 유닉스 사용자 계정 생성

동일한 컴퓨터에서 수행되는 다른 서비스와 하둡 프로세스를 구분하기 위해 하둡 전용 사용자 계정을 생성하는 것이 좋다.

- hdfs, mapred, yarn (별도의 사용자 계정으로 실행된다.)
  - 동일한 hadoop 그룹에 속해야 한다.

#### 10.2.3 하둡 설치

배포판 다운로드 및 /usr/local (/opt 표준) 위치에 압축 해제

```bash
$ cd /usr/local
$ sudo tar xzf hadoop-x.y.z.tar.gz
$ sudo chown -R hadoop:hadoop hadoop-x.y.z
$ export HADOOP_HOME=/usr/local/hadoop-x.y.z
$ export PATH=$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin
```

#### 10.2.4 SSH 구성

하둡 제어 스크립트는 SSH를 이용해 전체 클러스터를 대상으로 작업을 수행한다. 머신간 자유로운 접속을 위해 SSH 로그인 설정을 미리 해둔다.

- hdfs계정과 yarn 계정으로 두번 실행한다.

```bash
# 공통 우선 실행
$ ssh-keyhen -t rsa -f ~/.ssh/id_rsa
# 방법1
$ cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
# 방법2
$ ssh-copy-id <계정이름>@<접속 허용할 주소>
```

※ 개인키는 -f 옵션으로 지정된 ~/.ssh/id_rsa 에 있으며, 공개키는 ~/.ssh/id_rsa.pub 에 있다.

#### 10.2.5 하둡 구성

클러스터 기반 분산 모드로 구동하기 위해 하둡 설정을 정학히 해야 한다.

#### 10.2.6 HDFS 파일시스템 포맷

```bash
$ hdfs namenode -format
```

※ 네임노드가 파일시스템의 모든 메타데이터를 직접 관리하고 데이터 노드는 클러스터에 동적으로 포함되거나 제외될 수 있기 때문에 데이터노드는 초기 포맷 과정에 전혀 관여하지 않는다.

#### 10.2.7 데몬의 시작과 중지

hdfs 계정으로 명령 수행

```bash
# 데몬 시작
$ start-dfs.sh
# 네임노드 호스트명 찾아 표시
$ hdfs getconf -namenodes
```

yarn 계정으로 명령 수행

```bash
# YARN 데몬 시작
$ start-yarn.sh
```

mapred 계정으로 구동하는 MapReduce 데몬(잡 히스토리 서버)

```bash
$ mr-jobhistory-daemon.sh start historyserver
```



#### 10.2.8 사용자 디렉터리 생성

사용자별 디렉터리에 대한 소유권한을 부여한다.

```bash
$ hadoop fs -mkdir /user/username
$ hadoop fs -chown username:username /user/username
```

사용자별 사용 용량 제한

```bash
$ hdfs dfsadmin -setSpaceQuota 1t /user/username
```



### 10.3 하둡 환경 설정

하둡 기본 디렉터리 아래의 /etc/hadoop/ 디렉터리에 있다. --config 옵션으로 디렉터리를 지정하여 데몬을 실행하면 설정 디렉터리를 다른 곳으로 옮길 수 있다.

- hadoop-env.sh
- mapred-env.sh
- yarn-env.sh
- core-site.xml
- hdfs-site.xml
- mapred-site.xml
- yarn-site.xml

#### 10.3.1 환경 설정 관리

하둡에서는 환경 설정 정보를 단일 또는 전역 장소에서 관리하지 않는다. 그 대신 클러스터에 있는 각 하둡 노드는 자신만의 환경 설정 파일을 가지고 있다. 관리자는 이 파일들을 전체 클러스터에 동기화할 수 있어야 한다.

등급별 설정 관리하는 도구 예

- 셰프(Chef)
- 퍼펫(Puppet)
- CFEngine
- Bdfg2

#### 10.3.2 환경 설정

`hadoop-env.sh` 스크립트 변수를 설정하는 방법을 다룬다. 맵리듀스와 YARN의 설정 파일은 각각 mapred-env.sh, yarn-env.sh 며, 각 구성요소에 관한 변수를 설정 할 수 있다. hadoop-env.sh 파일에서 설정한 속성의 값은 맵리듀스와 YARN의 설정 파일에서 재정의 할 수 있다.

##### 자바

자바 라이브러리의 위치는 hadoop-env.sh의 JAVA_HOME에 정의한다. 만약 없다면 쉘 환경변수인 환경변수인 JAVA_HOME을 찾는다. (hadoop-env.sh에 위치를 명확하게 설정하여 전체 클러스터가 동일한 버전의 자바를 사용할 수 있도록 하는 것이 더 좋은 방법이다.)

##### 메모리 힙 크기

기본적으로 하둡은 각 데몬당 1000MB(1GB)의 메모리를 기본으로 할당한다. `hadoop-env.sh` 의 HADOOP_HEAPSIZE에 의해 제어된다. 또한 각 데몬의 힙 크기를 변경할 수 있는 환경 변수도 있다. ex) `yarn-env.sh` 의 YARN_RESOURCE_HEAPSIZE를 설정하면 리소스 매니저의 힙 크기를 재정의할 수 있다.

하둡 데몬 1GB - 수백만개 파일 다룰 때는 충분

※ 메모리 용량의 산정 경험을 바탕으로 계산해보면 저장소 백만 블록당 1,000MB 정도의 메모리 필요.

##### 시스템 로그파일

하둡이 생성하는 시스템 로그파일은 별도로 지정하지 않으면 $HADOOP_HOME/logs에 저장된다. `hadoop-env.sh`의 환경변수인 HADOOP_LOG_DIR로 변경할 수 있다. 하둡이 설치된 디렉터리와 로그파일을 저장하는 디렉터리는 서로 분리하는 것이 좋다. 보통 로그파일은 /var/log/hadoop에 저장하며, hadoop-env.sh에 다음과 같이 추가한다.

```bash
export HADOOP_LOG_DIR=/var/log/hadoop
```

※ 지정한 로그 디렉터리가 존재하지 않으면 자동으로 생성된다.(생성되지 않으면 관련된 하둡 사용자 계정에 생성 권한이 있는지 확인하라.)

표준 출력과 표준 에러 로그가 함께 기록되는 로그 파일

- hadoop-hdfs-datanode-ip-10-45-174-112.log.2014-09-20.1
- hadoop-hdfs-datanode-ip-10-45-174-112.log.2014-09-20.5
  - 데몬이 재시작 될때만 순환되고 최근 다섯개 로그만 보관

로그파일의 이름의 사용자는 hadoop-env.sh HADOOP_IDENT_STRING 속성의 값을 기본으로 따른다.

##### SSH 설정



#### 10.3.3 중요한 하둡 데몬 속성

##### HDFS

##### YARN

##### YARN과 맵리듀스의 메모리 설정

##### YARN과 맵리듀스의 CPU 설정

#### 10.3.4 하둡 데몬 주소 및 포트

#### 10.3.5 다른 하둡 속성

##### 클러스터 멤버십

##### 버퍼 크기

##### HDFS 블록 크기

##### 예약된 저장 공간

##### 휴지통

##### 잡 스케줄러

##### 느린 리듀스 시작

##### 단락 지역 읽기



### 10.4 보안

#### 10.4.1 커버로스와 하둡

#### 10.4.2 위임 토큰

#### 10.4.3 다른 보안 강화 사항



### 10.5 하둡 클러스터 벤치마크

#### 10.5.1 하둡 벤치마크

##### TeraSort를 이용한 맵리듀스 벤치마킹

##### 다른 벤치마크

#### 10.5.2 사용자 잡