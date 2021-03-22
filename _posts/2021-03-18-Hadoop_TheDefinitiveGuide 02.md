---
title: Hadoop | The Definitive Guide 02
date: 2021-03-18 09:29:00
categories:
 - hadoop
tag:
 - hadoop
---

# 1장 하둡 기초

## Part 2. 맵리듀스

맵리듀스는 데이터 처리를 위한 프로그래밍 모델이다. 자바, 루비, 파이썬으로 작성된 동일한 프로그램을 살펴보면서 예제를 통해 맵리듀스의 프로그래밍 모델을 살펴보자

<!-- more -->



### 2.1 기상 데이터셋

기상데이터는 반구조적(semi-structured), 레코드 지향적(record-oriented)이기 때문에 맵리듀스 이용 데이터 분석에 적합하다.

#### 2.1.1 데이터 포맷

예시 데이터는  국립기후자료센터[NCDC, National Climatic Data Center](www.ncdc.noaa.gov) 에서 가져와 사용한다.

- 한행이 하나의 레코드, 행 단위 아스키 형식
- 단순 처리를 위해 고정길이의 기온과 같은 기본 요소에 초점

데이터 예시)

```bash
0057
332130     # USAF 기상관측소 식별자
99999      # WBAN 기상관측소 식별자
19500101   # 관측 날짜
0300       # 관측 시간
4
+51317     # 위도 (위도 * 1000)
+028783    # 경도 (경도 * 1000)
FM-12
+0171      # 고도 (미터)
99999
V020
320        # 바람 방향 (도)
1          # 특성 코드 0,1,4,5,9
N
...
```



국립기후자료센터에서 데이터 파일은 해당 날짜와 기상 관측소 기준으로 분리되어 연도별 디렉터리에 위치한다.

```bash
$ ls raw/1990 | head
010010-99999-1990.gz
010014-99999-1990.gz
010015-99999-1990.gz
010016-99999-1990.gz
010017-99999-1990.gz
010030-99999-1990.gz
```

수만 개의 기상관측소가 존재하기 때문에 전체 데이터셋은 상대적으로 작은 파일이 매우 많다. 하둡의 특성상 소수의 큰 파일이 처리하기 쉽고 효율적이다. 따라서 다수의 파일을 특정 기준으로 병합하기 위해 먼저 전처리 작업을 해야 한다.

### 2.2 유닉스 도구로 데이터 분석하기

**Q. 연도별 전 세계 최고 기온은 얼마일까?**

- **유닉스 도구로만 분석하기**

```bash
#!/usr/bin/env bash
for year in *
do
	echo -ne `basename $year .gz`"\t"
	gunzip -c $year | \
		awk '{temp = substr($0, 88, 5) + 0;
			  q = substr($0, 93, 1);
			  if (temp !=9999 && q ~ /[01459]/ && temp > max) max = temp }
			  END { print max }'
done
```

1) 연도별 파일을 돌면서 해당 연도 출력

2) awk 이용 각 파일 처리 (데이터에서 기온 **temp**, 특성코드 **q** 추출)

3) 기온에 + 0 하여 그 값을 정수형으로 변환

4) 기온이 유효한 값인지(9999는 누락값을 의미), 특성코드로 데이터 신뢰 가능인지 체크

5) 현재 최고 기온과 비교하여 높은 값을 최고 기온으로 변경

6) END영역은 파일의 모든 행이 처리된 후에 실행되는데, 최종 최고기온을 출력

스크립트 실행 결과 예시

```bash
1901 317
1902 244
1903 289
1904 256
1905 283
...
```

##### 생각해볼 점

1. 일을 동일한 크기로 나누는 것이 언제나 쉽고 명확할 수 없다.
   - 전체 입력 파일을 고정길이 데이터 청크로 나누고 각 청크를 하나의 프로세스에 할당
2. 독립적인 프로세스 결과를 모두 합치는데 더 많은 처리가 필요할 수 있다.
3. 단일 머신 처리 능력에 한계가 있다.
   - 여러 대의 머신을 사용할 때는 코디네이션(Coordination,협력과 조정)과 신뢰성 고민이 필요하다.

### 2.3 하둡으로 데이터 분석하기

하둡의 병렬처리를 활용하기 위해 맵리듀스로 다시 표현할 필요가 있다.

#### 2.3.1 맵과 리듀스

