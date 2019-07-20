---
title: "SW Export Academy - D1_2025"
layout: post
date: 2019-07-20 12:28
tag:
- algorithm
- python
category: blog
author: insik
description: SW Export Academy - N줄덧셈
---

## SW_Export_Academy_개인학습

- 지속력 있는 학습을 위해 글을 올립니다.
- 모든 출처와 저작권은 [SW Export Academy][출처]에 있습니다.



# D1_2025. N줄덧셈

### 문제:

- 1부터 주어진 숫자만큼 모두 더한 값을 출력하시오.

  단, 주어질 숫자는 10000을 넘지 않는다.  



### 입력:

숫자(<10000)



### 출력:

ex) 58

1부터 주어진 숫자까지 모두 더한 값



#### 생각한 로직:

- **수학정리 활용** : 1부터 주어진 숫자까지 합이니까 등차수열의 합 (가우스 정리) 이용
- for 문 하나씩 다 더한다.



#### 코딩:

- **수학 정리 활용**

```python
N = int(input())
print((N+1)*N//2)
```

------

- **반복문 활용**

```python
N = int(input())
result = 0
for i in range(1,N+1):
    result+=i
print(result)
```



[출처]: https://www.swexpertacademy.com/