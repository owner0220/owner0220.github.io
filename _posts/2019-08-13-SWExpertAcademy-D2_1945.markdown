---
title: SW Expert Academy - D2_1945
date: 2019-08-13 12:28:00
categories:
 - SWEXPERT ACADEMY
tag:
 - Swexpert
 - Python
---

> D2_1945. 간단한 소인수분해
>
> 문제:
>
> 숫자 N은 아래와 같다.
>
> N=2a x 3b x 5c x 7d x 11e
>
> N이 주어질 때 a, b, c, d, e 를 출력하라.
>
> **[제약 사항]**
>
> N은 2 이상 10,000,000 이하이다.  

### 입력:

1. 테스트 케이스 T
2. 각 테스트 케이스에 N



### 출력:

ex)

#1 3 2 2 3 1
#2 6 1 2 3 0
#3 6 4 2 0 1
#4 0 0 2 3 0
#5 0 3 4 0 1
#6 4 4 1 0 0
#7 7 3 0 3 0
#8 0 8 0 0 0
#9 0 0 2 0 0
#10 1 3 3 2 0

#### 생각한 로직:

- 여기에서는 소수가 정해져있다. 2, 3, 5, 7, 11 이 숫자들만 배열에 넣어놓고 나눠서 몫들을 출력하면 될것 같다.



#### 코딩:

```python
decimal = [2,3,5,7,11]

for tc in range(1,int(input())+1):
    N = int(input())
    print("#{}".format(tc), end=" ")
    for i in range(len(decimal)):
        tmp = N
        cnt = 0
        while True:
            ck = cnt
            if tmp%decimal[i] == 0:
                cnt+=1
                tmp = tmp//decimal[i]
            if ck==cnt:
                break
        print(cnt,end=" ")
    print()

```



[출처]: https://www.swexpertacademy.com/