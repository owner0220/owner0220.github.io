---
title: SW Export Academy - D1_1933
date: 2019-08-30 12:28:00
categories:
 - Algorithm
tag:
 - Swexpert
 - Python
---

## SW_Export_Academy_개인학습

- 지속력 있는 학습을 위해 글을 올립니다.
- 모든 출처와 저작권은 [SW Export Academy][출처]에 있습니다.



# D1_1933. 간단한 N 의 약수

### 문제:

- 입력으로 1개의 정수 N 이 주어진다.
- 정수 N 의 약수를 오름차순으로 출력하는 프로그램을 작성하라



### 입력:

정수  N (1<=N<=1000)



### 출력:

정수 N의 약수를 오름차순으로 출력



#### 생각한 로직:

- 1부터 시작해서 N까지 숫자가 커지면서 딱 나눠 떨어지면 출력



#### 코딩:

```python
N = int(input())

for i in range(1,N+1):
    if N%i == 0:
        print(i, end=" ")
```



[출처]: https://www.swexpertacademy.com/