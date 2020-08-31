---
title: "SW Expert Academy - D1_1545"
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





## D1_1545. 거꾸로 출력해 보아요

### 문제:

주어진 숫자부터 0까지 순서대로 찍어보세요

아래는 입력된 숫자가 N일 때 거꾸로 출력하는 예시입니다

![img](https://www.swexpertacademy.com/main/common/fileDownload.do?downloadType=CKEditorImages&fileId=AV2geHu6ABcBBAS0)



### 입력:

N



### 출력:

8 7 6 5 4 3 2 1 0



#### 생각한 로직:

- 입력 받으면 거꾸로 바로 출력



#### 코딩:

```python
N = int(input())

for i in range(N,-1,-1):
    print(i,end=" ")
```

[출처]: https://www.swexpertacademy.com/