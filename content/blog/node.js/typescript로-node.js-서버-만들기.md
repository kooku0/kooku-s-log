---
title: typescript로 node.js 서버 만들기
date: 2019-12-30 20:12:42
category: node.js
---

TypeScript를 이용하여 Node 서버를 만든 것은 LINE 인턴을 했을때 처음 접하였다. 그리고 몇 달이 지나서 당근마켓의 과제 sample 코드를 보면서 다시 흥미를 가지게 되었고 이번에 다시 공부를 하여 포스팅을 하려고한다.

## Overview

TypeScript로 Node.js 서버 만들기 첫번째 포스팅에서는 ts-node, nodemon, tcs등을 이용하여 셋팅하는 방법을 소개하려고 합니다. 관련 내용은 당근마켓의 과제 sample 코드를 바탕으로 공부하여 작성하였습니다.

## Setting

### 1) npm init

```shell
npm init
```

```
package name: server
version: 0.0.1
discription: node server
git repository: // 알아서..
keywords: // 알아서..
author: (kooku) //알아서..
license: MIT
```

### 2) install dependencies

처음에 설치해줄 것은 총 6개 입니다.

typescript, express, nodemon, ts-node, @types/express @types/node

```shell
npm install typescript express nodemon ts-node @types/express @types/node
```

### 3) Server Code 작성

TypeScript의 경우 바로 실행을 시키지 못하기 때문에 tsc 이나 ts-node를 이용해서 컴파일을 해줘야합니다.

우선 컴파일이 잘 되는지 테스트를 하기 위해서 루트 디렉토리에 src 폴더를 만들고 그 안에 app.ts 파일을 만들어줍니다.

```typescript
// src/app.ts
import * as express from 'express'

function runServer() {
  const app = express()
  app.listen(5000, () => {
    console.log('start server')
  })
}
runServer()
```

이렇게 설정한 후

```shell
npx tsc src/app.ts
```

위의 명령어를 치게 된다면 src 디렉토리안에 `app.js`라는 컴파일된 js 파일이 생성된 것을 알 수 있습니다.

하지만 이렇게 일일이 옵션과 outputDir 경로들을 작성해주기 귀찮기 때문에 config 파일을 만들어 줄 겁니다.

### 4) tsconfig

우선 TypeScript 옵션을 지정해주기 위하여 tsconfig 파일을 만들어 주겠습니다.

```shell
npx tsc --init
```

위의 명령어를 실행하게되면 `tsconfig.json`파일이 만들어지게됩니다. 여기에 많은 Options이 주석처리 되어있어 살펴보고 필요한 부분은 주석을 해제하면 됩니다.

> 자세한 옵션들의 내용은 공식홈페이지에서 확인할 수 있습니다. <https://www.typescriptlang.org/docs/handbook/tsconfig-json.html>

저는 sample 코드에 있었던 Options을 그대로 사용하였습니다.

```json
{
  "compilerOptions": {
    "module": "commonjs",
    "strict": true,
    "baseUrl": "./",
    "outDir": "build",
    "removeComments": true,
    "experimentalDecorators": true,
    "target": "es6",
    "emitDecoratorMetadata": true,
    "moduleResolution": "node",
    "importHelpers": true,
    "types": ["node"],
    "typeRoots": ["../node_modules/@types"]
  },
  "include": ["./src/**/*.ts"],
  "exclude": ["./src/public/*", "../node_modules/**/*"]
}
```

여기서 중요한 것은 exclude와 include 그리고 outDir 입니다. include는 컴파일 할 파일들이고, exclude는 컴파일 제외 목록입니다. 그리고 outDir은 컴파일 후 파일들이 들어갈 곳입니다.

이렇게 한 후

```shell
npx tsc
```

위와 같은 명령어를 치게 된다면 `tsconfig.json`파일에 설정된 것처럼 빌드된 js파일들이 `build`폴더안에 생성되게 됩니다.

그리고 `node app.js`를 이용해서 서버를 키면 됩니다.

이제 어느정도 셋팅이 되었기 때문에 위의 내용들을 `script`에 등록시키겠습니다.

