---
title: "CSS - BoxModel"
layout: post
date: 2019-07-22 11:43
tag:
- css
- web
category: blog
author: insik
description: CSS - 레이아웃 박스모델 설명
---



# CSS

## 박스 모델(Box Model)

```html
<!doctype html>
<html lang="ko">
  <head>
    <meta charset="utf-8">
    <title>CSS</title>
    <style>
      h1 {
        margin: 30px;
        border: 30px solid #dddddd;
        padding: 30px;
      }
    </style>
  </head>
  <body>
    <h1>Box Model</h1>
  </body>
</html>
```



#### 실행 화면

![CSS_BoxModel_01](../assets/images/post_img/CSS_BoxModel_01.png)



![CSS_BoxModel_02](../assets/images/post_img/CSS_BoxModel_02.png)

색이 있는 모든 영역이 h1 요소입니다. 각 색이 나타내는 영역은 다음과 같습니다.

- ① : 바깥 여백 영역(Margin Area)
- ② : 테두리 영역(Border Area)
- ③ : 안쪽 여백 영역(Padding Area)
- ④ : 내용 영역(Content Area)

각 영역을 꾸밀 때 사용하는 속성은 다음과 같습니다.

- 바깥 여백 : margin 속성
- 테두리 : border 속성
- 안쪽 여백 : padding 속성
- 박스의 가로 크기 : width 속성
- 박스의 세로 크기 : height 속성
- 박스의 크기 기준 : box-sizing 속성
- 박스의 배경 : background 속성

> 참조 : https://www.codingfactory.net/10556