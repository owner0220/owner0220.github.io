---
title: 하둡 Configuration
date: 2021-03-02 19:35:00
categories:
 - hadoop
tag:
 - hadoop
 - prerequsition
---

### Non-Secure Mode 하둡 설정

- Hadoop Java Configuration 
  - Read-only default configuration
    - core-default.xml
  - hdfs-default.xml
  - yarn-default.xml
    - mapred-default.xml
  - Site-specific configuration
    - etc/hadoop/core-site.xml
    - etc/hadoop/hdfs-site.xml
    - etc/hadoop/yarn-site.xml
    - etc/hadoop/mapred-site.xml

<!-- more -->

/etc/hadoop/bin 폴더 안에 있는 하둡 스크립트를 통해 특정 값들을 변경 할 수도 있습니다.

- /etc/hadoop/hadoop-env.sh
- /etc/hadoop/yarn-env.sh



하둡 클러스터를 설정하기 위해서는 하둡 데몬이 실행 될 수 있는 환경을 세팅해 줘야 한다.

**HDFS daemons**

- NameNode
- SecondaryNamaNode
- DataNode

**YARN daemons**

- ResourceManager
- NodeManager
- WebAppProxy

- ( MapReduce Job History Server ) - if MapReduce is to be used



### Configuring Environment of Hadoop Daemons

- /etc/hadoop/hadoop-env.sh
- /etc/hadoop/mapred-env.sh
- /etc/hadoop/yarn-env.sh

**JAVA_HOME** should be set.



#### /etc/hadoop/hadoop-env.sh 수정

Daemon 별 동작 메모리 설정 변수

```bash
ex) export HDFS_NAMENODE_OPTS="-XX:+UseParallelGC -Xmx4g"
```

※ 다른 예시들은 ```/etc/hadoop/hadoop-env.sh``` 에서 확인 할 수 있습니다.

**NameNode**

- HDFS_NAMENODE_OPTS

**DataNode**

- HDFS_DATANODE_OPTS

**Secondary NamaNode**

- HDFS_SECONDARYNAMENODE_OPTS

**ResourceManager**

- YARN_RESOURCEMANAGER_OPTS

**NodeManager**

- YARN_NODEMANAGER_OPTS

**WebAppProxy**

- YARN_PROXYSERVER_OPTS

**Map Reduce Job History Server**

- MAPRED_HISTORYSERVER_OPTS



**HADOOP_PID_DIR**, **HADOOP_LOG_DIR** 이 두 환경 변수는 데몬의 프로세스 id 파일과 로그 파일 저장 위치를 말하는데 설정이 되어 있지 않으면 Symlink attack 위험이 있습니다.

```bash
HADOOP_HOME=/path/to/hadoop

export HADOOP_HOME
```





#### /etc/hadoop/core-site.xml  수정

fs.defaultFS=```hdfs://host:port```  (NameNode URI)

io.file.buffer.size=```131072``` (SequenceFile Size)



#### /etc/hadoop/hdfs-site.xml 수정

- **NameNode 관리 위한 설정**

dfs.namenode.name.dir=```/log```     (NameNode가 namespace와 transactions logs를 기록하는 위치)

dfs.hosts=                                          (허용한 데이터 노드)

dfs.hosts.exclude=                           (예외처리한 데이터 노드) 

dfs.blocksize=```268435456```                (허용한 데이터 노드)

dfs.namenode.handler.count=```100``` (데이터 노드로 부터오는 RPC를 관리하기 위한 Namenode 서버 쓰레드 갯수)



- **DataNode 관리 위한 설정**

dfs.datanode.data.dir=```/data``` (DataNode에서 저장하는 block 위치)



#### /etc/hadoop/yarn-site.xml 수정

- **ResourceManager와 NodeManager 관리를 위한 설정**

yarn.acl.enable=```true/false```     (ACL기능 설정 기본적으로 false)

yarn.admin.acl=```Admin```                (유저나 그룹을 콤마를 기준으로 접근 가능하도록 세팅 기본적으로 전부 허용을 의미하는 *)

yarn.log-aggregation-enable=```false```   (로그 집계 설정)



- **ResourceManager 관리를 위한 설정**

yarn.resourcemanager.address=```yarn.resourcemanager.hostname 이 설정되어 있다면 알아서 세팅됨```

yarn.resourcemanager.scheduler.address=```yarn.resourcemanager.hostname 이 설정되어 있다면 알아서 세팅됨```

yarn.resourcemanager.resource-tracker.address=```yarn.resourcemanager.hostname 이 설정되어 있다면 알아서 세팅됨```

yarn.resourcemanager.admin.address=```yarn.resourcemanager.hostname 이 설정되어 있다면 알아서 세팅됨```

yarn.resourcemanager.webapp.address=```yarn.resourcemanager.hostname 이 설정되어 있다면 알아서 세팅됨```

yarn.resourcemanager.hostname=```host```  (yarn.resourcemanager address 기본 주소가 됨)

yarn.resourcemanager.scheduler.class=

yarn.scheduler.minimum-allocation-mb=

yarn.scheduler.maximum-allocation-mb=

yarn.resourcemanager.nodes.include-path=

yarn.resourcemanager.nodes.exclude-path=

