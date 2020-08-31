---
title: SW Expert Academy - D1_2027
date: 2019-07-24 12:28:00
categories:
 - Algorithm
tag:
 - Swexpert
 - Python
---

## SW_Expert_Academy_개인학습

- 지속력 있는 학습을 위해 글을 올립니다.
- 모든 출처와 저작권은 [SW Expert Academy][출처]에 있습니다.



# D1_2027. 대각선 출력하기

### 문제:

#++++
+#+++
++#++
+++#+
++++#

- 주어진 텍스트를 그대로 출력하세요.



### 입력:





### 출력:

#++++
+#+++
++#++
+++#+
++++#



#### 생각한 로직:

- 변수로 저장해서 그대로 출력한다.
- for 문으로 출력한다.
  - 출력 문자를 보니 행과 열이 같을 때 # 이 출력된다.



#### 중요:

- 왼쪽 대각선은 행과 열 인덱스가 동일한다.
- 오른쪽 대각선은 len(배열)-idx 성질을 이용하자



#### 코딩:

- **변수 저장, 출력**

```python
t = """#++++
+#+++
++#++
+++#+
++++#"""
print(t)
```

------

- **for문**

```python
for i in range(5):
    for j in range(5):
        if i==j:
            print("#",end="")
        else:
            print("+",end="")
    print()
```



[출처]: https://www.swexpertacademy.com/