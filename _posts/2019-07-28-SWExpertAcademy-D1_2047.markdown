---
title: SW Expert Academy - D1_2047
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



# D1_2047. 신문 헤드라인

### 문제:

- 문자열의 알파벳 소문자를 모두 대문자로 바꾸는 프로그램을 개발 중이다.
- 입력의 모든 소문자 알파벳을 찾아 대문자로 변환한 다음, 그 결과를 출력하는 프로그램을 작성하라. 



### 입력:

80bytes 이하의 길이를 가진 문자열이 주어진다.



### 출력:

문자열의 소문자 모두를 대문자로 변경한 결과를 출력한다.



#### 생각한 로직:

- 딕셔너리로 소문자와 대문자를 key val로 묶은 데이터 셋을 만들어서 바꾸는 방법
- ord() 내장함수를 사용해서 수학적으로 소문자와 대문자의 관계를 확인하고 바꾼다.
- 소문자를 대문자로 바꿔주는 내장 함수를 사용한다.



#### 코딩:

- **데이터 셋으로 만들어서 바꾸자**

```python
#import sys
#sys.stdin = open("1.txt",'r')


bigchar = "ABCDEFGHIJKLMNOPQRSTUVWXYZ"
smallchar = "abcdefghijklmnopqrstuvwxyz"
dataset = dict()

for idx in range(len(bigchar)):
    dataset[smallchar[idx]] = bigchar[idx]

arr = input()
for item in arr:
    if item in dataset:
        print(dataset[item],end ="")
    else:
        print(item,end="")
```



[출처]: https://www.swexpertacademy.com/