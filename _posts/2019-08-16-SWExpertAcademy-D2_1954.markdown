---
title: SW Expert Academy - D2_1954
date: 2019-08-16 12:28:00
categories:
 - Algorithm
tag:
 - Swexpert
 - Python
---

> D2_1954. 달팽이 숫자
>
> 문제:
>
> - 달팽이는 1부터 N*N까지의 숫자가 시계방향으로 이루어져 있다.
>
> - 다음과 같이 정수 N을 입력 받아 N크기의 달팽이를 출력하시오.
>
> - **[예제]**
>
>   N이 3일 경우,
>
>   ![img](https://www.swexpertacademy.com/main/common/fileDownload.do?downloadType=CKEditorImages&fileId=AV5PpDX6AQIDFAUq)
>
>   N이 4일 경우,
>
>   ![img](https://www.swexpertacademy.com/main/common/fileDownload.do?downloadType=CKEditorImages&fileId=AV5PpGRqAQQDFAUq)

### 입력:

1. 테스트 케이스 T
2. 각 테스트 케이스 별 N (1 ≤ N ≤ 10)

### 출력:

ex)

#1
1 2 3
8 9 4
7 6 5
#2
1 2 3 4
12 13 14 5
11 16 15 6
10 9 8 7



#### 생각한 로직:

- 달팽이 숫자 리스트를 만들고 출력



#### 코딩:

```python
def makeSnaillist(direction,x,y,N):
    global resultlist
    global cnt
    if cnt==N*N:
        return
    cnt+=1
    resultlist[y][x] = cnt
    dx = [1,0,-1,0]
    dy = [0,1,0,-1]
    X = x + dx[direction]
    Y = y + dy[direction]
    if 0<=X<N and 0<=Y<N and resultlist[Y][X]==0:
            makeSnaillist(direction,X,Y,N)
    else:
        X = x + dx[(direction+1)%4]
        Y = y + dy[(direction+1)%4]
        makeSnaillist((direction+1)%4,X,Y,N)


def printList(listarr):
    for item in listarr:
        print(*item)

for tc in range(1,int(input())+1):
    N=int(input())
    resultlist = [[0]*N for x in range(N)]
    cnt=0
    makeSnaillist(0,0,0,N)
    print("#{}".format(tc))
    printList(resultlist)
```



[출처]: https://www.swexpertacademy.com/