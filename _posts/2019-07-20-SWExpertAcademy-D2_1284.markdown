---
title: SW Expert Academy - D2_1284
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



# D2_1284. 수도 요금 경쟁

### 문제:

- 아름이를 포함하여 총 N명의 사람이 돌 던지기 게임을 하고 있다.

  이 돌 던지기 게임은 앞으로 돌을 던져 원하는 지점에 최대한 가깝게 돌을 던지는 게임이다.

  정확하게 말하면 밀리미터 단위로 -100,000에서 100,000까지의 숫자가 일렬로 써져 있을 때, 사람들은 숫자 100,000이 써져 있는 위치에 서서 최대한 0에 가까운 위치로 돌을 던지려고 한다.

  삼성전자에 입사한 종민이는 회사 근처로 이사를 하게 되었다.

  그런데 집의 위치가 두 수도 회사 A, B 중간에 위치하기에 원하는 수도 회사를 선택할 수 있게 되었는데, 두 회사 중 더 적게 수도 요금을 부담해도 되는 회사를 고르려고 한다.

  종민이가 알아본 결과 두 회사의 수도 요금은 한 달 동안 사용한 수도의 양에 따라 다음과 같이 정해진다.

  A사 : 1리터당 P원의 돈을 내야 한다.

  B사 : 기본 요금이 Q원이고, 월간 사용량이 R리터 이하인 경우 요금은 기본 요금만 청구된다. 하지만 R 리터보다 많은 양을 사용한 경우 초과량에 대해 1리터당 S원의 요금을 더 내야 한다.

  ![img](https://www.swexpertacademy.com/main/common/fileDownload.do?downloadType=CKEditorImages&fileId=AV2cT1EqARsBBASw)

  종민이의 집에서 한 달간 사용하는 수도의 양이 W리터라고 할 때, 요금이 더 저렴한 회사를 골라 그 요금을 출력하는 프로그램을 작성하라.N명의 사람들이 던진 돌이 떨어진 위치를 측정한 자료가 주어질 때, 가장 0에 가깝게 돌이 떨어진 위치와 0 사이의 거리 차이와 몇 명이 그렇게 돌을 던졌는지를 구하는 프로그램을 작성하라.  

### 입력:

1. 테스트 케이스 T
2. 케이스 별로 P,Q,R,S,W   (1 ≤ P, Q, R, S, W ≤ 10000, 자연수) 공백으로 구분

### 출력:

ex)

#1 90
#2 1800



#### 생각한 로직:

- 조건을 잘 구분한다.
- A사 리터당 P원
- B사
  - R이하 기본요금             Q원
  - R초과 리터당 S원   ---> (사용-R)*S원



#### 코딩:

```python
for tc in range(1,int(input())+1):
   P,Q,R,S,W = map(int,input().split())
   result = 0
   A = P*W
   B = 0
   if W <= R:
       B = Q
   else:
       B = Q+(W-R)*S
   if A<B:
       result = A
   else:
       result = B
   print("#{} {}".format(tc,result))

```



[출처]: https://www.swexpertacademy.com/