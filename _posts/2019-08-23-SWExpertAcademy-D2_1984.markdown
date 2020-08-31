---
title: SW Expert Academy - D2_1984
date: 2019-08-23 12:28:00
categories:
 - Algorithm
tag:
 - Swexpert
 - Python
---

## SW_Expert_Academy_개인학습

- 지속력 있는 학습을 위해 글을 올립니다.
- 모든 출처와 저작권은 [SW Expert Academy][출처]에 있습니다.



# D2_1984. 중간 평균값 구하기

### 문제:

- 10개의 수를 입력 받아, 최대 수와 최소 수를 제외한 나머지의 평균값을 출력하는 프로그램을 작성하라.

  (소수점 첫째 자리에서 반올림한 정수를 출력한다.)

### 입력:

1. 테스트 케이스 T   
2. 테스트 케이스의 첫 번째 줄에는 10개의 수가 주어진다. (각 수는 0 이상 10000 이하의 정수이다.)



### 출력:

ex)

#1 5
#2 7
#3 6



#### 생각한 로직:

- 10개의 수를 입력 받으니 받은 수를 하나씩 탐색하면서 합을 구하고 최대와 최소값을 찾아 마지막에 제외해주어 평균값을 출력한다.



#### 코딩:

```python
for tc in range(1,int(input())+1):
    #숫자를 받는다.
    numlist=list(map(int,input().split()))
    result=0
    #받은 숫자마다 리스트 안에서 큰 값과 작은 값을 찾아야 하니 리스트 안 원소로 초기화
    nMax=numlist[0]
    nMin=numlist[0]
    for num in numlist:
        if num > nMax:
            nMax = num
        if num < nMin:
            nMin = num
        result+=num
    result=(result-nMax-nMin)/(len(numlist)-2)
    #python print 포매팅 문법 참고
    print("#{} {:.0f}".format(tc,result))
```



[출처]: https://www.swexpertacademy.com/