루트 디렉토리의 `package.json` 파일의 `scripts`에 명령어를 등록합니다.

```json
{
  "name": "node_test",
  "version": "0.0.1",
  "description": "ts-node test",
  "main": "app.ts",
  "scripts": {
    "build": "tsc",
    "start": "set NODE_ENV=production&& node build/app.js"
  },
  "author": "kooku",
  "license": "MIT",
  "dependencies": {
    "@types/express": "^4.17.2",
    "@types/node": "^13.1.1",
    "express": "^4.17.1",
    "nodemon": "^2.0.2",
    "ts-node": "^8.5.4",
    "typescript": "^3.7.4"
  }
}
```

이렇게 등록을 했다면 다음의 명령어로 서버를 킬 수 있습니다.

> `package.json`의 `scripts`에 등록된 명령어는 `npm run` 명령어를 통해 실행 시킬 수 있습니다.
> 하지만 많이 사용하는 stript인 `start`와 `test`는 run을 붙히지 않고 `npm start`, `npm test`로 실행 시킬 수 있습니다.

```shell
npm run build
npm run start
```

`start` 명령어중 `set NODE_ENV_production`의 경우 잠시후에 다시 소개하겠습니다.

### 5) Nodemon

하지만 위와 같은 방법으로 서버를 키게된다면 개발 속도가 너무 느린데요 그 이유는 코드를 한줄 수정하고 명령어를 두번 입력해야 되기 때문입니다. 따라서 node 개발에서 많이 사용하는 nodemon을 사용해 보겠습니다.

하지만 nodemon 은 javascript 파일만 반영되기 때문에 이때 ts-node라는 모듈을 사용해야합니다. ts-node는 메모리 상에서 typescript를 transpile 해주는 모듈입니다.

테스트를 해본결과

```shell
npx nodemon src/app.ts
```

위와 같은 명령어를 실행하면 알아서 ts-node를 불러와 수행하는 것을 확인할 수 있었다.

하지만 이러한 내용들도 `nodemon.json`을 이용하여 손쉽게 사용할 수 있다.

우선 `nodemon.json`파일을 만들어주자

```json
{
  "watch": ["src"],
  "ext": "ts",
  "delay": "2500ms",
  "ignore": ["src/public", "test/*"],
  "exec": "set NODE_ENV=dev&& ts-node src/app.ts"
}
```

잘 보면알겠지만 `src` 디렉토리에서의 변경내용들을 확인하여 자동으로 restart 시켜줍니다. 그리고 development 환경이기에 환경변수 `NODE_ENV=dev`를 설정하는데. 위에서 scripts 명령어중 `start` 에 `NODE_ENV=production`을 적어준 것과 같은 맥락입니다.

> 참고로 `NODE_ENV` 환경변수 설정에서 set이라는 명령어를 사용했지만 이것은 window에서의 명령어고, maxOS or Linux에서는 다른 명령어를 사용한다.
>
> NODE_ENV의 경우 개발환경인지 배포환경이지에 따라 다른 작업을 수행할 필요가 있을 때를 위하여 작성하였습니다.

`nodemon.json` 설정이 다 되었으므로 `package.json`에 `start:dev` 명령어를 추가하겠습니다.

```json
{
  "name": "node_test",
  "version": "0.0.1",
  "description": "ts-node test",
  "main": "app.ts",
  "scripts": {
    "start:dev": "nodemon --config ./nodemon.json",
    "build": "tsc",
    "start": "set NODE_ENV=production&& node build/app.js"
  },
  "author": "kooku",
  "license": "MIT",
  "dependencies": {
    "@types/express": "^4.17.2",
    "@types/node": "^13.1.1",
    "express": "^4.17.1",
    "nodemon": "^2.0.2",
    "ts-node": "^8.5.4",
    "typescript": "^3.7.4"
  }
}
```

### 마무리

이렇게 모든 설정이 완료되었습니다.

이렇게 설정을 한 후 개발할 때는

```shell
npm run start:dev
```

배포할 때는

```shell
npm run build
npm run start
```

하면 끝~!
