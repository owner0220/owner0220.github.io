# Apache Ambari 2.4 - Stack/Extension/Service Definitions

### 소개 | Introduction

앰버리는 관련 서비스를 스택 정의를 통해 스택으로 제공한다. 앰버리는 설치하기 위한 인터페이스 정의와 관리와 모니터링을 정의해 줄 수 있다. 더불어 3rd party 제품들을 추가할 수 있다.

#### 용어 설명 | Terminology

**Stack** 

서비스 묶음 정의, 스택은 여러 버전이 존재할 수 있고 각 버전은 active/inactive 할 수 있다. Stack="HDP-2.4"

**Extension**

사용자 정의 서비스 묶음 정의, 스택에 추가 될 수 있다. Extension은 여러 개가 있을 수 있다.

**Service**

서비스를 구성하는 컴포넌트를 정의한다. 컴포넌트는 MASTER, SLAVE, CLIENT 와 같은 것을 말한다.

ex) Service = HDFS

**Component**

각각의 컴포넌트는 정의된 라이프 사이클(start, stop, install, etc)을 지킨다.

ex) Service = "HDFS", 이 것은 Components="NameNode(MASTER)","Secondary NameNode(MASTER)","DataNode(SLAVE)", "HDFS Client(CLIENT)"



### Stacks

/var/lib/ambari-server/resources/stacks/HDP/x.x/metainfo.xml

#### Stack Versions

각각의 스택버전에서는 스택에 대한 정보를 담은 metainfo.xml를 필수로 제공해야 한다.

`metainfo.xml`

```bash
<metainfo>
	<versions>
		<active>true</active>
	</versions>
	<extends>2.3</extends>
	<minJdk>1.7</minJdk>
	<maxJdk>1.8</maxJdk>
</metainfo>
```

- versions/active : active 안의 값이 true 일때만 앰버리 설치 화면에서 보인다.
- extends : extends 안의 값의 부모 설정이 그대로 적용된다. (ex 2.3버전 HDP 설정을 따라감)
- minJdk : 스택이 지원하는 최소 JDK를 나타낸다. (앰버리 설치 시 해당 버전보다 낮으면 설치 마법사가 Warning을 준다.)
- maxJdk : 스택이 지원하는 최대 JDK를 나타낸다. (앰버리 설치 시 해당 버전보다 높으면 설치 마법사가 Warning을 준다.)

#### Stack Structure

스택은

빌드 전 : `apache-ambari-2.7.3-src/ambari-server/src/main/resources/stacks`

빌드 후, 설치 후 : `/var/lib/ambari-server/resource/stacks`

위치에 있으며 파일 트리 구조는 다음과 같다.

```bash
stacks     
  └── HDP
       └── 2.3
            ├── configuration
            │   └── cluster-env.xml
            ├── hooks
            ├── *metainfo.xml
            ├── properties
            │   ├── stack_features.json
            │   └── stack_tools.json
            ├── *repos
            │   └── repoinfo.xml
            └── services     
```

여기서 *는 필수 구성 이다.

#### Stack Properties

스택 설정과 비슷하게, 모든 설정은 서비스 레벨에서 정의되어 있어야 한다. 하지만 전역 설정으로 스택 버전 레벨에서 전체 서비스에 사용하는 설정을 할 수도 있다. 

한 예로 stack-selector와 conf-selector 에는 세부적인 이름이나 지원되는 스택 버전 정보가 들어 있다. 이런 설정에는 JSON 포맷으로 스택에 대해 적혀 있다.

stack/HDP/2.3/properties 안에 존재

`stack_packages.json`

```json
{
    "HDP": {
        "stack-select":{},
        "conf-select":{},
    	"conf-select-patching":{},
        "upgrade-dependencies":{}
    }
}
```

`stack_features.json`

```json
{
    "HDP": {
        "stack_features": [
            {
                "name": "snappy",
                "description": "snappy compressor/decompressor support",
                "min_version": "2.0.0.0",
                "max_version": "2.2.0.0"
            },
            {
                "name": "express_upgrade",
        		"description": "Express upgrade support",
		        "min_version": "2.1.0.0"
            },
            ...
            
        ]
    }
}
```

