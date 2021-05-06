---
title: SW Expert Academy - D1_2070
date: 2019-08-04 12:28:00
categories:
 - ALGORITHM
tag:
 - Swexpert
 - Python
---

> D1_2070. 큰 놈, 작은 놈, 같은 놈
>
> 문제:
>
> - 2개의 수를 입력 받아 크기를 비교하여 등호 또는 부등호를 출력하는 프로그램을 작성하라.

### 입력:

테스트 케이스 개수 T

테스트 케이스(한 줄에 2개의 수를 준다.)  : 여기서 각 수는 0 이상 10000 이하의 정수이다.



### 출력:

ex)

#1 <
#2 =
#3 >



#### 생각한 로직:

- if를 써서 크면 >
- 작으면 <
- 이외에는 =



#### 코딩:

```python
#import sys
#sys.stdin = open("1.txt",'r')

def printResult(A,B):
    if A>B:
        print(">")
    elif A<B:
        print("<")
    else:
        print("=")

T = int(input())
for tc in range(1,T+1):
    A,B = map(int,input().split())
    print("#{}".format(tc),end=" ")
    printResult(A,B)
```



[출처]: https://www.swexpertacademy.com/