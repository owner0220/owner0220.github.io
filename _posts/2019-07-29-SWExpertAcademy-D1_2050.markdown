---
title: SW Expert Academy - D1_2050
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



# D1_2050. 알파벳을 숫자로 변환

### 문제:

- 알파벳으로 이루어진 문자열을 입력 받아 각 알파벳을 1부터 26까지의 숫자로 변환하여 출력하라.



### 입력:

알파벳으로 이뤄진 문자열 주어진다. (문자열의 최대길이는 200)



### 출력:

ex) 1 2 3 4 5 6

알파벳을 숫자로 변환한 결과값을 빈 칸을 두고 출력한다.



#### 생각한 로직:

- A~Z 까지 대응하는 숫자를 입력해서 분류한다
- 문자를 유니코드로 바꿔주는 내장 함수 ord() 사용한다.

#### 코딩:

- **딕셔너리로 A~Z 대응하는 숫자를 만든다.**

```python
#import sys
#sys.stdin = open("1.txt",'r')

st = "ABCDEFGHIJKLMNOPQRSTUVWXYZ"
dataset = dict()
for i,item in enumerate(st):
    dataset[item] = i+1

arr = input()
for j in arr:
    print(dataset[j], end=" ")
```

------

- **내장함수 ord() 사용**

```python
#import sys
#sys.stdin = open("1.txt",'r')


arr = input()
for item in arr:
    print(ord(item)-ord("A")+1, end=" ")
```



[출처]: https://www.swexpertacademy.com/