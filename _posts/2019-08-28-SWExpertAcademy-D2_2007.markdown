---
title: SW Expert Academy - D2_2007
date: 2019-08-28 12:28:00
categories:
 - ALGORITHM
tag:
 - Swexpert
 - Python
---

> D2_2007. 패턴 마디의 길이
>
> 문제:
>
> - 패턴에서 반복되는 부분을 마디라고 부른다. 문자열을 입력 받아 마디의 길이를 출력하는 프로그램을 작성하라.

### 입력:

테스트 케이스 T

(각 문자열의 길이는 30이다. 마디의 최대 길이는 10이다.)



### 출력:

ex)

#1 5
#2 7
#3 6



#### 생각한 로직:

- 마디 길이는 최대 10이니까 마디 길이를 1부터 시작해서 문자열 길이가 끝날 때까지 마디 반복이 맞는지 확인하고 아니면 return -1 맞으면 return 마디 길이



#### 코딩:

```python
def chkptn(st):
    for num in range(1,11):
        for idx in range(len(st)):
            if st[idx] != st[idx%num]:
                break
            if idx == len(st)-1:
                return num
    return -1




T = int(input())

for tc in range(1,T+1):
    st = input()
    result = chkptn(st)
    print("#{} {}".format(tc,result))
```



[출처]: https://www.swexpertacademy.com/