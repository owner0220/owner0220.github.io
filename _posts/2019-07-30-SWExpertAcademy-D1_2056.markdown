---
title: SW Expert Academy - D1_2056
date: 2019-07-30 12:28:00
categories:
 - Algorithm
tag:
 - Swexpert
 - Python
---

> D1_2056. 연월일 달력
>
> 문제:
>
> - 연월일 순으로 구성된 8자리의 날짜가 입력으로 주어진다.
>
>   ![img](https://www.swexpertacademy.com/main/common/fileDownload.do?downloadType=CKEditorImages&fileId=AV5QOksKA1QDFAUq)
>
> 해당 날짜의 유효성을 판단한 후, 날짜가 유효하다면
>
> [그림1] 과 같이 ”YYYY/MM/DD”형식으로 출력하고,
>
> 날짜가 유효하지 않을 경우, -1 을 출력하는 프로그램을 작성하라.
>
> 연월일로 구성된 입력에서 월은 1~12 사이 값을 가져야 하며
>
> 일은 [표1] 과 같이, 1일 ~ 각각의 달에 해당하는 날짜까지의 값을 가질 수 있다.
>
> ![img](https://www.swexpertacademy.com/main/common/fileDownload.do?downloadType=CKEditorImages&fileId=AV5QOw9qA1UDFAUq)
>
> ※ 2월의 경우, 28일인 경우만 고려한다. (윤년은 고려하지 않는다.)

### 입력:

테스트 케이스의 개수  T

각 테스트 케이스가 주어진다.



### 출력:

날짜가 맞으면 "YYYY/MM/DD" 형식으로 출력

아니면 -1 출력



#### 생각한 로직:

- 앞의 4자리는 년도로 아무 숫자나 다 되니까 pass
- 그 다음 2자리는 0~13 사이 숫자이어야 한다.
- 마지막 2자리는 앞의 두 자리로 결정되는 월의 일수 범위 안에 들어 있어야 한다. (배열에 가능한 일자를 넣어두고 그 수를 벗어나면 -1)



#### 코딩:

```python
#import sys
#sys.stdin = open("1.txt",'r')

T = int(input())
for tc in range(1,T+1):
    dayarr = [0,31,28,31,30,31,30,31,31,30,31,30,31]
    num = input()
    year = num[:4]
    month = num[4:6]
    day = num[6:]
    # print(year,month,day)
    if 0<int(month)<13 and 0<int(day)<=dayarr[int(month)]:
        result = year+"/"+month+"/"+day
    else:
        result = -1
    print("#{} {}".format(tc, result))

```



[출처]: https://www.swexpertacademy.com/