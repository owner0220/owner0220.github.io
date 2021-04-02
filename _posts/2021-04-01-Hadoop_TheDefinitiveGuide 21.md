---
title: Hadoop | The Definitive Guide 21
date: 2021-04-01 08:49:00
categories:
 - hadoop
tag:
 - hadoop
---

# 4장 관련 프로젝트

## Part 21 주키퍼

분산 코디네이션 서비스인 주키퍼를 이용해서 분산 애플리케이션을 구축하는 방법을 알아본다. 주키퍼는 분산 실패를 안전하게 처리할 수 있는 분산 애플리케이션을 구축하기 위한 도구를 제공한다.

<!-- more -->

부분 실패(partial failure)는 분산 서버에 보낸 작업의 실패 여부조차 모르게 되는 상황을 말한다. 예를 들어 네트워크가 메시지 전송 중 끊겨서, 데이터 처리도중 작업이 죽어서 등 결과적으로 작업이 완료 되었는지 여부를 확인 할 수 없는 상태

### 21.1 주키퍼 설치와 실행

**설치 파일 다운로드 후 압축 해제**

```bash
# 설치 위치는 /home/tester 
$ tar xzf zookeeper-x.y.z.tar.gz
```

**환경 변수 등록**

`.bash_profile`

```bash
$ export ZOOKEEPER_HOME=/home/tester/zookeeper-x.y.z
$ export PATH=$PAEH:$ZOOKEEPER_HOME/bin
```

**주키퍼 실행 전 환경 설정 파일 구성**

`/home/tester/zookeeper-x.y.z/conf/zoo.cfg` 혹은 `/etc/zookeeper/config/zoo.cfg` 혹은 ZOOCFGDIR에 지정한 폴더 경로

```bash
tickTime=2000
dataDir=/home/tester/zookeeper
clientPort=2181
```

tickTime : 주키퍼 기본 시간 단위

dataDir : 주키퍼가 데이터를 저장할 로컬 파일시스템 위치

clientPort : 클라이언트 접속 포트

**서비스 스타트**

```bash
$ zkServer.sh start
```

**서비스 동작 확인**

```bash
$ echo ruok | nc localhost 2181
#imok
```



### 21.2 예제

수동적인 분산 데이터 구조가 아니라 어떤 외부 이벤트가 발생했을 때 엔트리의 상태를 능동적으로 변경하는 기능을 설명한다. 주키퍼는 이러한 기능을 서비스로 제공한다.

#### 21.2.1 주키퍼의 그룹 멤버십

주키퍼는 파일과 디렉터리는 없고, 파일과 디렉터리를 통합한 znode라 불리는 노드를 제공한다. znode는 루트(/) 노드를 시작으로 계층적인 네임스페이스를 형성한다.

그룹 이름으로 부모 znode를 생성

그룹 멤버(서버) 이름으로 자식 znode를 생성

#### 21.2.2 그룹 생성

다음은 자바 API로 /zoo 그룹에 해당하는 znode를 생성하는 프로그램을 작성한 예제이다.

```java
public class CreateGroup implements Watcher {
    public static final int SESSION_TIMEOUT = 5000;
    private ZooKeeper zk;
    private CountDownLatch connectedSignal = new CountDownLatch(1);
    
    public void connect(String hosts) throws IOException, InterruptedException {
        zk = new Zookeeper(hosts, SESSION_TIMEOUT, this);
        connectedSignal.countDown();
    }
    
    @Override
    public void process(WatchedEvent event) {
        if (event.getState() == KeeperState.SyncConnected){
            connectedSignal.countDown();
        }
    }
    
    public void create(String groupName) throws KeeperException, IntteruptedException {
        String path = "/" + groupName;
        String createdPath = zk.create(path, null/*data*/, Ids.OPEN_ACL_UNSAFE, CreateMode.PERSISTENT);
        System.out.println("Created" + createdPath);
    }
    
    public void close() throws InterruptedException {
        zk.close();
    }
    
    public static void main(String[] args) throws Exception{
        CreateGroup createGroup = new CreateGroup();
        createGroup.connect(args[0]);
        createGroup.create(args[1]);
        createGroup.close();
    }
}
```

