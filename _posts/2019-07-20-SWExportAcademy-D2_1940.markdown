---
title: "SW Export Academy - D2_1940"
layout: post
date: 2019-07-20 12:28
tag:
- algorithm
- python
category: blog
author: insik
description: SW Export Academy - 가랏! RC카!
---

## SW_Export_Academy_개인학습

- 지속력 있는 학습을 위해 글을 올립니다.
- 모든 출처와 저작권은 [SW Export Academy][출처]에 있습니다.



# D2_1940. 가랏! RC카!

### 문제:

- **RC (Radio Control)** 카의 이동거리를 계산하려고 한다.

  입력으로 매 초마다 아래와 같은 command 가 정수로 주어진다.

  0 : 현재 속도 유지.
  1 : 가속
  2 : 감속

  위 command 중, **가속(1)** 또는 **감속(2)** 의 경우 가속도의 값이 추가로 주어진다.

  가속도의 단위는, **m/s2** 이며, 모두 양의 정수로 주어진다.

  입력으로 주어진 **N** 개의 command 를 모두 수행했을 때, **N 초 동안 이동한 거리를 계산하는 프로그램을 작성하라.**

  RC 카의 초기 속도는 0 m/s 이다.  

  [예제]

  아래 예제 입력에서 정답은 3 이 된다.

  입력         시간     RC 카의 속도 RC     카의 이동거리
  1 2          1 sec          2 m/s                    2 m
  2 1          2 sec          1 m/s                    3 m

  [제약사항]

1. N은 2이상 30이하의 정수이다. (2 ≤ N ≤ 30)
2. 가속도의 값은 1 m/s2 혹은 2 m/s2 이다.
3. 현재 속도보다 감속할 속도가 더 클 경우, 속도는 0 m/s 가 된다.

### 입력:

1. 테스트 케이스 T
2. Command수 N
3. 각각의 커맨드



### 출력:

ex)

#1 3
#2 4

#### 생각한 로직:

- 이동거리는 커멘드가 실행될 때마다 1초*(커맨드 결과 반영 속도)가 더해지는 것
- 현재 속도를 저장해줄 변수와 총 이동거리를 저장할 변수 필요 가속도는 속도에만 더해준다.



#### 코딩:

```python
commendlist = [0,1,-1]
for tc in range(1,int(input())+1):
    N = int(input())
    movelen = 0
    velocity = 0
    for i in range(N):
        commend = list(map(int,input().split()))
        cmd = commend[0]
        v = 0
        if len(commend) != 1:
            v = commend[1]
        velocity+=v*commendlist[cmd]
        if velocity<0:
            velocity=0
        movelen+=velocity
    print("#{} {}".format(tc,movelen))

```



[출처]: https://www.swexpertacademy.com/