---
title: JAVA_JVM parameters
date: 2021-05-08 22:10:00
categories:
 - LANGUAGE
tag:
 - Java
---

### JVM parameters in Java

기본적으로 JVM 옵션은 두 부분으로 나눠서 생각할 수 있습니다.

1) -X로 시작하는 옵션

- -X로 시작하는 JVM 옵션은 표준이 아니며 (모든 JVM 구현에서 지원되지 않을 수도 있음) 이후 JDK 버전에서 예고 없이 변경 될 수 있습니다.

2)  -XX로 시작하는 옵션

- -XX로 시작하는 옵션은 안정적이지 않으며 일반적으로 사용을 추천하지 않습니다. 해당 옵션은 알림 없이 변경되기도 합니다.

![Hotspot2_JVM_Parameters_GC_Heap](/assets/images/Hotspot2_JVM_Parameters_GC_Heap.png)

<!-- more -->

JVM 옵션을 정확히 아는 것은 GC 튜닝과 실시간 애플리케이션 성능에 중요합니다. 특히 대용량 실시간 플랫폼 처럼 마이크로초 단위로 중요할 땐 더욱 그렇습니다.



### 핵심 JVM Options

1) Boolean JVM 옵션은 -XX로 제어할 수 있습니다.

2) Numeric JVM 옵션은 -XX으로 세팅할 수 있습니다.

- Numeric은 메가바이트의 'm'이나 'M' , 킬로바이트의 'k' 나 'K' 그리고 기가바이트의 'g'이나 'G'를 사용할 수 있습니다.

3) String JVM 옵션은 -XX으로 세팅할 수 있습니다.

- 대략 파일이나 경로 다수의 명령어를 쓰는데 사용할 수 있습니다.

※ `java -help`를  사용해서 기본 옵션(모든 JVM 버전 표준 옵션)을 확인할 수 있습니다. `java -X`는 설치된 특정 JVM에서 사용할 수 있는 비정규 옵션들을 확인할 수 있습니다. 그래서 -X 옵션들은 공지 없이 변경되기 때문에 JVM 인자로서 사용되는 것을 자바어플리케이션 단에서 알고 싶을 때는 아래 메서드를 통해서 확인 할 수 있다.

```java
ManagementFactory.getRuntimeMXBean().getInputArguments()
```



#### Java 힙 사이즈 JVM 메모리 옵션

**-Xms**  초기 자바 힙사이즈 설정

**-Xmx**  자바 최대 힙사이즈 설정

**-Xss>**  자바 스래드 스택 크기 설정



#### GC 정보 출력 JVM 

**-verbose:gc**   GC(Garbage Collector)의 동작과 얼마나 실행되었는지에 대한 로그를 확인할 수 있다.

- GC가 bottleneck 상태인지 확인할 때 주로 사용한다.

**-XXL-:+PrintGCDetails**  `-verbose:gc`의 내용을 포함하지만 추가적으로 새롭게 생성되는 크기와 타이밍에 대한 정보도 추가해서 보여줍니다.

**-XX:-PrintGCTimeStamps**  GC(Garbage Collector)에서 타임 스탬프를 인쇄합니다.



#### Java Garbage Collector 지정 JVM 매개변수

**-XX:+UseParallelGC**  병렬 가비지 콜렉터 사용

**-XX:-UseConcMarkSweepGC**  이전 세대에 동시 마크 스윕 수집을 사용 (1.4.1 버전부터 사용 가능)

**-XX:-UseSerialGC**   직렬 가비지 콜렉터 사용 (5.0 버전부터 사용가능)

시간이 중요한 애플리케이션의 경우 GC 사용에 유의해야 합니다. GC는 시간이 많이 걸리는 작업입니다.



#### JVM 원격 디버깅 옵션

```bash
-Xdebug -Xnoagent -Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=8000
```

JVM이 실행되는 서버에서 위의 옵션을 주고 실행하면 외부에서 해당 서버 주소로 8000포트 debug 접속할 수 있다.



#### 프로파일링 관련 JVM 옵션

`-Xprof`

`-Xrunhprof`



#### Java 클래스 경로 관련 JVM 옵션

**-Xbootclasspath**는 확인없이 로드할 클래스 경로 항목을 지정합니다. 확인을 통해 JVM이 안정적으로 동작하지만 리소스 비용이 많고 시작 지연이 오래 걸리기 때문에 없애줄수 있는 옵션입니다.



#### JVM에서 Perm Gen 크기 변경 옵션

이 옵션은 **java.lang.OutofMemoryError:Perm Gen Space** 을 해결하는데 유용합니다.

`-XX:PermSize`

`-XX:MaxPermSize`

`-XX:NewRatio=2`  Ratio new/old generation size

`-XX:MaxPermSize=64m`   Size of the Permanent Generation



#### JVM 클래스 로딩 및 언로딩 추척 옵션

`-XX:+TraceClassLoading`

`-XX:+TraceClassUnloading`

클래스가 JVM으로 로드되거나 JVM에서 언로드 될 때마다 로깅 정보를 인쇄하는데 사용되는 두가지 JVM 옵션입니다. 이러한 JVM 플래그는 클래스 로더와 관련한 메모리 누수가 있거나 클래스가 언로드되지 않거나 가비지 수집이 안될 때 사용하기에 유용합니다.



#### 로깅 관련 JVM 스위치

`-XX:+TraceClassLoading`   `-XX:+TraceClassUnloading`

로깅에 클래스 정보가 로딩, 언로딩 됩니다. 클래스 누출이 있는지 이전 클래스가 수집되는지 여부를 조사할 때 유용합니다.

`-XX:+PrintCompliation`

핫스팟이 JIT 컴파일을 결정한 각 Java 메소드의 이름을 인쇄합니다. 이 목록은 일반적으로 초기 핵심 Java 클래스 메소드를 표시한 다음 애플리케이션 메소드로 전환합니다. 



#### 디버깅 목적의 JVM 스위치

`-XX:HeapDumpPath=./java_pid.hprof`

힙 덤프를 위한 디렉토리 경로 또는 파일 이름

`-XX:-PrintConcurrentLocks`

스레드 덤프에서 java.util.concurrent 락을 인쇄합니다.

`-XX:-PrintCommandLineFlags`

명령 줄에 나타난 플래그를 인쇄합니다.

