---
title: async & await 완벽 가이드
date: 2020-03-11 07:03:09
category: javascript
---

async & await에 대하여 겉핡기식으로 공부하니 실제 코드를 짤때 이게 왜 되지? 이게 왜 안되지? 같은 상황이 많이 발생했습니다. 따라서 이번에 제대로 공부해 놓기로 마음 먹었습니다.



aync & await를 제대로 이해하고 사용하기 어려운 이유는 밑의 4단계를 모두 알아야 제대로 사용할 수 있기 때문입니다. 하나라도 부족하다면 제대로 이해하고 사용하기 어렵습니다.

## Index

[1. 자바스크립트 비동기 처리 과정의 이해](#1-자바스크립트-비동기-처리-과정의-이해)

[2. 비동기 처리와 콜백](#2-비동기-처리와-콜백)

[3. Promise](#3-promise)

[4. aync와 await](#4-async와-await)

[5. Promise 응용](#5-Promise-응용)

[Reference](#reference)

## 1. 자바스크립트 비동기 처리 과정의 이해

### 자바스크립트 엔진

자바스크립트 엔진은 기본적으로 하나의 쓰레드에서 동작한다. 하나의 쓰레드를 가지고 있다는 것은 하나의 stack을 가지고 있다는 의미와 같고, 하나의 stack이 있다는 의미는 `동시에 단 하나의 작업만을 할 수 있다`는 의미이다.

자바스크립트 엔진은 하나의 코드 조각을 하나씩 실행하는 일을 하고, 비동기적으로 이벤트를 처리하거나 Ajaxx 통신을 하는 작업은 사실상 Web API에서 모두 처리된다.

<img src='./images/browser-structure.png' />

<small>출처: [자바스크립트 비동기 처리 과정과 RxJS Scheduler :: 아내와 아들 그리고 딸밖에 모르는 남편](http://sculove.github.io/blog/)</small>

자바스크립트가 동시에 단 하나의 작업만을 한다는데 어떻게 여러가지 작업을 비동기로 작업 할 수 있을까?

그 비밀은 바로 `Event Loop`와 `Queue`에 있다.

### Event Loop 와 Queue

Event Loop에서 Loop의 사전적인 의미는 ‘반복. 순환’이다. Event Loop는 사전적인 의미처럼 계속 반복해서 call stack과 queue 사이의 작업들을 확인하고, call stack이 비워있는 경우 queue에서 작업을 꺼내어 call stack에 넣는다.
자바스크립트는 이 Event Loop와 Queue들을 이용하여 비동기 작업을 수행한다.
직접적인 작업은 Web API에서 처리되고, 그 작업들이 완료되면 요청시 등록했던 callback이 queue에 등록된다.
Event Loop는 이 작업들을 Queue에서 꺼내어 처리한다.
Event Loop는 stack에 처리할 작업이 없을 경우 우선적으로 microtask queue를 확인한다. microtask queue에 작업이 있다면 microtask에 있는 작업을 꺼내서 call stack에 넣는다. 만약 microtask의 queue가 비어서 더 이상 처리할 작업이 없으면 이때 task queue를 확인한다. task queue의 작업도 꺼내서 call stack에 넣는다.
이렇게 Event Loop와 Queue는 자바스크립트 엔진이 하나의 코드 조각을 하나씩 처리할 수 있도록 작업을 스케줄하는 동시에 이러한 이유로 우리는 자바스크립트에서 비동기 작업을 할수 있도록 해준다.

### Promise 처리 과정

```js
console.log("script start")

Promise.resolve()
    .then(() => console.log("promise1"))
    .then(() => console.log("promise2"))

console.log("script end")
```

위의 코드를 실행하면 다음과 같은 결과 화면을 얻을 수 있다.

```
script start
script end
promise1
promise2
```

간단해 보이는 이 코드는 실제 다음과 같이 처리된다.

1. 'script 실행 작업'이 stack에 등록된다.
2. console.log('script start')가 처리된다.
3. Promise작업이 stack에 등록되고, Web API에게 Promise작업을 요청한다. 이떄 Promise.then의 callback함수를 함께 전달한다. 요청 이후 stack에 있는 Promise 작업은 제거된다.
4. Web API는 Promise 작업이 완료되면 Promise.then의 callback 함수를 miscrotask queue에 등록한다.
5. console.log('script end')가 처리된다.
6. 'script 실행 작업'이 완료되어 stack에서 제거된다.
7. stack이 비워있어서 microtask queue에 등록된 Promsie.then의 callback 함수를 stack에 등록한다.
8. 첫번째 Promise.then의 callback 함수가 실행되어 내부의 console.log('promise1')가 처리된다.
9. 첫번째 Promise.then 다음에 Promise.then이 있다면 다음 Promise.then의 callback 함수를 microtask queue에 등록한다.
10. stack 에서 첫번째 Promise.then의 callback 함수를 제거하고 microtask queue에서 첫번째 Promise.then의 callback 함수를 제거한다.
11. 두번째 Promise.then의 callback 함수를 stack에 등록한다.
12. 두번째 Promise.then의 callback 함수가 실행되어 내부의 console.log(‘promise2’)가 처리된다.



## 2. 비동기 처리와 콜백

콜백은 비동기 처리의 가장 기초적인 부분입니다.

### ajax

```js
function getData() {
    var data
	data = $.get('http://localhost:3000', response => data = response)
    return data
}
console.log(getData()) // undefined
```

`$.get()`로 ajax 통신을 한 후 response를 data 변수에 넣어줍니다. 하지만, retrun 이 response값을 기다리지 않고 반환함으로 undefined가 출력됩니다.

### setTimeout

```js
// #1
console.log('Hello');
// #2
setTimeout(function() {
	console.log('Bye');
}, 3000);
// #3
console.log('Hello Again');
```

원하는 결과

```
'Hello'
'Bye'
'Hello Again'
```

실제 출력값

```
'Hello'
'Hello Again'
'Bye'
```

### 콜백 함수로 해결하기

이제는 콜백함수로 문제를 해결해 보겠습니다.

```js
function getData(callbackFunc) {
	$.get('http://localhost:3000', function(response) {
		callbackFunc(response); // 서버에서 받은 데이터 response를 callbackFunc() 함수에 넘겨줌
	});
}

getData(function(tableData) {
	console.log(tableData); // $.get()의 response 값이 tableData에 전달됨
});
```

## 3. Promise

프로미스는 Callback Hell을 막기 위해 나온 것 입니다. 아주 유용하죠.

위의 문제들을 Promise로 해결해 보겠습니다.

```js
function getData() {
	return new Promise((resolve, reject) => 
    	$.get('http://localhost:3000', (response) => resolve(response))
    )
}

getData().then((data) => console.log(data))
```

```js
function sayBye() {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            console.log('Bye')
            resolve()
		}, 3000)
    })
}

// #1
console.log('Hello');
// #2
sayBye()
    .then(() => 
          // #3
          console.log('Hello Again')
     )
```

### 에러 처리 방법

Promise는 `catch`로 에러 헨들링을 편하게 할 수 있습니다.

```js
function getData() {
    return new Promise((resolve, reject) => reject('failed'))
}
getData()
	.then()
	.catch((err) => console.log(err))
```



## 4. async와 await

`async`와 `await`는 `axios`를 사용할 때 많이 사용하게 됩니다.

사용법은 매우 간단합니다.

```js
function apiCall() {
	return new Promise((resolve, reject) => 
    	$.get('http://localhost:3000', (response) => resolve(response))
    )
}

async function getData() {
	const response = await apiCall()
	console.log(response)
}
```

```js
function sayBye() {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            console.log('Bye')
            resolve()
		}, 3000)
    })
}

async function hello() {
	console.log('Hello')
    await sayBye()
    console.log('Hello Again')
}
```

코드에서 보시면 알겠지만 `Promise`와 `async await`는 독립된 개념이 아닙니다. `await`는 `promise`를 기다리는 역할입니다. 만약 `await`에  함수의 return `Promise`를 물려놓치 않았다면 `await`를 잘 못 사용하고 있는 것 입니다.



### 예외 처리

async & await에서 예외를 처리하는 방법은 `try catch`입니다.

```js
function apiCall() {
	return new Promise((resolve, reject) => 
    	$.get('http://localhost:3000', (response) => resolve(response))
    )
}

async function getData() {
    try {
        const response = await apiCall()
        console.log(response)
    }
    catch (err) {
        console.error(err)
    }
}
```

## 5. Promise 응용

`async & await`가 `Promise`보다 좋아보이지만 `Promise`를 써야하는 상황도 있습니다. 그런 상황을 한 번 보겠습니다.

```js
const queryExec = sql => new Promise ((resolve, reject) => {
  con.query(sql, function (err, res) => {
    err ? reject(err) : resolve(res) // reject는 예외 처리를 할 때 사용합니다.
  })
});
(async () => {
  try {
    const res1 = await queryExec(sql1)
    const res2 = await queryExec(sql2)
    const res3 = await queryExec(sql3)
    const res4 = await queryExec(sql4)
    const res5 = await queryExec(sql5)
    /* ... 최종 처리 코드 작성 ... */
  } catch (err) {
    console.log(err)
  }
})()
```

위와 같은 코드는 어떤 방식으로 처리가 될까요?

첫 번째 `queryExec` 함수가 끝나게 되면 두 번째 `queryExec`가 수행됩니다. 즉, 함수가 하나씩 차례대로 수행되게 됩니다.

만약 위의 5개의 함수를 동시에 수행하고 기다리는 방법은 없을까요?

그걸 해결하는 하나의 대안이 제시되었었는데, ES2016에서 채택되지는 못했습니다.

```js
let [foo, bar] = await* [getFoo(), getBar()]
```

대신에 다음과 같이 사용이 가능합니다.

```js
let [foo, bar] = await Promise.all([getFoor(), getBar()])
```

`Promise.all`은 `Promise` 들의 배열을 받습니다. 그리고 그걸 다 합쳐서 하나의 `Promise`로 만듭니다.

**그 하나의 Promise는 배열 안에 있는 모든 구성원 Promise 들이 resolved될 때 비로소 resolve 합니다.**

## Reference

* [자바스크립트 async와 await :: CAPTAIN PANGYO](https://joshua1988.github.io/web-development/javascript/js-async-await/)

* [자바스크립트 비동기 처리와 콜백 함수 :: CAPTAIN PANGYO](https://joshua1988.github.io/web-development/javascript/javascript-asynchronous-operation/)

* [자바스크립트 Promise 쉽게 이해하기 :: CAPTAIN PANGYO](https://joshua1988.github.io/web-development/javascript/promise-for-beginners/)

* [[번역] async/await 를 사용하기 전에 promise를 이해하기 :: MEDIUM](https://medium.com/@kiwanjung/번역-async-await-를-사용하기-전에-promise를-이해하기-955dbac2c4a4)

* [[javascript] Promise, async, awaitf :: 개발자 황준일](http://junil-hwang.com/blog/javascript-promise-async-await/)

* [자바스크립트 비동기 처리 과정과 RxJS Scheduler :: 아내와 아들 그리고 딸밖에 모르는 남편](http://sculove.github.io/blog/2018/01/18/javascriptflow/)