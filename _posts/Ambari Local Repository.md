### Ambari Local Repository

폐쇄망에서 Ambari software package 설치 시 Local Repository 설정 필요.

#### 용어 정의

Repository : 설치할 소프트웨어 패키지 설치 파일 레포지토리

Yum : 레포지토리 내 설치 패키지 관리 매니저

- RHEL / CentOS : "yum"
- SLES : "zypper"

Local Repository : 로컬 네트워크에 호스트 된 레포지토리



#### Repositories

레포지토리 별 관리하는 내용

- Ambari
  - Ambari-Server, Ambari-Agent 그리고 모니터링 소프트웨어 패키지
- HDP
  - Hadoop "Stack" 패키지 (Hadoop, Pig, Hive, HCatalog, Oozie, HBase, Zookeeper, Sqoop)
- HDP-UTILS
  - Ambari와 HDP 유틸리티 패키지 (Ganglia, Nagios, snappy, rrd)
- EPEL (Extra Packages for Enterprise Linux)
  - 엔터프라이즈용 리눅스 패키지
  - Ambari에 필요한 (ex Ganglia, Nagios) 패키지들을 포함하고 있습니다.



### Ambari Repository 사용 과정

