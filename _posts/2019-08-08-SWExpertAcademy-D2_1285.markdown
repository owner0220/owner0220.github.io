---
title: SW Expert Academy - D2_1285
date: 2019-08-08 12:28:00
categories:
 - Algorithm
tag:
 - Swexpert
 - Python
---

> D2_1285. 아름이의 돌 던지기
>
> 문제:
>
> 아름이를 포함하여 총 N명의 사람이 돌 던지기 게임을 하고 있다.
>
> 이 돌 던지기 게임은 앞으로 돌을 던져 원하는 지점에 최대한 가깝게 돌을 던지는 게임이다.
>
> 정확하게 말하면 밀리미터 단위로 -100,000에서 100,000까지의 숫자가 일렬로 써져 있을 때, 사람들은 숫자 100,000이 써져 있는 위치에 서서 최대한 0에 가까운 위치로 돌을 던지려고 한다.
>
> N명의 사람들이 던진 돌이 떨어진 위치를 측정한 자료가 주어질 때, 가장 0에 가깝게 돌이 떨어진 위치와 0 사이의 거리 차이와 몇 명이 그렇게 돌을 던졌는지를 구하는 프로그램을 작성하라.  

### 입력:

1. 테스트 케이스 T
2. 돌을 던지는 사람 수 N(1<=N<=1000)
3. 돌이 떨어진 위치를 나타내는 N개 정수 공백으로 구분되어 주어진다. (-100,000<= 돌 떨어진 위치 <= 100000)

### 출력:

ex)

#1 100 2
#2 1 1



#### 생각한 로직:

- 가깝게 던진 거리와 그렇게 던진 사람이 몇명인지 나타내는 정수
- 좌표에서 표현되는 양수 음수에 상관없이 절대값 크기가 곧 거리와 같고 그러한 사람이 몇명인지만 세면 되겠다.



#### 코딩:

```python
for tc in range(1,int(input())+1):
    N = int(input())
    arrlist=list(map(int,input().split()))
    # print(arrlist)
    # 거리가 제일 작은 값을 찾고 그것을 센다.
    length = abs(arrlist[0])
    result = 0
    for item in arrlist:
        if length > abs(item):
            length = abs(item)
    result += arrlist.count(length)
    result += arrlist.count(-1*length)
    print("#{} {} {}".format(tc,length,result))
```



[출처]: https://www.swexpertacademy.com/