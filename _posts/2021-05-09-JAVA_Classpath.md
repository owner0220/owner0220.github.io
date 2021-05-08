---
title: JAVA_CLASSPATH
date: 2021-05-08 23:45:00
categories:
 - LANGUAGE
tag:
 - Java
---

### Java에서 CLASSPATH란?

﻿자바에서 CLASSPATH는 자바 프로그램에서 사용할 (JVM ClassLoaders가 사용할) 클래스를 찾는 폴더나 폴더리스트가 있는 경로를 말합니다.

**CLASSPATH**를 사용하는 방법은

- OS에서 환경변수로 등록해서 사용
- 자바로 실행할 때 옵션으로 `-cp`나 `-classpath` 를 사용
- 자바 애플리케이션 manifest.mf 파일에 Class-Path를 명시해서 JAR 파일에 넣어 사용

<!-- more -->

### Java에서 PATH와 CLASSPATH의 차이

- **PATH** : cli에서 명령어를 실행할 경우 PATH에 등록된 위치에서 검색하여 실행한다.
- **CLASSPATH** : JVM(Java Virtual Machine)에서 사용자가 정의한 클래스를 가져다 사용하기 위한 위치를 기록한 환경 변수이다.

※ 사용하는 방법으로는 다음과 같다.

##### cli 창에서 java 명령어 옵션으로 일회성 사용

 cli창에서는 `java -classpath` 이나 `java -cp` 으로 사용한다.

※ 이 옵션으로 실행할 경우 기존 CLASSPATH를 덮어씌워 사용한다. 그리고 cli로 실행할 경우 CLASSPATH는 실행하는 현재 경로인 "."을 기본 값으로 가지고 있습니다. -jar으로 실행 될 경우 -cp나 -classpath는 무시됩니다. 이때는 애플리케이션에서 META-INF/MANIFEST.MF 파일에 Class-path 속성으로 명시해야합니다.



##### Java 애플리케이션 내 META-INF/MANIFEST.MF 파일에 명시

META-INF/MANIFEST.MF 파일에 Class-Path 속성을 사용해서 위치를 등록할 수 있다.



##### Windows 환경변수로 등록

 윈도우 시스템 설정의 환경변수 창에서 CLASSPATH 이름으로 변수를 등록하고 값으로 Java 애플리케이션에서 사용할 클래스가 들어있는 위치를 입력해주면 된다.

ex) CLASSPATH=$JAVA_HOME\lib

윈도우 dos에서 환경변수 등록 명령어 (wndows xp, windows 2000, windows7, windows8)

```bash
set CLASSPATH=%CLASSPATH%;JAVA_HOME\lib;
```



##### UNIX 또는 Linux에서 환경변수 등록

`.bash_profile` 스크립트 또는 `.bashrc` 다음 한줄을 추가하면 된다.

```bash
export CLASSPATH="리눅스 내 사용할 자바클래스 폴더 위치"

#예시
export CLASSPATH=/home/tester/first:/home/tester/second
```



※ CLASSPATH 내 동일한 명의 클래스가 존재한다면 먼저 찾는 디렉토리 순서로 우선순위를 가지며 찾아서 실행되면 다음 디렉토리에서 동일한 클래스를 찾지 않는다.

---



### 주로 확인되는 CLASSPATH 관련 ERROR

#### ClassNotFoundException

- 자바 프로그램이 특정 클래스를 런타임에서 로드하려고 할 때 CLASSPATH에서 해당 클래스를 못찾아서 발생하는 에러이다.
  - `Class.forname()` 이나 `loadClass()` 에서 발생한다.

#### NoClassDefFoundError

- 특정 클래스가 컴파일 환경에서는 있었지만 런타임 중에는 CLASSPATH에서 사용 못 할때 발생하는 에러이다.
  - 라이브러리엔 있지만 CLASSPATH에는 없는 경우
  - Static initializer failure
  - class not visible to Classloaders in the J2EE env