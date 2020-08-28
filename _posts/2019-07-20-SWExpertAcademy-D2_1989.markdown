---
title: SW Expert Academy - D2_1989
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



# D2_1989. 초심자의 회문 검사

### 문제:

- "level" 과 같이 거꾸로 읽어도 제대로 읽은 것과 같은 문장이나 낱말을 회문(回文, palindrome)이라 한다.

  단어를 입력 받아 회문이면 1을 출력하고, 아니라면 0을 출력하는 프로그램을 작성하라.  

### 입력:

1. 테스트 케이스 T
2. 각 테스트 케이스 별로 단어가 나온다. (각 단어의 길이는 3 이상 10 이하이다.)

### 출력:

ex)

#1 5
#2 7
#3 6



#### 생각한 로직:

- 한 단어만 나오니까 처음 인덱스, 끝 인덱스가 같은지 비교하면서 단어의 전체 길이 절반까지만 돌면 회문인지 확인할수 있다.
  - 짝수 일때,
    - ex(길이는 6 ->0,5 / 1,4 / 2,3         길이의 절반에 해당하는 3개만 확인하면 된다.)
  - 홀수 일때,
    - ex(길이는 5 ->0,4 / 1,3 / 2,2         마지막의 비교는 같은 인덱스 위치를 비교할 필요는 없기 때문에 길이의 절반에 해당하는 2개만 확인하면 된다. )



#### 코딩:

```python
def chkPalindrome(palin,tc,n):
    for i in range(n//2):
        if palin[i] != palin[n-1-i]:
            print("#{} {}".format(tc,0))
            return
    print("#{} {}".format(tc,1))

for tc in range(1,int(input())+1):
    palin = input()
    n=len(palin)
    #펠린드롬(회문)을 확인하는 함수가 필요하다. 확인하고 출력까지 시키자.
    chkPalindrome(palin,tc,n)
```



[출처]: https://www.swexpertacademy.com/