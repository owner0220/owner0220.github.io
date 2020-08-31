---
title: SW Expert Academy - D1_2019
date: 2019-07-22 12:28:00
categories:
 - Algorithm
tag:
 - Swexpert
 - Python
 
---

# D1_2019. 더블더블

### 문제:

- 1부터 주어진 횟수까지 2를 곱한 값(들)을 출력하시오.
- 주어질 숫자는 30을 넘지 않는다.



### 입력:

N (N<30)



### 출력:

ex) 1 2 4 8 16 32 64 128 256



#### 생각한 로직:

- 반복문을 주어진 N만큼 회전 할 때마다 결과값을 출력해 준다.



#### 코딩:

```python
N = int(input())
result = 1
for i in range(N+1):
    print(result,end=" ")
    result *= 2

```



[출처]: https://www.swexpertacademy.com/