맵리듀스 작업은 크게 맵 단계와 리듀스 단계로 구분된다. 각 단계는 입력과 출력으로 키-값의 쌍을 가지며 그 타입은 프로그래머가 선택한다. 맵,리듀스 함수를 각각 프로그래머가 작성해야 한다.

- 맵 단계 (데이터 제공의 역할)
  - 데이터 입력 NCDC 원본 데이터
    - 텍스트 입력 포맷 선택
    - 값은 각 행 (문자열), 키는 파일 시작부에서 각 행이 시작되는 지점까지 오프셋 (여기서 키는 의미가 없으므로 패스)

0067011990999991950051507004...9999999N9+00001+9999999999...
0043011990999991950051512004...9999999N9+00221+9999999999...
0043011990999991950051518004...9999999N9-00111+9999999999...
0043012650999991949032412004...0500001N9+00781+9999999999...

- 각 행에서 연도와 기온을 추출
  - 기온 필드 값이 누락,이상,문제 레코드 제거 작업
  - 각 행은 키-값 쌍으로 변환, 맵 함수의 입력이 된다.

(0,      006701199099999**1950**051507004...9999999N9**+0000**1+999999999....)
(106,  004301199099999**1950**051512004...9999999N9**+0022**1+9999999999...)
(212,  004301199099999**1950**051518004...9999999N9**-0011**1+9999999999...)
(318,  004301265099999**1949**032412004...0500001N9**+0078**1+9999999999...)

맵 함수는 연도와 기온(굵은 문자)을 추출하여 출력으로 보낸다.(기온은 정수형으로 변환된다.)

(1950, 0)

(1950, 22)

(1950, -11)

(1949, 87)



맵리듀스 프레임워크에 의해 맵 함수의 출력이 리듀스 함수 입력으로 전달된다. (git 맵리듀스 interface 구현 코드 참고) 리듀스 함수가 받는 값은 아래와 같다.

(1949, [87])

(1950,[0,22,-11])

연도별로 측정된 모든 기온 값이 하나의 리스트로 묶이며 리듀스 함수는 리스트 전체를 반복하여 최고 측정값을 추출한다.

(1949, 111)

(1950, 22)

#### 2.3.2 자바 맵리듀스

**최고 기온을 구하는 Mapper 예제**

```java
import java.io.IOException;

import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.LongWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Mapper;

public class MaxTemperatureMapper
    extends Mapper<LongWritable, Text, Text, IntWritable> {
    
    private static final int MISSING = 9999;

    @Override
    public void map(LongWritable key, Text value, Context context)
        throws IOException, InterruptedException{

        String line = value.toString();
        String year = line.substring(15,19);
        int airTemperature;
        if(line.charAt(87) == '+') {
            airTemperature = Integer.parseInt(line.substring(88,92));
        }else{
            airTemperature = Integer.parseInt(line.substring(87,92));
        }
        String quality = line.substring(92,93);
        if(airTemperature != MISSING && quality.matches("[01459]")){
            context.write(new Text(year), new IntWritable(airTemperature));
        }
    }
}
```

Mapper 클래스는 제네릭 타입으로, 네 개의 정규 타입 매개변수(입력키, 입력값, 출력키, 출력값)을 갖는다. 최적화된 네트워크 직렬화를 위해 별도로 하둡에서 제공하는 타입 셋을 사용한다.



**최고 기온을 구하는 Reducer 예제**

```java
import java.io.IOException;

import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduece.Reducer;

public class MaxTemperatureReducer
    extends Reducer<Text, IntWritable, Text, IntWritable>{
    @Override
    public void reduce(Text key, Iterable<IntWritable> values, Context context) throws IOException, InterruptedException {
        int maxValue = Integer.MIN_VALUE;
        for (IntWritable value : values){
            maxvalue = Math.max(maxValue, value.get());
        }
        context.write(key, new IntWritable(maxValue));
    }
}
```

리듀스 함수 역시 입력과 출력 타입을 규정하기 위해 네 개의 정규 타입 매개변수 사용. 여기서 리듀스 함수의 입력 타입은 맵 함수의 출력 타입이었던 Text, IntWritable과 짝을 이룬다.



**MapReduce 구동 코드 예제**

