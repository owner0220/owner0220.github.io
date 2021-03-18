---
title: Architecting Modern Data Platforms 07
date: 2021-03-08 08:55:00
categories:
 - hadoop
tag:
 - hadoop
---

### 7장 클러스터의 프로비저닝

하둡 클러스터 노드를 프로비저닝하고 배열하는 방법에 대해 알아본다. 대부분의 하둡 노드는 리눅스 상에서 동작하므로 이번 장에서 설명하는 운영체제와 관련되는 내용들은 어떤 환경에도 적용할 수 있다.

<!-- more -->

#### 운영체제

- 서버 부트스트랩
  - *부트스트랩(Bootstrapping) '컴퓨터 시동하기' ; 컴퓨터는 OS가 작동가능한 상태로 만들기 위해 OS를 메모리에 로드하고 여러 초기화 작업을 수행해야 한다. 이 과정을 Bootstrapping, 줄여서 booting 이라고 한다.
    - ex) PXE 부트
- OS 구성
  - OS 전용 설정 서비스 접촉 담당(실제 사용자가 필요한 정보를 입력, 템플릿이 선택할 모든 옵션을 기록한 파일을 인스톨러에 전달하면, 설치가 자동적으로 진행된다.)
    - minimal installer
    - Kickstart(레드햇 리눅스)
- OS 설정
  - OS가 동작되면 노드는 자신에게 할당된 특정 작업을 실행하도록 설정
    - 소프트웨어 설정 관리(SCM, Software Configuration Management)
      - 앤서블
      - 셰프
      - 퍼펫
      - 새틀라이트(Satellite); 오픈소스 Foreman 포함되어 있음

##### 운영체제 선택

- 온라인 리포지토리
- 오프라인 리포지토리

##### 하둡을 위한 OS 설정

- 파일시스템 설정 조정

  - 접근 시간 제어의 비활성화  (파일 작업의 속도가 빨라짐)

    /etc/fstab 설정 파일 noatime 옵션 추가

    ```bash
    $ vi /etc/fstab
    # 마운트된 디스크에 noatime 옵션 적용
    /dev/sdb /data01 ext3 defaults,noatime 0
    ```

    런타임에 디스크 다시 마운트 (아래 작업은 모든 볼륨마다 반복해서 실행해야 한다.)

    ```bash
    $ mount -?o remount /data01
    ```

  - 관리 목적으로 예약된 디스크 공간의 축소 (물리 디스크에 더 많은 저장 공간을 활용)

    파일시스템을 생성하면서 공간을 할당하는 명령어

    ```bash
    $ mkfs.ext3 -m 0 /dev/sdb
    ```

    파일시스템이 생성된 이후에 공간을 조정하는 명령어

    ```bash
    $ tune2fs -m 0 /dev/sdb
    ```

    값을 0으로 지정한다는 것은 관리 목적의 작업을 위한 공간을 모두 축소한다는 것이다. (**단, OS 파일이 있는 드라이브에 대해서는 절대 이 명령을 실행해서는 안된다.**)

- 프로세스 제한의 증가

  자원이 부족할 경우 클러스터 동작 불능이 되거나 성능에 심각한 영향을 초래한다. (이 제한은 기술적 사용자 계정 단위로 진행되어야 하며 시스템이 재시작하더라도 영속적으로 적용되어야 한다.)

  ```bash
  # 파일 핸들러 값 조정(기본값 1024)
  $ echo hdfs - nofile 32768 >> /etc/security/limit.conf
  $ echo mapred - nofile 32768 >> /etc/security/limit.conf
  $ echo hbase - nofile 32768 >> /etc/security/limit.conf
  
  # 프로세스 값 조정
  $ echo hdfs - nproc 32768 >> /etc/security/limit.conf
  $ echo mapred - nproc 32768 >> /etc/security/limit.conf
  $ echo hbase - nproc 32768 >> /etc/security/limit.conf
  ```

- 스와핑 축소

  서버 주 메모리는 유한하므로 OS는 필요에 따라 사용하지 않는 메모리 페이지를 디스크로 옮긴다. 이와 같은 Swapping은 프로세스가 실행되는 동안 성능 저하의 원인이 된다. Swappiness를 최소화 하도록 한다.

  ```bash
  # 일시적인 설정
  $ echo 1 > /proc/sys/vm/swappiness
  # 영구적인 설정
  $ echo "vm.swappiness = 1" >> /etc/sysctl.conf
  ```

  

- 시간 동기화

- 고급 네트워크 설정의 적용

- 네임 서비스 캐싱 활성화

- OS 수준 최적화 비활성화

- 보안이 강화된 리눅스(SE Linux) 비활성화

- 로컬 방화벽 비활성화

- 기타 까다로운 옵션들

##### 자동화된 설정의 예

#### 서비스 데이터베이스

##### 필요한 데이터베이스

- 하이브 메타스토어
- 아파치 우지
- 휴
- 아파치 레인저/ 아파치 센트리
- 아파치 앰버리/ 클라우데라 매니저

##### 데이터베이스 통합 옵션

- 테스트 구성
- 운영 환경 구성
- 공유 데이터베이스 모드
- 개별 데이터베이스 모드
- 고가용성 데이터베이스 모드
- 호스티드(hosted) 데이터베이스 모드

##### 데이터베이스에 대한 검토

#### 하둡 배포

##### 하둡 배포판

- 순수한 아파치
- 벤더 하둡 배포판
- PaaS 하둡

##### 설치 방법의 선택

- 소스코드
- 바이너리 릴리스
- 소프트웨어 설저 관리 도구
- 하둡 배포판 인스톨러

##### 배포판의 아키텍처

##### 설치 절차

- 수동 설치
- 완전한 자동 설치

