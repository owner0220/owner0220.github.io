---
title: SW Expert Academy - D2_1974
date: 2019-08-20 12:28:00
categories:
 - Algorithm
tag:
 - Swexpert
 - Python
---

# D2_1974. 스도쿠 검증

### 문제:

- 스도쿠는 숫자퍼즐로, **가로 9칸 세로 9칸**으로 이루어져 있는 표에 **1 부터 9 까지의 숫자**를 채워넣는 퍼즐이다.

  ![img](https://www.swexpertacademy.com/main/common/fileDownload.do?downloadType=CKEditorImages&fileId=AV5PtLXqAYUDFAUq)

  같은 줄에 

  1 에서 9 까지의 숫자를 한번씩만 넣고, 3 x 3 크기의 작은 격자 또한, 1 에서 9 까지의 숫자가 겹치지 않아야 한다.

   

  ![img](https://www.swexpertacademy.com/main/common/fileDownload.do?downloadType=CKEditorImages&fileId=AV5PtUu6AYYDFAUq)

  입력으로 9 X 9 크기의 스도쿠 퍼즐의 숫자들이 주어졌을 때, 위와 같이 겹치는 숫자가 없을 경우, 1을 정답으로 출력하고 그렇지 않을 경우 0 을 출력한다.

### 입력:

1. 테스트 케이스 T

2. 9*9 크기의 퍼즐 데이터를 테스트 케이스 마다 준다.

   (입력으로 주어지는 퍼즐의 모든 숫자는 1 이상 9 이하의 정수이다.)

   (퍼즐은 모두 숫자로 채워진 상태로 주어진다)



### 출력:

ex)

#1 1
#2 0
#3 1
#4 0
#5 0
#6 1
#7 0
#8 1
#9 1
#10 0



#### 생각한 로직:

- 스도쿠를 한칸 한칸 탐색하면서 한번이라도 중복이 나오면 return 0 문제 없이 다 돌아간다면 return 1
- 중요한 것은 한칸 한칸 빠짐없이 탐색하면서 중복을 어떻게 처리할 것인가



#### 코딩:

```python
import sys
sys.stdin = open("1.txt","r")

def chksudoku(sudoku):
    for i in range(9):
        garo = list()
        sero = list()

        for j in range(9):
            #가로탐색
            if sudoku[i][j] in garo:
                return 0
            else:
                garo.append(sudoku[i][j])
            #세로탐색
            if sudoku[j][i] in sero:
                return 0
            else:
                sero.append(sudoku[j][i])
            if i%3==0 and j%3==0:
                sq = list()
                for k in range(3):
                    for l in range(3):
                        if sudoku[i+k][j+l] in sq:
                            return 0
                        else:
                            sq.append(sudoku[i+k][j+l])
            # print(garo)
            # print(sero)
            # print(sq)
            # print("=======================")
    return 1

    #네모탐색




for tc in range(1,int(input())+1):
    sudoku=[list(map(int,input().split())) for x in range(9)]
    #각자 탐색하면서 리스트에 넣다가 하나라도 중간에 중복되는게 있으면 out!
    result = chksudoku(sudoku)

    print("#{} {}".format(tc,result))
```



[출처]: https://www.swexpertacademy.com/