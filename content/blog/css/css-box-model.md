---
title: CSS Box Model
date: 2019-12-02 13:12:38
category: css
---

> 원래 CSS에 관심이 많이 없으나, 이번 카카오 개발자 겨울 인턴 면접에서 CSS Box Model에 대한 질문을 받아 정리하고자 한다.


모든 HTML elements는 박스로 간주된다. CSS에서는 설계와 레이아웃에 대해 이야기할 때 "box model"이라는 용어를 사용한다.

CSS box model은 본질적으로 모든 HTML element를 감싸는 상자다. 이것은 margins, borders, padding 그리고 content로 구성되어 있다.


### Explanation of the different parts:

* **Content** - 텍스트 및 이미지가 나타나는 box의 내용

* **Padding** - Border 안쪽 여백. 패딩은 투명하다.

* **Border** - Padding 및  Content를 감싸는 테두리. Padding과 Margin을 나누는 경계선

* **Margin** - Border 바깥쪽 여백. Margin은 투명하다.


### Reference

* [The CSS Box Model](https://www.w3schools.com/css/css_boxmodel.asp)