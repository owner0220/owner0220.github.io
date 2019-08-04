---
title: "SW Export Academy - D1_2029"
layout: post
date: 2019-07-20 12:28
tag:
- algorithm
category: blog
author: insik
description: SW Export Academy - 몫과 나머지 출력하기
---

## SW_Export_Academy_개인학습

- 지속력 있는 학습을 위해 글을 올립니다.
- 모든 출처와 저작권은 [SW Export Academy][출처]에 있습니다.



# D1_2029. 몫과 나머지 출력하기

### 문제:

- 2개의 수 a, b를 입력 받아, a를 b로 나눈 몫과 나머지를 출력하는 프로그램을 작성하라.

### 입력:

테스트 케이스 T

테스트 케이스가 나오는데 한 줄에 2개의 수가 주어진다.

### 출력:

ex) #1 4 1

몫과 나머지를 출력한다.



#### 생각한 로직:

- 문제 그대로 a를 b로 나눈 몫과 나머지를 저장해 출력한다.



#### 코딩:

```python
#import sys
#sys.stdin = open("1.txt",'r')

T = int(input())
for tc in range(1,T+1):
    a,b = map(int,input().split())
    print("#{} {} {}".format(tc,a//b,a%b))
```



[출처]: https://www.swexpertacademy.com/