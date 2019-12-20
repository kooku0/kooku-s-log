---
title: SCSS 문법 정리
date: 2019-12-20 18:12:96
category: css
---

블로그 코드 스타일이 내가 좋아하는 스타일이 아니라 커스터마이징을 하려고 하였다. 하지만 SCSS로 쓰여있어 이번에 제대로 공부해서 바꿔보려고 한다.

## Overview

SASS(Syntactically Awesome Style Sheets)의 3버전에서 새롭게 등장하는 SCSS는 CSS 구문과 완전히 호환되도록 새로운 구문을 도입해 만든 SASS의 모든 기능을 지원하는 CSS의 상위집합입니다.

## 문법 정리

### 셀렉터 네스팅

선택자 내에 속성 정의만 하는게 아닌 nested된 속성 정의 블럭이 들어올 수 있다.

```scss
.entry-content {
    p { font-size: 9.1rem; }
}

// compiled to 
.entry-content p {
    font-size: 9.1rem;
}
```

### 속성 네스팅

선택자가 네스팅되는 것과 유사하게, 특정한 속성들의 집합들도 네스팅이 가능하다. 예를 들어 font의 경우 `font-family`, `font-size`, `font-weight`등이 주로 세트로 정의된다.

```scss
.entry-content {
    p {
        font: {
            family: "Noto Serif CJK KR", serif;
            size: 9.814rem;
            weight: 400;
        }
    }
}
```

### 상위요소 참조

`&`를 사용하면 현재 블럭이 적용되는 셀렉터를 참조한다. 정확하게는 참조가 아닌 치환이다.

```scss
a {
    text-decoration: none;
    &:hover { text-decroation: underline; }
}
.widget {
    font-weight: 400;
    &-area { font-weight: 600; }
    &-top_posts { font-weight: 1000; }
}

// compiled to
a { text-decoration: none; }
a:hover { text-decoration: underline; }
.widget { font-weight: 400; }
.widget-area { font-weight: 600; }
.widget-top_posts { font-weight: 1000; }
```

### 문자열의 치환 및 내삽(interpolation)

`#{...}`을 사용하면 문자열 내에 표현식의 결과를 내삽하거나, 다른 변수의 내용으로 치환하는 것이 가능하다.

```scss
$foo: bar;
$fontsize: 12px;
$lineheight: 30p;

p {
  font: #{$fontsize}/#{$lineheight};
  &.#{$foo} { color: red; }
}

//compiled to 
p { font: 12px/30px; }
p.bar { color: red; }
```

### 임포트

`@import` 지시어를 이용해서 다른 css 파일을 임포트할 수 있다.

### 확장

확장은 이미 정의해둔 다른 셀렉터의 속성에 현재 셀렉터가 얹어가는 효과를 낼 수 있다. 

```scss
// 베이스 클래스
.message {
  border: 1px solid #ccc;
  padding: 10px;
  color: #333;
}

.success {
  @extend .message;
  border-color: green;
}

.error {
  @extend .message;
  border-color: red;
}

// compiled to 
.message, .success, .error {
  border: 1px solid #cccccc;
  padding: 10px;
  color: #333;
}

.message { border-color: green; }
.error { border-color: red; }
```

### 믹스인

공통적으로 많이 쓰이는 CSS 선언값들을 묶어서 믹스인으로 만들어 재사용이 가능하게끔 할 수 있다.

믹스인을 정의할 때에는 파라미터를 받을 수 있게끔 할 수 있기 때문에 단순 복붙이 아닌 파라미터 값에 따른 가변적 속성 집합을 만들어 유용하게 쓸 수 있다.

```scss
@mixin border-radius($radius) {
  -webkit-border-radius: $radius;
  -moz-border-radius: $radius;
  -ms-border-radius: $radius;
  border-radius: $radius;
}

.box { @include border-radius(10px); }
```

#### 리스트 인자

인자며에 `...`을 붙이면 단일 값이 아닌 리스트로 인자를 받는 다는 의미이다.

예를 들어 그림자 속성은 컴마로 구분된 리스트가 될 수 있다.

```scss
@mixin box-shadow($shadows...) {
  -moz-box-shadow: $shadows;
  -webkit-box-shadow: $shadows;
  box-shadow: $shadows;
}

.shadows { @include box-shadows(0px 4px 5px #666, 2px 6px 10px #999); }
```

### 블럭을 믹스인에 넘기기

흔한 케이스는 아니지만, 블럭 자체를 믹스인에 넘겨줄 수 있다.  믹스인내에서 `@content` 지시어를 쓴 부분이 넘겨받은 블럭으로 치환된다.

```scss
@mixin code-inline {
  code {
    background-color: #cecece;
    padding: 2px;
    border-radius: @include border-raidus(4px);
    font-family: monospaces;
    @content
  }
}

p {
  @include code-inline {
    color: #33ccff;
    font-size: .8em;
  }
}
```



## Reference

* [SCSS/SASS 문법 정리 :: Wireframe](https://soooprmx.com/archives/7948)

* [SASS 공식문서](https://sass-lang.com)