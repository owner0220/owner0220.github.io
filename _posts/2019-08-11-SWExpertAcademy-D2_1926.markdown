---
title: SW Expert Academy - D2_1926
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



# D2_1926. 간단한 369게임

### 문제:

- "3" "6" "9"가 들어가 있는 수를 말하지 않는대신, 박수를 친다. 박수는 해당 숫자가 들어간 개수만큼 쳐야 한다. 
  예를 들어 숫자 35의 경우 박수 한 번, 숫자 36의 경우 박수를 두번 쳐야 한다.입력으로 정수 N 이 주어졌을 때, 1~N 까지의 숫자를

  게임 규칙에 맞게 출력하는 프로그램을 작성하라.

  박수를 치는 부분은 숫자 대신, 박수 횟수에 맞게 “-“ 를 출력한다.

  **여기서 주의해야 할 것은 박수 한 번 칠 때는 - 이며, 박수를 두 번 칠 때는 - - 가 아닌 -- 이다**



### 입력:

정수 N (10<=N<=1000)



### 출력:

ex)

1 2 - 4 5 - 7 8 - 10



#### 생각한 로직:

- 숫자로 생각한다면 10으로 나눈 나머지가 3,6,9가 된다면 그만큼 **-**를 출력하게 한다.
- 문자로 바꿔서 3,6,9 수를 세서 그만큼 **-**를 출력한다.



#### 코딩:

- 숫자로 계산

```python
def chk369(num):
    start = num
    cnt = 0
    while num!=0:
        if num%10 == 3 or num%10 == 6 or num%10 == 9:
            cnt+=1
        num//=10
    if cnt == 0:
        print(start, end=" ")
    else:
        print('-'*cnt, end=" ")

N = int(input())
for x in range(1,N+1):
    chk369(x)
```



- 문자로 풀이

```python
def chk369(num):
    cnt = str(num).count('3')+ str(num).count('6') + str(num).count('9')
    if cnt == 0:
        print(num, end = " ")
    else:
        print('-'*cnt, end = " ")

N = int(input())
for x in range(1,N+1):
    chk369(x)
```



- 문자로 풀이2

```python
def chk369(num):
    cnt = str(num).count('3')+ str(num).count('6') + str(num).count('9')
    if cnt == 0:
        return num
    else:
        return '-'*cnt

N = int(input())
for x in range(1,N+1):
    res = chk369(x)
    print(res,end=" ")
```



[출처]: https://www.swexpertacademy.com/