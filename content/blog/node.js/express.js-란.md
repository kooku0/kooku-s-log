---
title: Express.js 란
date: 2019-12-31 19:12:63
category: node.js
---

**Express** 공식문서를 참고하면 **Express**를 다음과 같이 설명하고 있습니다.

_<div align="center">Node.js를 위한 빠르고 개방적인 간결한 웹 프레임워크</div>_

개발자는 다양한 기능의 미들웨어를 필요한 것만 선택하여 익스프레스와 결합해 사용할 수 있습니다. 이러한 미들웨어(Middleware) 구조이기에 **Express**는 간결하고 유연한 Node.js 웹 어플리케이션 프레임워크라고 부릅니다.

미들에어 구조라는게 무슨 말이냐면 로봇에 각종 부품들을 장착하듯이 express에 javascript로 작성된 다양한 기능들을 붙힐 수 있다는 것입니다.

```javascript
// my-middleware.js
module.exports = function(options) {
  return function(req, res, next) {
    // options 객체에 따른 middleware 함수 구현
    next()
  }
}
```

위에 작성된 미들웨어를 개발자는 다음과 같이 사용할 수 있습니다.

```javascript
var mw = requjire('./my-middleware.js')

app.use(mw({ option1: '1', option2: '2' }))
```
