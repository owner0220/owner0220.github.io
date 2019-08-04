---
title: "SW Export Academy - D1_2058"
layout: post
date: 2019-07-20 12:28
tag:
- algorithm
category: blog
author: insik
description: SW Export Academy - 자릿수 더하기
---

## SW_Export_Academy_개인학습

- 지속력 있는 학습을 위해 글을 올립니다.
- 모든 출처와 저작권은 [SW Export Academy][출처]에 있습니다.



# D1_2058. 자릿수 더하기

### 문제:

- 하나의 자연수를 입력 받아 각 자릿수의 합을 계산하는 프로그램을 작성하라.



### 입력:

자연수 N (1<=N<=9999 인 자연수)



### 출력:

각 자리수 합 출력

ex) 30



#### 생각한 로직:

- 각 자리수는 10으로 나눈 나머지를 구하면 일의 자리 부터 하나씩 구할 수 있다.
- 10으로 나눈 나머지를 계속 더하고 몫을 다시 10으로 나눠 나머지를 더하는 과정을 몫이 0이 될때까지 반복한다.



#### 코딩:

```python
#import sys
#sys.stdin = open("1.txt",'r')

N = int(input())
result = 0
while N != 0 :
    result += N%10
    N//=10
print(result)
```



[출처]: https://www.swexpertacademy.com/