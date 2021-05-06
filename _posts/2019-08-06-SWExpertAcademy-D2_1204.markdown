---
title: SW Expert Academy - D2_1204
date: 2019-08-06 12:28:00
categories:
 - ALGORITHM
tag:
 - Swexpert
---

>  D2_1204. 최빈수 구하기
>
> 문제:
>
> 어느 고등학교에서 실시한 1000명의 수학 성적을 토대로 통계 자료를 만들려고 한다.
>
> 이때, 이 학교에서는 최빈수를 이용하여 학생들의 평균 수준을 짐작하는데, 여기서 최빈수는 특정 자료에서 가장 여러 번 나타나는 값을 의미한다.
>
> 다음과 같은 수 분포가 있으면,
>
> 10, 8, 7, 2, 2, 4, 8, 8, 8, 9, 5, 5, 3
>
> 최빈수는 8이 된다.
>
> 최빈수를 출력하는 프로그램을 작성하여라 (단, 최빈수가 여러 개 일 때에는 가장 큰 점수를 출력하라).
>
> **[제약 사항]**
>
> 학생의 수는 1000명이며, 각 학생의 점수는 0점 이상 100점 이하의 값이다.  

### 입력:

1. 테스트 케이스 T
2. 테스트 케이스 번호
3. 점수

### 출력:

ex)

#1 71
#2 76



#### 생각한 로직:

- 받은 값을 정렬하고 뒤에서 부터 탐색을하는데 카운트 값이 클때마다 그녀석을 최대 값으로 설정한다.



#### 코딩:

```python
for tc in range(1,int(input())+1):
    T = int(input())
    arrlist = list(map(int,input().split()))
    arrlist.sort()
    bCnt = 0
    bRes = 0
    cnt = 0
    r = 0
    for i in range(len(arrlist)-1,0,-1):
        if arrlist[i]==arrlist[i-1]:
            cnt+=1
            r=arrlist[i]

        else:
            if cnt > bCnt:
                bCnt = cnt
                bRes = r
            cnt=0

    print("#{} {}".format(tc,bRes))

```



[출처]: https://www.swexpertacademy.com/