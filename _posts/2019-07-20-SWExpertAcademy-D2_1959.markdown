---
title: SW Expert Academy - D2_1959
date: 2019-07-20 12:28:00
categories:
 - Algorithm
tag:
 - swexpert
 - python
---

## SW_Expert_Academy_개인학습

- 지속력 있는 학습을 위해 글을 올립니다.
- 모든 출처와 저작권은 [SW Expert Academy][출처]에 있습니다.



# D2_1959. 두 개의 숫자열

### 문제:

- N 개의 숫자로 구성된 숫자열 Ai (i=1~N) 와 M 개의 숫자로 구성된 숫자열 Bj(j=1~M) 가 있다.

  아래는 N =3 인 Ai 와 M = 5 인 Bj 의 예이다.

  ![img](https://www.swexpertacademy.com/main/common/fileDownload.do?downloadType=CKEditorImages&fileId=AV5PqPTKAUEDFAUq)
  Ai 나 Bj 를 자유롭게 움직여서 숫자들이 서로 마주보는 위치를 변경할 수 있다.

  단, 더 긴 쪽의 양끝을 벗어나서는 안 된다.

  ![img](https://www.swexpertacademy.com/main/common/fileDownload.do?downloadType=CKEditorImages&fileId=AV5PqULaAUIDFAUq)

  서로 마주보는 숫자들을 곱한 뒤 모두 더할 때 최댓값을 구하라.

  위 예제의 정답은 아래와 같이 30 이 된다.

   

  ![img](https://www.swexpertacademy.com/main/common/fileDownload.do?downloadType=CKEditorImages&fileId=AV5PqbLKAUcDFAUq)

### 입력:

1. 테스트 케이스 T
2. 각 테스트 케이스 별 N,M (N 과 M은 3 이상 20 이하이다.)
3. Ai
4. Bj

### 출력:

ex)

#1 30
#2 63



#### 생각한 로직:

- 이중 포문을 활용해서 바깥쪽 하나는 큰 녀석에서 작은 녀석 비교 시작할 첫번째 위치를 선택
- 안쪽은 작은녀석 다 돌아서 계산



#### 코딩:

```python
def findRes(Ai,Bi,N,M):
    global result
    if N<M:
        for lg in range(M-N+1):
            tmp=0
            for dd in range(N):
                tmp+=Ai[dd]*Bi[lg+dd]
            if tmp > result:
                result=tmp
    else:
        for lg in range(N-M+1):
            tmp=0
            for dd in range(M):
                tmp+=Ai[lg+dd]*Bi[dd]
            if tmp > result:
                result=tmp


for tc in range(1,int(input())+1):
    N,M = map(int,input().split())
    Ai = list(map(int,input().split()))
    Bi = list(map(int,input().split()))
    result = 0
    if N<M:
        for i in range(N):
            result+=Ai[i]*Bi[i]
    else:
        for i in range(M):
            result+=Ai[i]*Bi[i]
    findRes(Ai,Bi,N,M)
    print("#{} {}".format(tc,result))
```



[출처]: https://www.swexpertacademy.com/