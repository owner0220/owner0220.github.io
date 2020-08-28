---
title: SW Expert Academy - D1_2068
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



# D1_2068. 최대수 구하기

### 문제:

- 10개의 수를 입력 받아, 그 중에서 가장 큰 수를 출력하는 프로그램을 작성하라.



### 입력:

테스트 케이스 수 T

테스트 케이스 숫자 10개 씩 케이스 T줄 만큼 주어진다.



### 출력:

가장 큰수 출력

ex)

#1 99

#2 123

#3 76





#### 생각한 로직:

- 입력 받는 테스크 케이스 원소를 리스트에 넣는다.
- 배열 원소들을 하나씩 비교하면서 최대 값을 찾는다.



#### 중요

- **최대값이나 최소값을 찾는 경우 비교하는 변수를 배열 원소 안의 값 중에서 시작**해야 음수,양수 크기에 상관없이 잘 동작한다.



#### 코딩:

```python
#import sys
#sys.stdin = open("1.txt",'r')

T = int(input())
for tc in range(1,T+1):
    arr = list(map(int,input().split()))
    result = arr[0]
    for item in arr:
        if item > result:
            result = item
    print("#{} {}".format(tc,result))


```



[출처]: https://www.swexpertacademy.com/