**해당 코드 실행**

```bash
$ export CLASSPATH=ch21-zk/target/classed/:$ZOOKEEPER_HOME/*:$ZOOKEEPER_HOME/lib/*:$ZOOKEEPER_HOME/conf
$ java CreateGroup localhost zoo
```



#### 21.2.3 그룹 가입

이제 멤버를 그룹에 등록하는 프로그램이다. 각 멤버는 프로그램을 각자 실행하여 그룹에 가입한다. 그리고 프로그램이 종료될 때 그룹에서 자동으로 제거될 것이다.

```java
public class JoinGroup extends ConnectionWatcher {
    public void join(String groupName, String memberName) throws KeeperException, InterruptedException {
        String path = "/" + groupName + "/" + memberName;
        String createdPath = zk.create(path, null/*data*/, Ids.OPEN_ACL_UNSAFE, CreateMode.EPHEMERAL);
        System.out.println("Created" + createdPath);
    }
    
    public static void main(String[] args) throws Exception{
        JoinGroup joinGroup = new JoinGroup();
        joinGroup.connect(args[0]);
        joinGroup.join(args[1],args[2]);
        
        //프로세스가 죽거나 스레드가 실패할 때까지 살아있다.
        Thread.sleep(Long.MAX_VALUE);
    }
}
```



**주키퍼와 연결 성공을 대기하는 헬퍼 클래스**

```java
public class ConnectionWatcher implements Watcher{
    private static final int SESSION_TIMEOUT = 5000;
    
    protected Zookeeper zk;
    private CountDownLatch connectedSignal = new CountDownLatch(1);
    
    public void connect(String hosts) throws IOException, InterruptedException {
        zk = new Zookeeper(hosts, SESSION_TIMEOUT, this);
        connectedSignal.await();
    }
    
    @Override
    public void process(WatchedEvent event){
        if (event.getState() == KeeperState.SyncConnected) {
            connectedSignal.countDown();
        }
    }
    
    public void close() throws InterruptedException{
        zk.close();
    }
}
```

※ JoinGroup 코드는 CreateGroup과 유사하다. Join() 메서드에서 그룹 znode의 자식으로 znode를 생성한 후 프로세스가 강제 종료될 때까지 계속 일을 하는 것처럼 잠든다. 나중에 이 프로세스가 종료되면 주키퍼는 임시 znode를 자동으로 제거한다.



#### 21.2.4 그룹 멤버 목록

**그룹 멤버를 찾을 수 있는 프로그램 만들기**

```java
public class ListGroup extends ConnectionWatcher {
    public void list(String groupName) throws KeeperException, InterruptedException{
        String path = "/" + groupName;
        
        try {
            List<String> children = zk.getChildren(path, false);
            if (children.isEmpty()){
                System.out.printf("No members in group %s\n", groupName);
                System.exit(1);
            }
            for (String child: children){
                System.out.println(child);
            }
        } catch (KeeperException.NoNodeException e){
            System.out.printf("Group %s does not exist\n", groupName);
            System.exit(1);
        }
    }
    
    public static void main(String[] args) throws Exception {
        ListGroup listGroup = new ListGroup();
        listGroup.connect(args[0]);
        listGroup.list(args[1]);
        listGroup.close();
    }
}
```

**실행 코드**

```bash
# zoo 그룹 내 멤버 리스트
$ java ListGroup localhost zoo
# zoo 그룹 멤버 추가
$ java JoinGroup localhost zoo duck &
$ java JoinGroup localhost zoo cow &
$ java JoinGroup localhost zoo goat &
# 마지막으로 실행 한 프로세스 아이디를 저장한다. 여기서는 goat의 process id (이 프로세스를 종료하려면 반드시 프로세스 아이디를 기억해야 한다.)
$ goat_pid=$!

# 그룹 멤버의 목록을 다시 확인해 보자
$ java ListGroup localhost zoo
goat
duck
cow

# goat 멤버 제거
$ kill $goat_pid

# 그룹 멤버 목록 다시 확인
$ java ListGroup localhost zoo
duck
cow
```