```java
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Job;
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;

public class MaxTemperature {
    
    public static void main(String[] args) throws Exception{
        if (args.length != 2) {
            System.err.println("Usage: MaxTemperature <input path> <output path>");
            System.exit(-1);
        }
        
        Job job = new Job();
        job.setJarByClass(MaxTemperature.class);
        job.setJobName("Max temperature");
        
        FileInputFormat.addInputPath(job, new Path(args[0]));
        FileOutputFormat.setOutputPath(job, new Path(args[1]));
        
        job.setMapperClass(MaxTemperatureMapper.class);
        job.setReducerClass(MaxTemperatureReducer.class);
        
        job.setOutputKeyClass(Text.class);
        job.setOutputValueClass(IntWritable.class);
        
        System.exit(job.waitForCompletion(true) ? 0 : 1);
    }
}
```

Job 객체는 잡 명세서를 작성한다. 잡을 수행하는 방법을 정의한 잡 명세서는 사용자가 결정한다. 하둡이 클러스터의 해당 머신에 JAR 파일을 배포한다.  wairForCompletion 메서드는 잡을 제출한 후 잡이 모두 끝날 때까지 기다린다.



**참고**  ※ 실제 위 소스를 사용하기 위해 컴파일과 jar 파일 생성 필요

[ [hadoop MR codeing](https://hadoop.apache.org/docs/current/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html) ]

**환경 설정**

```bash
export JAVA_HOME=/usr/java/default
export PATH=${JAVA_HOME}/bin:${PATH}
export HADOOP_CLASSPATH=${JAVA_HOME}/lib/tools.jar
```

**컴파일과 jar 생성** ( HADOOP_CLASSPATH 에 생성 )

```bash
$ bin/hadoop com.sun.tools.javac.Main MaxTemperature.java
$ jar cf mt.jar Max*.class
```

**입력 파일은 하둡 내 존재해야 하며 결과는 입력 파일 위치와 달라야 한다.**

```bash
# ex) 하둡 내 /user/joe/word/input/file01.txt 위치에 존재
$ bin/hadoop jar mt.jar MaxTemperature /user/joe/word/input/file01.txt /user/joe/word/output/out01.txt
```



**테스트 수행** ( 수행은 하둡 클러스터, 데이터는 로컬 파일시스템에서? )

```bash
$ export HADOOP_CLASSPATH=hadoop-examples.jar
$ hadoop MaxTemperature input/ncdc/sample.txt output
```



### 2.4 분산형으로 확장하기

#### 2.4.1 데이터 흐름

- 맵리듀스 잡 (Job) : 클라이언트가 수행하는 작업의 기본단위

  - 잡은 입력 데이터, 맵리듀스 프로그램, 설정 정보로 구성된다.

  - 하둡은 Job 을

    - 맵 태스크(map task)
    - 리듀스 태스크(reduce task)

    나눠서 실행한다. 각 태스크는 YARN을 이용해 스케줄링되고 클러스터의 여러 노드에서 실행된다. 특정 노드의 태스크 하나가 실패하면 자동으로 다른 노드를 재할당하여 다시 실행된다.

  - 하둡은 맵리듀스 잡의 입력을 입력 스플릿(input split) 또는 단순히 스플릿이라고 부르는 고정 크기 조각으로 분리한다.  각 스플릿 마다 맵 태스크를 생성하고 스플릿의 각 레코드를 사용자 정의 맵 함수로 처리한다.

#### 2.4.2 컴바이너 함수

```java
max(max(0,20,10),max(25,15)) = max(20,25) = 25
```

맵에서 컴바이너 함수를 적용해도 리듀스의 결과는 같다. 되지 않는 경우도 있다.

```java
mean(mean(0,20,10), mean(25,15)) = mean(10,20) = 15
mean(0,20,10,25,15) = 14
```

그럼에도 컴바이너를 사용하는 이유는 매퍼와 리듀서 사이의 셔플 단계에서 전송되는 데이터양을 줄이는 데 큰 도움이 되기 때문이다.

##### 컴바이너 함수 작성하기

```java
public class MaxTemperatureWithCombiner {
    public static void main(String[] args) throws Exception{
        if (args.length != 2){
            System.err.println("Usage: MaxTemperatureWithCombiner <input path> " + "<output path>");
            System.exit(-1);
        }
        
        Job job = new Job();
        job.setJarByClass(MaxTemperatureWithCombiner.class);
        job.setJobName("Max temperature");
        
        FileInputFormat.addInputPath(job, new Path(args[0]));
        FileOutputFormat.setOutputPath(job, new Path(args[1]));
        
        job.setMapperClass(MaxTemperatureMapper.class);
        job.setCombinerClass(MaxTemperatureReducer.class);
        job.setReducerClass(MaxTemperatureReducer.class);
        
        job.setOutputKeyClass(Text.class);
        job.setOutputValueClass(IntWritable.class);
        
        System.exit(job.waitForCompletion(true) ? 0 : 1);
    }
}
```



#### 2.4.3 분산 맵리듀스 잡 실행하기

### 2.5 하둡 스트리밍

하둡 스트리밍은 하둡과 사용자 프로그램 사이의 인터페이스로 유닉스 표준 스트림을 사용한다. 따라서 사용자는 표준 입력을 읽고 표준 출력으로 쓸 수 있는 다양한 언어를 이용해 맵리듀스 프로그램을 작성할 수 있다. 리듀스 함수의 입력은 키-값 쌍과 같은 포맷으로 키를 기준으로 데이터를 정렬하며, 리듀스 함수는 표준으로부터 각 행을 읽고, 그 결과를 표준 출력에 쓴다.

#### 2.5.1 루비

**루비 최고 기온을 찾는 맵 함수**

```ruby
#!/usr/bin/env ruby
STDIN.each_line do |line|
    val = line
    year, temp, q = val[15,4], val[87,5], val[92,1]
    puts "#{year}\t#{temp}" if (temp != "+9999" && q =~ /[01459]/)
end
```

STDIN(전역 상수 타입 IO)에서 각 행을 처리하는 블록을 반복적으로 실행하여 표준 입력으로 받은 모든 행을 처리한다. 각 행에서 필요한 필드를 (year, temp, q) 추출하고, 기온 필드의 값이 유효하면 탭문자(\t)로 구분된 연도와 기온을 표준 출력(puts 사용)에 기록한다.



**루비 최고 기온을 찾는 리듀스 함수**

```ruby
#!/usr/bin/env ruby
last_key, max_val = nil, -1000000
STDIN.each_line do |line|
    key, val = line.split("\t")
    if last_key && last_key != key
        puts "#{last_key}\t#{max_val}"
        last_key, max_val = key, val.to_i
    else
        last_key, max_val = key, [max_val, val.to_i].max
    end
end
puts "#{last_key}\t#{max_val}" if last_key
```



**루비 맵리듀스 실행**

```bash
$ cat input/ncdc/sample.txt | sample/max_temperature_map.rb | sort | sample/max_temperature_reduce.rb
```



**하둡에서 실행**

```bash
$ hadoop jar $HADOOP_HOME/share/hadoop/tools/lib/hadoop-streaming-*.jar
 -input input/ncdc/sample.ext
 -output output
 -mapper sample/max_temperature_map.rb
 -reducer sample/max_temperature_reduce.rb
```



**하둡 내 콤바이너 활용 실행**

```bash
$ hadoop jar $HADOOP_HOME/share/hadoop/tools/lib/hadoop-streaming-*.jar
 -files sample/max_temperature_map.rb, sample/max_temperature_reduce.rb
 -input input/ncdc/all
 -output output
 -mapper sample/max_temperature_map.rb
 -combiner sample/max_temperature_reduce.rb
 -reducer sample/max_temperature_reduce.rb
```

※ 클러스터 스트리밍 프로그램 실행 시 해당 스크립트를 클러스터로 전송하기 위해 -files 옵션 사용



#### 2.5.2 파이썬

**파이썬 최고 기온 찾는 맵 함수**

```python
#!/usr/bin/env python
import re
import sys

for line in sys.stdin:
    val = line.strip()
    (year, temp, q) = (val[15:19], val[87:92], val[92:93])
    if(temp != "+9999" and re.match("[01459]", q)):
        print "%s\t%s" % (year, temp)
```



**파이썬 최고 기온 찾는 리듀스 함수**

```python
#!/usr/bin/env python
import sys

(last_key, max_val) = (None, -sys.maxint)
for line in sys.stdin:
    (key, val) = line.strip().split("\t")
    if last_key and last_key != key:
        print "%s\t%s" % (last_key, max_val)
        (last_key, max_val) = (key, int(val))
    else:
        (last_key, max_val) = (key, max(max_val, int(val)))
        
if last_key:
    print "%s\t%s" % (last_key, max_val)
```



**파이썬 맵리듀스 실행**

```bash
$ cat input/ncdc/sample.txt | sample/max_temperature_map.py | sort | sample.max_temperature_reduce.py
```

