---
title: SW Expert Academy - D2_1946
date: 2019-08-14 12:28:00
categories:
 - SWEXPERT ACADEMY
tag:
 - Swexpert
 - Python
---

> D2_1946. 간단한 압축 풀기
>
> 문제:
>
> 원본 문서는 너비가 10인 여러 줄의 문자열로 이루어져 있다.
>
> 문자열은 마지막 줄을 제외하고 빈 공간 없이 알파벳으로 채워져 있고 마지막 줄은 왼쪽부터 채워져 있다.
>
> 이 문서를 압축한 문서는 알파벳과 그 알파벳의 연속된 개수로 이루어진 쌍들이 나열되어 있다. (예 : A 5    AAAAA)
>
> 압축된 문서를 입력 받아 원본 문서를 만드는 프로그램을 작성하시오.
>
> [예제]
> 압축된 문서의 내용
>
> A 10
> B 7
> C 5
>
> 압축을 풀었을 때 원본 문서의 내용
>
> AAAAAAAAAA
> BBBBBBBCCC
> CC
>
> [제약사항]
>
> 1. 압축된 문서의 알파벳과 숫자 쌍의 개수 N은1이상 10이하의 정수이다. (1 ≤ N ≤ 10)
> 2. 주어지는 알파벳 Ci는 A~Z의 대문자이다. (i는 줄의 번호로 1~N까지의 수)
> 3. 알파벳의 연속된 개수로 주어지는 수 Ki는 1이상 20이하의 정수이다. (1 ≤ Ki ≤ 20, i는 줄의 번호로 1~N까지의 수)
> 4. 원본 문서의 너비는 10으로 고정이다.

### 입력:

1. 테스트 케이스 T
2. 각 테스트 케이스에 N
3. N번 반복해서 Ci와 Ki가 빈칸을 사이에 두고 주어진다.



### 출력:

ex)

#1
AAAAAAAAAA
BBBBBBBCCC
CC

#### 생각한 로직:

- 문서의 길이는 10 
- 문제에서 설명한 그대로 입력을 받으면 문자를 그 갯수 만큼 한줄 한줄 채워서 넣는다.
- 그렇게 하려고 했는데 생각해보니 그럴 필요가 없네 그냥 한줄 배열에 때려박고 10개씩 부르면 되겠다.



#### 코딩:

```python
def Mreader(Char,num):
    global result
    for k in range(num):
        result.append(Char)

def print10(result):
    for j in range(len(result)):
        if j!=0 and j%10==0:
            print()
        print(result[j],end="")
    print()

for tc in range(1,int(input())+1):
    N = int(input())
    result = list()
    for i in range(N):
        char,num = input().split()
        Mreader(char,int(num))
    print("#{}".format(tc))
    print(result)

```



[출처]: https://www.swexpertacademy.com/