##### 주키퍼 명령행 도구

※ 코딩 없이 네임스페이스와 상호작용할 수 있는 명령행 도구를 제공한다. /zoo znode아래에 있는 znode를 나열하기 위해 다음과 같은 도구를 사용할 수 있다.

```bash
$ zkCli.sh -server localhost ls /zoo
[cow, duck]
```



#### 21.2.5 그룹 삭제

**그룹과 모든 멤버를 삭제하는 프로그램**

```java
public class DeleteGroup extends ConnectionWatcher {
    public void delete(String groupName) throws KeeperException, InterruptedException {
        String path = "/" + groupName;
        
        try {
            List<String> children = zk.getChildren(path, false);
            for (String child: children){
                zk.delete(path + "/" + child, -1);
            }
            zk.delete(path,-1);
        }catch (KeeperException.NoNodeException e){
            System.out.printf("Group %s does not exist\n", groupName);
            System.exit(1);
        }
    }
    public static void main(String[] args) throws Exception{
        DeleteGroup deleteGroup = new DeleteGroup();
        deleteGroup.connect(args[0]);
        deleteGroup.delete(args[1]);
        deleteGroup.close();
    }
}
```

**명령 실행**

```bash
$ java DeleteGroup localhost zoo
$ java ListGroup localhost zoo
Group zoo does noe exist
```



### 21.3 주키퍼 서비스

#### 21.3.1 데이터 모델

주키퍼는 znode로 불리는 노드를 계층적 트리 형태로 관리한다. 각 znode는 데이터를 저장하고 연관 ACL을 가진다. 주키퍼는 코디네이션 서비스(일반적으로 작은 데이터 파일을 사용한다.)를 위해 설계되었으며 대용량의 데이터를 저장하는 용도로는 사용할 수 없다.

※ znode에 저장할 수 있는 데이터 크기는 1MB로 제한되어 있다.

##### 임시 znode

znode는 임시 또는 영속 타입 중 하나를 가진다. znode의 타입은 처음 생성될 때 결정되며 나중에 변경할 수 없다. 임시 znode는 이를 생성한 클라이언트의 세션이 종료되면 주키퍼가 자동으로 삭제한다.

##### 순차 번호

순차(sequential) znode는 주키퍼가 znode 이름 뒤에 순자 번호를 부여한 것이다. 순차 플래그를 설정하고 znode를 생성하면 단조 증가 카운터(부모 znode가 관리)의 값이 znode 이름에 덧붙여진다.

##### 감시

감시는 znode에 어떤 변화가 있을 때 클라이언트에 관련 이벤트를 통지하는 기능이다. 주키퍼 서비스에는 감시 기능을 설정하는 연산자와 이를 유발하는 트리거 연산자가 따로 있다. 

#### 21.3.2 연산

주키퍼 서비스의 연산에는

- create
- delete
- exists
- getACL, setACL
- getChildren
- getData, setData
- sync

가 있다. 만약 버전 숫자가 일치하지 않으면 update 연산은 실패한다.

##### 다중 갱신

또 다른 주키퍼 연산인 multi는 여러 개의 프리미티브 연산을 하나의 배치로 묶어서 전체 연산의 성공과 실패를 반환하는 방식이다. 따라서 배치에 포함된 어떤 프리미티브 연산은 성공했는데 어떤 연산은 실패했다는 것은 절대 있을 수 없다.

##### API

주키퍼는 클라이언트를 위해 두 개의 핵심 언어인 자바와 C를 위한 바인딩을 제공한다. 동기, 비동기 API를 모두 지원하며 상황에 따라 적합한 것을 골라서 사용하면 된다.

##### 감시 트리거

ACL 연산은 감시의 대상이 아니다. 하나의 감시가 작동할 때 해당 감시 이벤트가 생성되고, 감시 이벤트의 타입은 해당 감시와 그 감시를 유발시킨 연산 모두와 관련 있다.

