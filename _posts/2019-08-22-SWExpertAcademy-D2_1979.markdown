---
title: SW Expert Academy - D2_1979
date: 2019-07-20 12:28:00
categories:
 - Algorithm
tag:
 - Swexpert
 - Python
---

## SW_Expert_Academy_개인학습

- 지속력 있는 학습을 위해 글을 올립니다.
- 모든 출처와 저작권은 [SW Expert Academy][출처]에 있습니다.



# D2_1979. 어디에 단어가 들어갈 수 있을까

### 문제:

- N X N 크기의 단어 퍼즐을 만들려고 한다. 입력으로 단어 퍼즐의 모양이 주어진다.

  주어진 퍼즐 모양에서 특정 길이 K를 갖는 단어가 들어갈 수 있는 자리의 수를 출력하는 프로그램을 작성하라.

  **[예제]**

  N = 5, K = 3 이고, 퍼즐의 모양이 아래 그림과 같이 주어졌을 때

  ![img](https://www.swexpertacademy.com/main/common/fileDownload.do?downloadType=CKEditorImages&fileId=AV5PuqX6AawDFAUq)

  길이가 3 인 단어가 들어갈 수 있는 자리는 2 곳(가로 1번, 가로 4번)이 된다.

   

  ![img](https://www.swexpertacademy.com/main/common/fileDownload.do?downloadType=CKEditorImages&fileId=AV5Puv2aAa4DFAUq)

### 입력:

1. 테스트 케이스 T
2. 테스트 케이스마다 퍼즐의 가로,세로 길이N, 단어의 길이 K   (5<=N<=15)   (2<=K<=N)
3. 테스트 케이스마다 N줄의 퍼즐 모양이 2차원정보로 주어진다.(흰색 1, 검정색 0)



### 출력:

ex)

#1 5
#2 7
#3 6



#### 생각한 로직:

- 2차원 배열 빠짐없이 탐색 문제
- 흰색인 부분이 연속 될때 카운트해서 검정색이나 벽을 만나는 경우 끝이날때 단어의 길이와 일치하면 +1
- 이 방법을 가로 탐색, 세로 탐색 나눠서 실행



#### 코딩:

```python
def chk(puzzle,N,K):
    global result
    for i in range(N):
        gcnt=0
        scnt=0
        # print(N,K,result)
        for j in range(N):
            #가로 체크
            if puzzle[i][j]==1:
                gcnt+=1
            else:
                if gcnt==K:
                    result+=1
                gcnt=0


            #세로 체크
            if puzzle[j][i]==1:
                scnt+=1
            else:
                if scnt==K:
                    result+=1
                scnt = 0
        #체크를 다하고 지금 벽을 만났을 때 확인 
        if j==N-1:
            if gcnt == K:
                result += 1
            if scnt == K:
                result += 1



for tc in range(1,int(input())+1):
    N,K = map(int,input().split())
    puzzle=[list(map(int,input().split())) for x in range(N)]
    result=0
    chk(puzzle,N,K)
    # print(puzzle)
    # 가로로 확인
    # 세로로 확인
    print("#{} {}".format(tc,result))
```



[출처]: https://www.swexpertacademy.com/