---
title: SW Expert Academy - D1_2071
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



# D1_2071. 평균값 구하기

### 문제:

- 10개의 수를 입력 받아, 평균값을 출력하는 프로그램을 작성하라.

  (소수점 첫째 자리에서 반올림한 정수를 출력한다.)  



### 입력:

테스트 케이스 개수 T

테스트 케이스(한 줄에 10개의 수를 준다.)  : 여기서 각 수는 0 이상 10000 이하의 정수이다.



### 출력:

ex)

#1 24
#2 29
#3 27



#### 생각한 로직:

- 각 수 10개를 배열로 입력을 받아 각 원소합을 구하고 10으로 나눈 값이 평균값
- 소수점 첫째 자리에서 반올림 (**format의 포멧** 사용 or 내장함수 사용 **round** or **수학적으로 0.5 이상이면 +1**)



#### 중요

**※ 파이썬에서 몫을 구하는 // 오퍼레이터는 소수점 버림으로 생각하면된다.**

**※ 반올림 하는 내장 함수는 round()**

**※ `print("{:0.0f}".format(result))`  float 수를 소수점 없이 반올리해서 출력해라**





#### 코딩:

```python
T = int(input())
for tc in range(1,T+1):
    result = 0
    arr = list(map(int,input().split()))
    for item in arr:
        result += item
    result /= 10
    # print(result)
    print("#{} {:0.0f}".format(tc, result))
```



[출처]: https://www.swexpertacademy.com/