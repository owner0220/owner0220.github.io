---
title: "SW Export Academy - D1_2072"
layout: post
date: 2019-07-20 12:28
tag:
- algorithm
- python
category: blog
author: insik
description: SW Export Academy - 홀수만 더하기
---

## SW_Export_Academy_개인학습

- 지속력 있는 학습을 위해 글을 올립니다.
- 모든 출처와 저작권은 [SW Export Academy][출처]에 있습니다.



# D1_2072.홀수만 더하기

### 문제:

- 10개의 수를 입력 받아, 그 중에 홀수만 더한 값을 출력해라.



### 입력:

테스트 케이스 개수 T

테스트 케이스(한 줄에 10개의 수를 준다.)  : 여기서 각 수는 0 이상 10000 이하의 정수이다.



### 출력:

ex)

#1 200
#2 208
#3 121



#### 생각한 로직:

- 입력 받는 테스크 케이스 원소를 리스트에 넣고 결과를 저장할 result 변수 생성
- 리스트안 원소를 하나씩 홀수 인지 확인하면서
- 홀수면 result에 더한다.
- 다 확인하면 그 결과를 출력한다.



#### 코딩:

```python
#import sys
#sys.stdin = open("1.txt",'r')

T = int(input())
for tc in range(1, T+1):
    inputarr = list(map(int,input().split()))
    # print(inputarr)
    result = 0
    for item in inputarr:
        # print(item)
        if item % 2 == 1:
            result += item
    print("#{} {}".format(tc,result))
```



[출처]: https://www.swexpertacademy.com/