---
title: SW Expert Academy - D2_1859
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



# D2_1859. 백만 장자 프로젝트

### 문제:

- 25년 간의 수행 끝에 원재는 미래를 보는 능력을 갖게 되었다. 이 능력으로 원재는 사재기를 하려고 한다.

  다만 당국의 감시가 심해 한 번에 많은 양을 사재기 할 수 없다.

  다음과 같은 조건 하에서 사재기를 하여 최대한의 이득을 얻도록 도와주자.

  ​    \1. 원재는 연속된 N일 동안의 물건의 매매가를 예측하여 알고 있다.
  ​    \2. 당국의 감시망에 걸리지 않기 위해 하루에 최대 1만큼 구입할 수 있다.
  ​    \3. 판매는 얼마든지 할 수 있다.

  예를 들어 3일 동안의 매매가가 1, 2, 3 이라면 처음 두 날에 원료를 구매하여 마지막 날에 팔면 3의 이익을 얻을 수 있다.  

### 입력:

테스트 케이스 수 T

테스트 케이스 별 N(2<=N<=1,000,000)

각 날의 매매가를 나타내는 N개의 자연수 들이 공백으로 구분되어 나타난다.(매매가<=10,000)



### 출력:

ex) 최대 이익 출력

#1 0
#2 10
#3 5

#### 생각한 로직:

- 최대 이익을 내야 하니까 도표처럼 생각을 해서 지금 수보다 더 큰수를 찾을때까지 뒤로 탐색 찾으면 그 차이를 더한다. 이것을 모두 반복해야 하니까 n^2의 시간이 걸릴듯  (그래서 fail runtime error 30)
- 패턴으로 생각해보니 최대값을 찾고 그 전까지 다 더하고 그 다음 피크를 찾고 그 전까지 찾고 더해서 리턴



##### 다른사람 풀이:

- 사는건 상관 없지만 파는건 최대 값이 나오면 팔아야 함으로 뒤에서 부터 파는게 쉽다.



#### 코딩:

- **재귀함수**

```python
def findpeek(arr,st,end):
    if st == end:
        return 0
    result = 0
    mx = max(arr[st:end])
    # print(mx,result)
    for idx in range(st,end):
        # print(idx)
        if arr[idx] == mx:
            # print(mx, result)
            return result+findpik(arr,idx+1,end)
        result += mx - arr[idx]

T = int(input())
for tc in range(1,T+1):
    N = int(input())
    arr = list(map(int,input().split()))
    print("#{} {}".format(tc,findpeek(arr,0,N)))
```



- 다른사람 풀이

```python
T = int(input())
for tc in range(1, T+1):
    N = int(input())
    arr = list(map(int, input().split()))
    money = list(reversed(arr))
    maxprice = money[0]
 
    res = 0
    for i in range(N):
        if money[i] < maxprice:
            res += maxprice - money[i]
        else:
            maxprice = money[i]
 
    print("#{} {}".format(tc, res))
```



[출처]: https://www.swexpertacademy.com/