---
title: SW Expert Academy - D2_1986
date: 2019-08-24 12:28:00
categories:
 - ALGORITHM
tag:
 - Swexpert
 - Python
---

> D2_1986. 지그재그 숫자
>
> 문제:
>
> - 1부터 N까지의 숫자에서 홀수는 더하고 짝수는 뺐을 때 최종 누적된 값을 구해보자.
>
> - **[예제 풀이]**
>
>   N이 5일 경우,
>
>   1 – 2 + 3 – 4 + 5 = 3
>
>   N이 6일 경우,
>
>   1 – 2 + 3 – 4 + 5 – 6 = -3  

### 입력:

1. 테스트 케이스 T
2. N (1 ≤ N ≤ 10)

### 출력:

ex)

#1 5
#2 7
#3 6



#### 생각한 로직:

- N까지 for 문으로 돌리면서 홀수, 짝수를 판단 홀수는 더하고 짝수는 빼는 코딩 (D1~D3까지의 문제는 최적화보다는 노가다를 해보라는 문제들이니 그냥 다 빠짐없이 잘 돌려보자)
- 가우스의 덧셈 공식을 이용해서 수학 문제로 풀수도 있다.



#### 코딩:

```python
for tc in range(1,int(input())+1):
    N=int(input())
    result=0
    for i in range(1,N+1):
        if i%2==0:
            result-=i
        else:
            result+=i

    print("#{} {}".format(tc,result))
```



[출처]: https://www.swexpertacademy.com/