##### ACL

znode는 ACL 목록과 함께 생성되는데, ACL은 누가 특정 기능을 수행할 수 있는지 결정한다. ACL은 인증 과정을 따르는데, 이는 클라이언트가 자신의 신분을 주키퍼에 확인받는 과정이다. 주키퍼가 제공하는 몇 가지 인증 체계가 있다.

- digest
  - 사용자 이름과 암호로 클라이언트를 인증한다.
- sasl
  - 커버로스를 사용해서 클라이언트를 인증한다.
- ip
  - IP 주소로 클라이언트를 인증한다.

#### 21.3.2 구현

주키퍼 서비스는 두 가지 방식으로 운용할 수 있다.

- 독립 모드

  단일 주키퍼 서버가 존재하고 간단한 구조로 테스트 용도로 적합

- 복제 모드 (replicated mode)

  **앙상블**이라는 컴퓨터 클러스터에서 복제를 통해 고가용성을 달성하는데 유의할 점은 6노드 앙상블에서도 컴퓨터의 두 대의 장애까지만 허용된다는 사실을 유념해야하는데 세 대의 컴퓨터에서 장애가 발생하여 세 대가 남으면 (클러스터 서버 총 대수 기반) 과반수가 되지 않기 때문이다. 이러한 이유로 일반적으로 앙상블에는 홀수의 컴퓨터를 둔다.

#### 21.3.4 일관성

- 순차적 일관성

  특정 클라이언트 업데이트는 보낸 순서대로 적용된다.

- 원자성

  업데이트는 성공 또는 실패 둘 중 하나다. 업데이트가 실패하면 클라이언트는 그 결과를 결코 볼 수 없다.

- 단일 시스템 이미지

  클라이언트는 연결 서버와 관계없이 같은 시스템을 바라본다. 이것은 클라이언트가 같은 세션에 있는 동안 새로운 서버에 연결되었을 때 이전 서버에서 보았던 것보다 더 오래된 상대를 보지 않는다는 것이다.

- 지속성

  업데이트 연산이 성공하면 업데이트 내역은 저장되고 취소되지 않는다. 이는 서버 장애 후에도 업데이트 내역을 유지한다는 것을 의미한다.

- 적시성

  클라이언트가 시스템을 볼 때 지연이 발생하더라도 수십 초 이상 오래된 정보를 보여주지는 않을 것이다. 이는 오래된 데이터를 보게 할 바엔 차라리 서버를 중단시켜 강제로 클라이언트를 좀 더 최신 서버에 연결되도록 한다는 의미이다.

#### 21.3.5 세션

주키퍼 클라이언트는 앙상블의 서버 목록 설정을 가지고 있어야 한다. 시작할 때 목록의 서버 중 하나로 연결을 시도한다. 연결이 실패하면 또 다른 서버에서 시도하고, 그중 하나의 연결이 성공할 때까지 계속 시도한다. 만약 모든 서버가 이용할 수 없는 상태면 결국 실패한다.

##### 시간

시간과 관련된 몇 개의 인자가 있다. 틱타임(tick time)은 주키퍼 시간의 기본 단위고, 앙상블 내의 서버는 수행하는 동작에 대한 스케줄링을 위해 틱타임을 사용한다. 일반적으로 주키퍼 앙상블의 크기가 늘어날수록 세션 타임아웃도 늘어나야 한다. 만약 빈번하게 연결 손실이 있다면 타임아웃을 증가시킨다. JMX를 사용해서 요청 응답 시간의 통계와 같은 주키퍼 지표를 모니터링 할 수 있다.

#### 21.3.6 상태

Zookeeper 객체는 생애 주기 내에서 다른 상태로 전이된다. 우리는 언제든 getState() 메서드를 호출하여 그 상태를 질의할 수 있다.



### 21.4 주키퍼 애플리케이션 구현

