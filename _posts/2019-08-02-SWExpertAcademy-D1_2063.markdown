---
title: SW Expert Academy - D1_2063
date: 2019-08-02 12:28:00
categories:
 - ALGORITHM
tag:
 - Swexpert
 - Python
---

> D1_2063. 중간값 찾기
>
> 문제:
>
> - 중간 값은 통계 집단의 수치를 크기 순으로 배열 했을 때 전체의 중앙에 위치하는 수치를 말한다.
> - 입력으로 N개의 점수가 주어졌을 때 중간값을 출력해라.

### 입력:

9<=N (항상 홀수)<=199

N개의 점수가 주어진다.



### 출력:

ex) 58

중간값에 해당하는 점수



#### 생각한 로직:

- 입력 받는 테스크 케이스 원소를 리스트에 넣는다.
- 크기 순으로 재배열 한다.
- 재배열된 배열의 인덱스가 (배열크기 - 1)//2 인 위치의 값을 출력한다.



#### 코딩:

- **정렬 내장함수 사용**

```python
#import sys
#sys.stdin = open("1.txt",'r')

N = int(input())
score = list(map(int,input().split()))

score.sort()
print(score[N//2])
```



- **버블정렬 구현**

```python
# import sys
# sys.stdin = open("1.txt",'r')

def bubble(score):
    for i in range(len(score)):
        for j in range(i,len(score)):
            if score[i] > score[j]:
                tmp = score[j]
                score[j] = score[i]
                score[i] = tmp

N = int(input())

score = list(map(int,input().split()))
bubble(score)
print(score[N//2])
```



[출처]: https://www.swexpertacademy.com/