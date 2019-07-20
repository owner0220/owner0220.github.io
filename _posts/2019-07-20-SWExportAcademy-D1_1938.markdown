---
title: "SW Export Academy - D1_1938"
layout: post
date: 2019-07-20 12:28
tag:
- algorithm
- python
category: blog
author: insik
description: SW Export Academy - 아주 간단한 계산기
---

## SW_Export_Academy_개인학습

- 지속력 있는 학습을 위해 글을 올립니다.
- 모든 출처와 저작권은 [SW Export Academy][출처]에 있습니다.



# D1_1938. 아주 간단한 계산기

### 문제:

- 두 개의 자연수를 입력받아 사칙연산을 수행하는 프로그램을 작성하라.



### 입력:

a b 두개의 자연수가 빈칸을 두고 주어진다.



### 출력:

사칙연산한 결과를 순서대로 출력한다.

+,-,*,/ 순서로 연산 (나누기 연산에서 소수점 이하의 숫자는 버린다.)



#### 생각한 로직:

- a b를 입력 받아서 계산 결과를 바로 출력한다.
  - 나누기한 소수점 이하의 숫자는 버림으로 파이썬에서 몫을 구하는 오퍼레이션을 사용하면 되겠다.



#### 코딩:

```python
a,b = map(int,input().split())
print(a+b)
print(a-b)
print(a*b)
print(a//b)

```



[출처]: https://www.swexpertacademy.com/