---
title: SW Expert Academy - D1_1936
date: 2019-07-20 12:28:00
categories:
 - Algorithm
tag:
 - swexpert
 - python
---

## SW_Expert_Academy_개인학습

- 지속력 있는 학습을 위해 글을 올립니다.
- 모든 출처와 저작권은 [SW Expert Academy][출처]에 있습니다.



# D1_1936. 1대1 가위바위보

### 문제:

- A와 B가 가위바위보를 하였다.
- 가위는 1, 바위는 2, 보는 3으로 표현되며 A와 B가 무엇을 냈는지 입력으로 주어진다.
- A와 B중에 누가 이겼는지 판별해보자. 단, 비기는 경우는 없다.  



### 입력:

A B (빈칸을 사이로 주어진다.)



### 출력:

A가 이기면 A, B가 이기면 B를 출력한다.



#### 생각한 로직:

- 이기는 경우와 지는 경우를 분류하고 특징을 잡아서 문제를 해결한다.
  - 중요한 것은 꼭 모든 경우의 수를 보고 나서 일반화 해야 한다.



#### 코딩:

```python
#이기는 경우
# 1 3  -2
# 2 1  1
# 3 2  1
#지는 경우
# 3 1 2
# 1 2 -1
# 2 3 -1
A,B = map(int,input().split())
if A-B == 1 or A-B == -2:
    print("A")
else:
    print("B")
```



[출처]: https://www.swexpertacademy.com/