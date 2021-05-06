---
title: SW Expert Academy - D2_2001
date: 2019-08-26 12:28:00
categories:
 - ALGORITHM
tag:
 - Swexpert
---

> D2_2001. 파리 퇴치
>
> 문제:
>
> N x N 배열 안의 숫자는 해당 영역에 존재하는 파리의 개수를 의미한다.
>
> 아래는 N=5 의 예이다.
>
> ![img](https://www.swexpertacademy.com/main/common/fileDownload.do?downloadType=CKEditorImages&fileId=AV5P0m66AkIDFAUq)
>
> M x M 크기의 파리채를 한 번 내리쳐 최대한 많은 파리를 죽이고자 한다.
>
> 죽은 파리의 개수를 구하라!
>
> 예를 들어 M=2 일 경우 위 예제의 정답은 49마리가 된다.
>
>  ![img](https://www.swexpertacademy.com/main/common/fileDownload.do?downloadType=CKEditorImages&fileId=AV5P0reqAkMDFAUq)

### 입력:

1. 테스트 케이스 T
2. 케이스 별로 N, M
3. N줄에 거쳐 N*N 배열이 주어진다.



### 출력:

ex)

#1 5
#2 7
#3 6



#### 생각한 로직:

- N*N 배열을 0~N-M 까지 순회하면서 각 자리에서 0~M-1까지 탐색해 다 더한 값들 중 최대값을 찾는다.
- O(N^2)



#### 코딩:

```python
def findM(arrlist,M,y,x):
    global result
    tmp=0
    for i in range(M):
        for j in range(M):
            tmp+=arrlist[y+i][x+j]
    if tmp > result:
        result = tmp


for tc in range(1,int(input())+1):
    N,M = map(int,input().split())
    arrlist = [list(map(int,input().split())) for x in range(N)]
    result = 0
    #N사이즈의 배열을 순회
    for row in range(N-M+1):
        for col in range(N-M+1):
            #row,col 위치에서 파리채 사이즈 만큼 죽은 파리 수를 확인하는 함수
            findM(arrlist,M,row,col)
    print("#{} {}".format(tc,result))
```



[출처]: https://www.swexpertacademy.com/