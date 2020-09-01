---
title: SW Expert Academy - D1_1938
date: 2019-07-21 13:28:00
categories:
 - Algorithm
tag:
 - Swexpert
 - Python 
---

> D1_1938. 아주 간단한 계산기
>
> 문제:
>
> - 두 개의 자연수를 입력받아 사칙연산을 수행하는 프로그램을 작성하라.

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