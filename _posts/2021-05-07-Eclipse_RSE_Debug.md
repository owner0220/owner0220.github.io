---
title: Eclipse Debugging (RSE, TM)
date: 2021-05-07 22:53:00
categories:
 - PRODUCT
tag:
 - Eclipse
---

### Eclipse RSE Debugging

Remote Debugging 은 Java나 C++로 다른 개발 환경의 기기에서 실행되는 것을 디버깅하는 것을 말합니다. 로컬에서 디버깅하는 것이 일반적이지만 환경이 허락하지 않을 땐 Remote Debugging Java Application이 해결책이 될 수 있습니다.

<!-- more -->

일반적인 IDE인 NetBeans, Eclipse, IntelliJ는 Remote debug Java Program을 가능하게 합니다. 하지만 종류에 따라서 30일 체험판으로만 기능이 제공될 수도 있습니다. 

### Eclipse에서 Java remote debugging을 사용할 때

대부분의 Java project는 Linux OS에서 동작하지만, 사용자는 윈도우에서 Eclipse 같은 IDE를 사용하는 경우가 많습니다. 윈도우에서 개발하고 싶지만 환경이나 platform-dependent library나 특정 Linux 모듈로 인해서 할 수 없거나 너무 커서 윈도우 환경에서 개발을 할 수 없는 경우도 있습니다.

여러 방법이 있지만 여기서 Eclipse의 remote debugging Java application를 사용할 수 있습니다. Eclipse에는 "Remote debugging"을 debug Java application running  on remote Linux or Windows Server 기능을 제공합니다.

#### 소스가 있는 원격 서버에서 준비할 내용

Java application을 디버깅하기 위해서는 JVM debug 옵션이 필요합니다.

```bash
# 명령어 내용
$ java -Xdebug -Xrunjdwp:transport=dt_socket, address=<디버깅포트>, server=y suspend=n -jar <빌드된 Java application jar 파일>

# 명령어 옵션 설명
# suspend=y 자바 어플리케이션이 이클립스 연결될 때까지 기다렸다가 연결되면 실행
# suspend=n 자바 어플리케이션이 일반적으로 실행되지만, breakpoints에서 멈춘다.

# 예시
$ java -Xdebug -Xrunjdwp:transport=dt_socket, address=8001, server=y suspend=n -jar /usr/test.jar
```

위의 명령을  실행해 주면 **Java Debug Wire Protocol(JDWP)**을 통해서 입력해준 디버깅포트로 이클립스가 연결될 때까지 test 어플리케이션이 실행되지 않습니다. 

**Failed to connect to remote VM. Connection refused**

**Connection refused: connect**

위의 에러가 떴을 때는

- 리모트 서버에 JAVA 애플리케이션이 실행 중인지 먼저 확인해야 합니다.
- 리모트 서버 방화벽이 해당 포트 허용인지 확인
- 포트 주소 확인 이클립스에 입력한 호스트 주소와 포트 주소가 맞는지 확인
- 리모트 서버에서 JAVA 애플리케이션을 실행할 때 위의 예시와 같이 JVM debug 세팅을 했는지 확인



**※ 디버깅 방법은 여러가지가 있을 수 있는데 편한 것 하나를 사용하면 됩니다.**





#### 디버깅 방법1 : Eclipse 활용 | Eclipse 사용하는 서버에서 준비할 내용

1. Java Project 소스를 Eclipse에 준비합니다.

   - 동일한 소스를 카피해서 Eclipse에 저장
   - RSE plugin을 설치해서 create remote server project를 해서 소스 준비

2. Package Explorer에서 디버깅하고자 하는 Project를 선택하고 "Run>>Debug Configuraotions"을 누릅니다.

3. "Remote Java Application" 아이콘을 왼쪽에서 찾아 오른쪽 마우스 클릭 후 New 선택

4. 프로젝트를 선택, 외부 리모스 서버에 연결해서 결과를 가져올 것이기 때문에

   - Connection Type: Standard (Socket Attach)
   - Connection Properties
     - Host : 연결할 서버
     - Port : 연결할 서버에서 Debug 모드로 실행되어 있는 Java Application
   - Allow termination of remote VM (이클립스에서 연결을 컨트롤 할 수 있도록 하려면 선택)

   ※ 디버그를 실행하기 전에 리모트 서버에는 Debug mode로 Java application이 위에 입력한 정보대로 실행되어 있어야 합니다.



#### 디버깅 방법2 : JDB(Java debugger)

※ 디버깅 모드로 위와 같이 실행 됬다면 Java debugger인 "jdb"를 연결할 수 있다.

```bash
$ jdb -attach 8001
```









#### 