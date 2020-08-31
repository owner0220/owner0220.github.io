---
title: SW Expert Academy - D2_1948
date: 2019-08-15 12:28:00
categories:
 - Algorithm
tag:
 - Swexpert
 - Python
---

# D2_1948. 날짜 계산기

### 문제:

- 월 일로 이루어진 날짜를 2개 입력 받아, 두 번째 날짜가 첫 번째 날짜의 며칠째인지 출력하는 프로그램을 작성하라.
- 월은 1 이상 12 이하의 정수이다. 각 달의 마지막 날짜는 다음과 같다.
  - 1/31, 2/28, 3/31, 4/30, 5/31, 6/30, 7/31, 8/31, 9/30, 10/31, 11/30, 12/31
  - 두 번째 날짜가 첫 번째 날짜보다 항상 크게 주어진다.  

### 입력:

1. 테스트 케이스 T
2. 각 테스트 케이스에는 4개의 수가 주어진다. (월 일 형식으로 총 2개의 일자가 주어진다.)



### 출력:

ex)

#1 31
#2 103
#3 161



#### 생각한 로직:

- 월은 12월 까지 있고 각 달에 일자가 모두 주어져 있기때문에
- 배열에 일자를 저장해 놓고 그 일자 만큼만 계산
- 월이 같을 때, 월이 다를 때로 구분해서 풀이



#### 코딩:

```python
month = [0,31,28,31,30,31,30,31,31,30,31,30,31]
for tc in range(1,int(input())+1):
    m1,d1,m2,d2 = map(int,input().split())
    result=0
    if m1==m2:
        result = d2-d1+1
    else:
        result += (month[m1] - d1 + 1)
        for mth in range(m1+1,m2):
            result += month[mth]
        result += d2
    print("#{} {}".format(tc, result))
```



[출처]: https://www.swexpertacademy.com/