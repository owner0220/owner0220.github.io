---
title: SW Expert Academy - D2_2005
date: 2019-08-27 12:28:00
categories:
 - Algorithm
tag:
 - Swexpert
 - Python
---

> D2_2005. 파스칼의 삼각형
>
> 문제:
>
> - 크기가 N인 파스칼의 삼각형을 만들어야 한다.
>
> - 파스칼의 삼각형이란 아래와 같은 규칙을 따른다.
>
> - \1. 첫 번째 줄은 항상 숫자 1이다.
>
> - \2. 두 번째 줄부터 각 숫자들은 자신의 왼쪽과 오른쪽 위의 숫자의 합으로 구성된다.
>
> - N이 4일 경우,
>
>   ![img](https://www.swexpertacademy.com/main/common/fileDownload.do?downloadType=CKEditorImages&fileId=AV5P1SEKAlYDFAUq)
>
> - N을 입력 받아 크기 N인 파스칼의 삼각형을 출력하는 프로그램을 작성하시오.

### 입력:

1. 테스트 케이스 T
2. 각 테스트 케이스에는 N이 주어진다.  
3. 파스칼의 삼각형의 크기 N은 1 이상 10 이하의 정수이다. (1 ≤ N ≤ 10)



### 출력:

ex)

#1
1
1 1
1 2 1
1 3 3 1



#### 생각한 로직:

- 문제에서 말하는 것처럼 순서대로 파스칼의 삼각형 뜻대로 구현



#### 코딩:

```python
for tc in range(1,int(input())+1):
    N=int(input())
    result = [[1]]
    #한줄 한줄씩
    for i in range(1,N):
        #한줄 만들어 저장할 공간
        tmp=list()
        for j in range(i+1):
            #처음과 마지막을 제외한 구간에서는 파스칼의 삼각형 원칙대로 되어야 하니
            if 0<j<i:
                tmp.append(result[i-1][j]+result[i-1][j-1])
            #이외에는 1을 추가해준다.
            else:
                tmp.append(1)
        #다 만들어진 한줄 한줄을 삼각형에 추가시켜 놓는다.
        result.append(tmp)
    print("#{}".format(tc))
    for item in result:
        print(*item)
```



[출처]: https://www.swexpertacademy.com/