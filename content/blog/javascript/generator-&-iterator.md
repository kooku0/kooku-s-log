---
title: Generator & Iterator
date: 2020-02-02 19:02:49
category: javascript
---

> 해당 포스팅은 [반복기 및 생성기 :: MDN Web docs](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Iterators_and_Generators)를 바탕으로 정리를 하였습니다.

요즘 react-saga를 공부하고 있습니다. 공부하는 와중에 `function*` 키워드를 마추치곤, 이게 뭐지? 라는 생각이 들었고 **Generator** 와 **Iterator**에 대하여 공부를 하게 되었습니다. 

## 들어가기 전에

react를 1년 가까이 했지만 **Generator** 와 **Iterator**를 사용한 적이 없습니다. 굳이 고르자면 `for .. of .. ` 정도? 하지만 공부해보니 알아두면 좋을 것 같다는 생각이 많이 들었습니다. 이 포스팅이 많은 도움이 되길 바랍니다.

## Index

1. **[반복자(Iterator)](#1-반복자iterator)** </br>
   - **[내장 iterable](#내장-iterable)**
2. **[Generator function](#2-generator-function)**
3. **[yield\* 표현식](#3-yield-표현식)**
4. **[마치며](#4-마치며)**

## 1. 반복자(Interator)

자바스크립트에서 **반복자(Iterator)**는 c++에서의 반복자를 떠올리면 이해하기 쉽습니다.</br>
반복자를 생성하면 `next()` 메소드를 반복적으로 호출하여 명시적으로 반복시킬 수 있습니다. 반복자를 반복시키는 것은 일반적으로 한 번씩만 할 수 있기 때문에, 반복자를 소모시키는 것이라고 할 수 있습니다.</br>
`next()` 메소드를 호출했을 시 `{value: .. , done: false}`의 객체를 반환하며 마지막 값을 산출하고나서 `next()`를 추가적으로 호출하면 `{done: true}` 가 반환됩니다.

### 내장 iterable

`String`, `Array`, `TypeArray`, `Map` 및 `Set`은 모두 내장 반복가능 객체입니다. 그들의 프로토타입 객체가 모두 `Symbol.iterator` 메서드가 있기 때문입니다.

```js
for(let value of ['a', 'b', 'c']) {
  console.log(value)
}
// "a"
// "b"
// "c"

[...'abc'] // ["a", "b", "c"]

function* gen() {
  yield* ['a', 'b', 'c']
}

gen().next() // { value: "a", done: false }

const s = new Set()
s.add(1)
s.add(0)
s.add(2)

for(let i of s.keys()) {
  console.log(i)
}
// 1
// 0
// 2
```

## 2. Generator function

**Generator function**은 `function*` 문법을 사용하여 작성됩니다. 생성자의 함수가 최초로 호출될 때, 함수 내부의 어떠한 코드도 실생되지 않고, 대신 생성자라고 불리는 반복자 타입을 반환합니다. 생성자의 `next()` 메소드를 호출함으로서 어떤 값이 소비되면, 생성자 함수는 `yield` 키워드를 만날 때까지 실행됩니다.</br>

생성자 함수는 원하는 만큼 호출될 수 있고, 매번 새로운 생성자를 반환합니다. 하지만, 각 생성자는 단 한 번만 순회될 수 있을 것입니다.

다음의 코드를 보면 이해하기 쉽습니다.

```js
function* markRangeIterator(start = 0, end = 10, step = 2) {
  let n = 0
  for (let i = start; i < end; i == step) {
    n++
    yield i
  }
  return n
}
```

이후 다음과 같이 호출해 보겠습니다.

```js
const generator = markRangeIterator()

console.log(generator.next()) // {value: 0, done: false}
console.log(generator.next()) // {value: 2, done: false}
console.log(generator.next()) // {value: 4, done: false}
console.log(generator.next()) // {value: 6, done: false}

console.log(makeRangeIterator().next()) // {value: 0, done: false}
console.log(makeRangeIterator().next()) // {value: 0, done: false}
```

보시면 아시겠지만 `return` 값이 출력되는게 아니라 `yield`값이 출력되는 것을 볼 수 있습니다. 즉 위에서 설명했듯이 `next()` 메소드를 수행하면 `yield` 키워드를 만날 때 까지 수행하며 이를 반환합니다.</br>
그리고 생성자 함수는 매번 새로운 생성자를 반환하기에 `makeRangeIterator().next()`를 호출하면 처음 `iterator`만 반환되는 것은 당연할 것입니다.

## 3. yield\* 표현식

**`yield\*` 표현식**은 다른 `generator` 또는 이터러블(iterable) 객체에 yield를 위임할 때 사용됩니다.

다음 코드는, `next()` 호출을 통해 `g1()`으로부터 `yield`되는 값을 `g2()`에서 `yield`되는 것처럼 만듭니다.

```js{9}
function* g1() {
  yield 2;
  yield 3;
  yield 4;
}

function* g2() {
  yield 1;
  yield* g1();
  yield 5;
}

var iterator = g2();

console.log(iterator.next()); // { value: 1, done: false }
console.log(iterator.next()); // { value: 2, done: false }
console.log(iterator.next()); // { value: 3, done: false }
console.log(iterator.next()); // { value: 4, done: false }
console.log(iterator.next()); // { value: 5, done: false }
console.log(iterator.next()); // { value: undefined, done: true }
```

**generator**객체 말고도, `yield*`는 다른 반복 가능한 객체도 `yield` 할 수 있습니다. *eg. 배열, 문자열 또는 arguments 객체*

```js
function* g3() {
  yield* [1, 2];
  yield* "34";
  yield* Array.from(arguments);
}

var iterator = g3(5, 6);

console.log(iterator.next()); // { value: 1, done: false }
console.log(iterator.next()); // { value: 2, done: false }
console.log(iterator.next()); // { value: "3", done: false }
console.log(iterator.next()); // { value: "4", done: false }
console.log(iterator.next()); // { value: 5, done: false }
console.log(iterator.next()); // { value: 6, done: false }
console.log(iterator.next()); // { value: undefined, done: true }
```

## 4. 마치며

이번에 공부한 내용을 바탕으로 **Generator**와 **Iterator**에 대한 개념을 완벽히 이해하셨으면 좋겠습니다. 저는 이만 *react-saga*를 공부하러 가보도록 하겠습니다. ㅎ

### reference

* [반복기 및 생성기 :: MDN web docs](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Iterators_and_Generators)
* [function* :: MDN web docs](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/function*)
* [yield* :: MDN web docs](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/yield*)