`stack_tools.json`

```json
{
    "HDP": {
        "stack_selector": [
            "hdp-select",
            "/usr/bin/hdp-select",
            "hdp-select"
        ],
        "conf_selector": [
            "conf-select",
            "/usr/bin/conf-select",
            "conf-select"
        ]
    }
}
```

이러한 정보들은 반드시 베이스 스택 버전에 명시가 되어 만들어져 있어야 한다.  베이스 스택 버전은 상속 받을 경우 정의했던 세팅들을 덮어쓰지 않도록 반드시 주의한다.

##### Stack Features (스택기능/특징)

스택은 버전에 따라서 지원하는게 달라 질 수 있다. 예를 들어 upgrading support, NFS support, support for specific new components such as Ranger, Phoenix

여기서 스택은 설정을 관리하는데 `configuration/cluster-env.xml` 같이 configuration 폴더 안에 넣고 관리한다.

ex) `.HDP/2.0.6/configuration` 

- core-site.xml, global.xml, hadoop-policy.xml, hdfs-site.xml

`cluster-env.xml`

```xml
<property>
	<name>stack_features</name>
    <value/>
    <description>List of features supported by the stack</description>
    <property-type>VALUE_FROM_PROPERTY_FILE</property-type>
    <value-attributes>
        <property-file-name>stack_features.json</property-file-name>
        <property-file-type>json</property-file-type>
        <read-only>true</read-only>
        <overridable>false</overridable>
        <visible>false</visible>
    </value-attributes>
    <on-ambari-upgrade add="true"/>
</property>
```

위에서 정의한 stack_features.json 파일은 `properties/stack_features.json` 같이 properties 폴더 안에 넣고 관리한다.

ex) `./HDP/2.0.6/properties`

- stack_features.json, stack_packages.json, stack_tools.json

```json
{
    "stack_features": [
        {
            "name": "snappy",
            "description": "Snappy compressor/decompressor support",
            "min_version": "2.0.0.0",
            "max_version": "2.2.0.0"
        },
        {
            "name": "express_upgrade",
            "description": "Express upgrade support",
            "min_version": "2.1.0.0"
        }
    ]
}
```

※ 여기서 min_version, max_version은 선택 제한 조건이다.



##### 		Stack Tools

#### 	Stack Inheritance

##### 		Services Folder Inheritance

### Extensions

#### 	Extension Structure

#### 	Extension Inheritance

#### 	Supported Stack Versions

#### 	Extension Links

### Services

#### 	Service Overview

#### 	Metainfo

##### 		Structure

#### 	Components and Commands

#### 	Service Scripts

#### 	Creating Custom Services

#### 	Implementing Custom Services

#### 	Adding Configuration Settings to a Custom Service

#### 	Service Advisor

#### 		Service Advisor Inheritance

#### 		Service Advisor Behavior

#### 		Service Advisor Examples

#### 	Service Upgrade

#### 		Service Upgrade Packs

#### 		Matching Upgrade Packs

#### 		Upgrade XML Format

#### 	Service Role Command Order

#### 		Role Command Order Sections

#### 		Commands

#### 	Alerts

#### 		Common Properties

#### 		Types

#### 		Structures & Concepts

#### 	Kerberos

#### 		The Kerberos Descriptor

#### 		Descriptor Specifications

#### 		Examples

#### 	Metrics

#### 		Metrics Structure

#### 		Example

#### 	Quick Links

#### 		Declaring Quick Links

#### 	Widgets

#### 		Graph Widget

#### 		Gauge Widget

#### 		Number Widget 

#### 		Template Widget

#### 		Aggregation

#### 		Example Graph Widget

#### 		Widget Definition

#### 	Service Inheritance

#### 		Service Metainfo Inheritance

#### Packing Custom Services

#### 	Management Packs

#### 	mpack.json Format

#### 	Extension Management Packs Structure

#### Installing Management Packs

#### Verifying the Extension Installation

#### Linking Extensions to the Stack