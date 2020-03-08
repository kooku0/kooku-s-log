---
title: Basic of Redux
date: 2020-03-08 23:03:11
category: react
---

# Basic of Redux

![img](./images/redux.png)

```js
function reducer(oldState, action) {
	//....
}
var store = Redux.createStore(reducer)
```

render라는 값을 통해서 state를 참조해서 뷰를 그린다.

```js
function render() {
    var state = store.getState()
    //...
    document.querySelectore('#app').innerHTML = `<H1>WEB</H1>`
}
```

action이 dispatch에게 전달이 된다.

dispatch는 두 가지 일을 하는데, reducer를 이용해서 state값을 바꾼다. 그리고 그 작업이 끝나면 subscribe를 호출해서 render 함수를 호출한다. 

```js
store.subscribe(render)
```

dispatch가 reducer를 호출할때 두 가지를 전달하는데, 1. 현재의 state, 2. action type


```js
store.dispatch({type: 'decrease', payload: -1})
```

reducer는 state를 입력값으로 받고, action을 참조해서 새로운 state를 만들어내는 state 가공자입니다.

리듀서가 리턴하는 값이 새로운 state값이 된다.

```js
function reducer(state, action) {
    if(action.type === 'decrease') {
        return {
            state += action.payload
        }
    }
}
```

이 후 dispatch가 subscribe에 등록되어있는 render를 호출하면 render가 getState를 호출하여 state값을 들고와서 새롭게 화면을 갱신하게 됩니다.

*다시 정리 할 예정*

# Reference

[Redux-2.1. 리덕스 여행의 지도 : 소개 :: 생활코딩](https://www.youtube.com/watch?v=N9PT9iNTZAE)