분산 애플리케이션에 필요한 가장 기본적인 서비스 중 하나는 환경 설정 서비스로, 공통의 설정 정보가 클러스터의 서버 사이에 공유될 수 있도록 하는 것이다. 가장 간단한 방법은 주키퍼가 환경 설정을 위한 고가용성 저장소로 동작하고, 참가 애플리케이션이 설정 파일을 가져가거나 업데이트할 수 있도록 하는 것이다.

#### 21.4.1 환경 설정 서비스

분산 애플리케이션에 필요한 가장 기본적인 서비스 중 하나는 환경 설정 서비스로, 공통의 설정 정보가 클러스터의 서버 사이에 공유될 수 있도록 하는 것이다. 가장 간단한 방법은 주키퍼가 환경 설정을 위한 고가용성 저장소로 동작하고, 참가 애플리케이션이 설정 파일을 가져가거나 업데이트 할 수 있도록 하는 것이다.

첫째, 우리가 저장하고자 하는 설정 값은 오로지 문자열이며 키는 그냥 znode의 경로이다. 그래서 각 키-값 쌍을 저장하기 위해 하나의 znode를 사용한다.

둘째, 업데이트 수행은 언제나 하나의 클라이언트에서만 한다. 무엇보다 이 방식은 마스터(HDFS의 네임노드 같은 것)

`ActiveKeyValueStore.class`

```java
public class ActiveKeyValueStore extends ConnectionWatcher {
    private static final Charset CHARSET = Charset.forName("UTF-8");
    
    public void write(String path, String value) throws InterruptedException, KeeperException {
        Stat stat = zk.exists(path, false);
        if (stat == null){
            zk.create(path, value.getBytes(CHARSET), Ids.OPEN_ACL_UNSAFE, CreateMode.PERSISTENT);
        }else{
            zk.setData(path, value.getBytes(CHARSET), -1);
        }
    }
}
```



**※ 임의 시간에 주키퍼 속성 값을 업데이트하는 애플리케이션**

```java
public class ConfigUpdater {
    public static final String PATH = "/config";
    
    private ActiveKeyValueStore store;
    private Random random = new Random();
    
    public ConfigUpdater(String hosts) throws IOException, InterruptedException {
        store = new ActiveKeyValueStore();
        store.connect(hosts);
    }
    
    public void run() throws InterruptedException, KeeperException {
        while (true){
            String value = random.nextInt(100) + "";
            store.write(PATH, value);
            System.out.printf("Set %s to %s\n", PATH, value);
            TimeUnit.SECONDS.sleep(random.nextInt(10));
        }
    }
    
    public static void main(String[] args) throws Exception {
        ConfigUpdater configUpdater = new ConfigUpdater(args[0]);
        configUpdater.run();
    }
}
```



**※ 주키퍼의 속성 업데이트를 감시하고 콘솔에 출력하는 애플리케이션**

```java
public class ConfigWatcher implements Watcher {
    private ActiveKeyValueStore store;
    
    public ConfigWatcher(String hosts) throws IOException, InterruptedException {
        store = new ActiveKeyValueStore();
        store.connect(hosts);
    }
    
    public void displayConfig() throws InterruptedException, KeeperException {
        String value = store.read(ConfigUpdater.PATH, this);
        System.out.printf("Read %s as %s\n", ConfigUpdater.PATH, value);
    }
    
    @Override
    public void process(WatchedEvent event){
        if (event.getType() == EventType.NodeDataChanged){
            try {
                displayConfig();
            } catch (InterruptedException e){
                System.err.println("Interrupted. Exiting");
                Thread.currentThread().interrupt();
            } catch (KeeperException e){
                System.err.println("KeeperException: %s. Exiting.\n", e);
            }
        }
    }
    
    public static void main(String[] args) throws Exception {
        ConfigWatcher configWatcher = new ConfigWatcher(args[0]);
        configWatcher.displayConfig();
        
        Thread.sleep(Long.MAX_VALUE);
    }
}
```





#### 21.4.2 탄력적인 주키퍼 애플리케이션

##### InterruptedException

작업이 인터럽트 되었을 때 실행되는 것이다.

##### KeeperException

