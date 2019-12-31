---
title: npm vs npx
date: 2020-01-01 05:01:65
category: node.js
---

## 1. npm: node package manager

Node로 개발을 하다보면 npm 명령어를 정말 많이 사용했을 것이다. **npm**은 Node Package Manager의 약자로 모듈들을 관리해주는 툴입니다. **npm**은 Node.js를 설치하면 같이 설치가 되는데 이를 통해 **npm**이 중요한 것이라는 것을 유추해볼 수 있다.

```shell
npm install MODULE_NAME
```

보통 모듈 패키지들은 프로젝트 단위로 작업을 하기에 `npm init`을 통하여 만들어진 `package.json`파일로 프로젝트 모듈들을 관리한다. python에서의 `requirement.txt`와 같은 역할이다.

## 2. npx: npm package runner

[npm@5.2.0](https://github.com/npm/npm/releases/tag/v5.2.0) (2017-07-05) 이후로 npx가 추가되었습니다. 해당 릴리즈 노트에는 **npx**를 다음과 같이 소개하고 있습니다.

> #### npx!!!
>
> npx is a tool intended to help round out the experience of using packages from the npm registry — the same way npm makes it super easy to install and manage dependencies hosted on the registry, npx is meant to make it easy to use CLI tools and other executables hosted on the registry. It greatly simplifies a number of things that, until now, required a bit of ceremony to do with plain npm.

여기서 말하는 npm registry는 npm에서 운영하는 npm 모듈들을 관리하는 곳 입니다. 만약 `npm install MODULE_NAME`으로 모듈을 설치했다면 해당 모듈은 npm registry에 있는 것을 다운받는 것 입니다.

### Reference

- [the npm blog](https://blog.npmjs.org/post/162869356040/introducing-npx-an-npm-package-runner)
