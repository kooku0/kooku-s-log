---
title: babel을 이용한 es6 환경 구축
date: 2020-01-05 15:01:40
category: node.js
---

**TypeScript로 node.js 서버 만들기**라는 포스팅을 먼저 해버려 순서가 조금 바뀌기는 했지만 ㅎㅎ **babel을 사용해서 es6 환경 구축을 하는 법**도 설명하려고 합니다.
babel에 대한 설명은 [(번역)Babel에 대한 모든 것 :: Jbee's blog](https://jaeyeophan.github.io/2017/05/16/Everything-about-babel/)을 참고하시면 될 것 같습니다.

## 1. 환경설정

우선 환경설정을 하도록 하겠습니다.

```shell
npm init
```

## 2. 모듈 설치

필요한 모듈들을 설치하겠습니다.

**@babel/cli** , **@babel/core** , **@bable/node** , **@babel/preset-env** , **express** , **nodemon**

```shell
npm install @babel/cli @babel/core @babel/node @babel/preset-env express nodemon
```

## 3. 서버코드 작성

`src` 폴더 안에 `app.js`파일을 만든 후 서버 코드를 작성합니다.

```javascript
import express from 'express'
;(function runServer() {
  const app = express()
  app.use(express.json())

  app.listen(5000, () => {
    console.log('start server')
  })
})()
```

## 4. nodemon 설정

dev 환경에서 자동 빌드를 하기 위하여 nodemon 설정을 하도록 하겠습니다.
`nodemon.json` 파일을 만든 후 다음과 같이 셋팅을 해줍니다.

```json
{
  "watch": ["src"],
  "ext": "ts",
  "delay": "2500ms",
  "ignore": ["src/public", "test/*"],
  "exec": "set NODE_ENV=devlopment&& babel-node ./src/app.js"
}
```

여기서 `exec`를 조금 살펴봐야하는데 설치한 모듈인 babel-node로 src/app.js 파일을 자동 빌드 하도록 합니다.

## 5. babelrc 설정

**tsc**는 `tsconfig.json`, **nodemon**은 `nodemon.json`파일에서 옵션을 설정하였습니다. **babel**은 `.babelrc`에서 환경설정을 해줍니다.

```json
// .babelrc
{
  "presets": ["@babel/preset-env"]
}
```

`presets`에 들어가는 건 어떻게 컴파일 할지 입니다. `es5`, `es6`, `stage`등 여러 설정을 할 수 있지만 저희는 그런것들 신경쓰지 않고 모두 한 번에 적용되는 `@babel/preset-env`을 사용하도록 하겠습니다.

## 6. package.json 설정

마지막으로 `package.json`에 script 명령어를 작성하도록 하겠습니다.

```json
{
  "name": "server",
  "version": "0.0.1",
  "description": "node server",
  "main": "app.js",
  "scripts": {
    "start": "nodemon --config ./nodemon.json",
    "build": "rd /s build && babel ./src --out-dir ./build",
    "watch": "babel ./src --out-dir ./src/public -w"
  },
  "author": "kooku",
  "license": "MIT",
  "dependencies": {
    "@babel/cli": "^7.7.7",
    "@babel/core": "^7.7.7",
    "@babel/node": "^7.7.7",
    "@babel/preset-env": "^7.7.7",
    "express": "^4.17.1",
    "nodemon": "^2.0.2"
  }
}
```

_끝~_

### Reference

- [(번역)Babel에 대한 모든 것 :: Jbee's blog](https://jaeyeophan.github.io/2017/05/16/Everything-about-babel/)