주키퍼 서버가 오류를 발생시킬 때 또는 서버와의 통신에 문제가 있을 때 던져진다.

- 상태 예외
- 회복 가능한 예외
  - 멱등(idempotent)
  - 비멱등(nonidempotent)
- 회복 불가능한 예외

##### 신뢰성 있는 환경 설정 서비스

#### 21.4.3 락 서비스

분산 락(distributed lock)은 프로세스 집단에서 상호 배제하기 위한 메커니즘이다.

1. 임시 순차 znode를 락 znode 아래에 lock-이라는 이름으로 생성하고 순차 znode의 실제 경로명을 기억한다.(create 연산의 반환값).
2. 락 znode의 자식을 얻고 감시를 설정한다.
3. 1에서 생성한 znode의 경로명이 2에서 반환한 자식 중에서 가장 낮은 번호면 락을 획득한다. 그리고 종료한다.
4. 2에서 설정한 감시로부터 통지를 기다리고 2단계로 간다.

##### 집단 효과

위 알고리즘이 올바르더라도 몇 가지 문제가 있다. 첫 번째 문제는 집단효과(hard effect)를 겪는다는 것이다. '집단 효과'란 오직 적은 수의 클라이언트만 진행할 수 있는 이벤트를 수많은 클라이언트가 똑같이 통지받는 것을 말한다. 여기서 하나의 서버만 성공적으로 락을 획득할 수 있지만, 모든 클라이언트에 감시 이벤트를 보내고 관리하는 과정은 혼잡을 야기하여 주키퍼 서버에 압박을 준다.

##### 회복 가능한 예외

락 알고리즘의 또 다른 문제는 연결 손실로 인해  create 연산이 실패할 때를 처리하지 않는 것이다. 이때 이 연산이 성공했는지 실패했는지 알 수 없다는 것을 기억하라. 별도로 식별자를 부여하여 관리한다면 해당 문제를 해결 할 수 있다.

##### 회복 불가능한 예외

주키퍼 세션이 만료되면 클라이언트가 생성한 임시 znode가 삭제되면서 락을 포기한다. 이 과정에서 관련 작업을 총괄하는 것은 락 구현체가 아니라 애플리케이션이다. 사용자가 어떻게 코딩하느냐에 따라 문제는 달라지기 때문에 가늠할 수 없다.

##### 구현

주키퍼는 클라이언트가 쉽게 사용할 수 있는 WriteLock이라는 자바 언어로 된 실 서비스가 가능한 락을 구현해놓았다.

#### 21.4.4 더 많은 분산 데이터 구조와 프로토콜

주키퍼로 구현할 수 있는 많은 분산 데이터 구조와 프로토콜이 있는데, 배리어(barrier), 큐, 2단계 커밋(two-phase commit) 등이 있다. 흥미로운 점은 이것은 동기 프로토콜이지만 우리는 이를 비동기 주키퍼 프리미티브(통지와 같은 것)로 구축할 것이다.

##### 북키퍼와 헤드윅

북키퍼(BookKeeper)는 고가용성의 신뢰성 높은 로깅 서비스다. WAL(write-ahead logging)을 제공하는데 사용될 수 있는데, WAL은 저장소 시스템 안에 있는 데이터의 무결성을 보장하는 데 사용되는 일반적인 기술이다.

북키퍼 클라이언트는 레저(ledger)라는 로그를 생성하고, 레저에 추가된 각 레코드를 레저엔트리(ledger entry)라고 한다. 레저 엔트리는 간단하게 바이트 배열로 되어 있다. 레저는 부키(bookie)가 관리하며, 부키는 레저 데이터를 복제하는 서버다. 레저 데이터는 주키퍼 내에 저장되지 않고 오직 메타데이터만 저장됨을 명심한다.

### 21.5 주키퍼 실 서비스

실 서비스 환경에서는 주키퍼를 복제모드로 운영해야 한다. 여기서는 주키퍼 서버 앙상블을 운용할

#### 21.5.1 탄력성과 성능

#### 21.5.2 환경 설정



### 21.6 참고 도서

