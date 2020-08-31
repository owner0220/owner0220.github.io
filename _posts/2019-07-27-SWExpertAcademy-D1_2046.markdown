---
title: SW Expert Academy - D1_2046
date: 2019-07-27 12:28:00
categories:
 - Algorithm
tag:
 - Swexpert
 - Python
---

# D1_2043. 서랍의 비밀번호

### 문제:

- 비밀번호 P는 000부터 999까지 번호 중의 하나이다.

- 주어지는 번호 K부터 1씩 증가하며 비밀번호를 확인해 볼 생각이다.

- 예를 들어 비밀번호 P가 123 이고 주어지는 번호 K가 100 일 때, 100부터 123까지 24번 확인하여 비밀번호를 맞출 수 있다.

  P와 K가 주어지면 K부터 시작하여 몇 번 만에 P를 맞출 수 있는지 알아보자.  



### 입력:

P K



### 출력:

몇 번 만에 비밀번호를 맞출 수 있는지 출력한다.



#### 생각한 로직:

- 주어지는 번호 K 부터 1씩 증가해서 비밀번호를 확인하니까 시작하는 번호 K부터 비밀번호가 맞는 끝 번호 P까지 횟수는 수학문제
  - P - K +1 (1을 더해주는 이유는 K부터 시작하니까 포함시켜주기 위해서 이다.)
- 반복문을 실제로 돌려보면서 값을 구해본다.



#### 코딩:

- P - K +1

```python
#import sys
#sys.stdin = open("1.txt",'r')

P,K = map(int,input().split())
print(P-K+1)
```

------

- 반복문 실제로 돌린다.

```python
#import sys
#sys.stdin = open("1.txt",'r')

P,K = map(int,input().split())
cnt = 0
while True:
    cnt += 1
    if P == K:
        print(cnt)
        break
    K += 1
```



[출처]: https://www.swexpertacademy.com/