---
title: SW Expert Academy - D2_1961
date: 2019-08-18 12:28:00
categories:
 - Algorithm
tag:
 - Swexpert
 - Python
---

# D2_1961. 숫자 배열 회전 

### 문제:

- N x N 행렬이 주어질 때,

  시계 방향으로 90도, 180도, 270도 회전한 모양을 출력하라.  

### 입력:

1. 테스트 케이스 T
2. 각 테스트 케이스에는 N이 주어진다.    (N은 3이상 7이하이다.)
3. 다음 N줄에 N*N 행렬이 주어진다.



### 출력:

ex)

#1
741 987 369
852 654 258
963 321 147
#2
872686 679398 558496
952899 979157 069877
317594 487722 724799
997427 894586 495713
778960 562998 998259
694855 507496 686278



#### 생각한 로직:

- 90도 180도 270도 회전한다는 것은 원래 있던 데이터를 정말 수학적으로 회전한다기 보다
- 불러오는 순서를 마치 회전한 것과 같이 불러 오면 된다.



#### 코딩:

```python
def printRotate(arrlist,N):
        for i in range(N):
            #읽은 한줄을 잠시 저장할 배열들
            r90 = list()
            r180 = list()
            r270 = list()
            for j in range(N):
                #읽어서 저장할 것들
                r90.append(arrlist[N-1-j][i])
                r180.append(arrlist[N-1-i][N-1-j])
                r270.append(arrlist[j][N-1-i])
            #한줄에 연이어서 출력
            for item in r90:
                print(item,end="")
            print(end=" ")
            for item in r180:
                print(item, end="")
            print(end=" ")
            for item in r270:
                print(item, end="")
            print(end=" ")
            print()




for tc in range(1,int(input())+1):
    N=int(input())
    arrlist=[list(map(int,input().split())) for x in range(N)]
    print("#{}".format(tc))
    printRotate(arrlist,N)
    # print(arrlist)
```



[출처]: https://www.swexpertacademy.com/