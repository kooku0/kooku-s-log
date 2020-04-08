---
title: (ZeroCho 리액트 무료 강좌 01) React 기본
date: 2020-04-08 19:04:27
category: react
---



React.createClass -> Class -> Hooks

Hooks가 FaceBook이 미는 React의 표준이지만 이전 웹이 Class로 만들어 졌기 떄문에 두개를 다 다루겠다.

# 1. 리액트란 무엇인가? 왜 쓰는가?

<div align="center">사용자 인터페이스를 만들기 위한 JavaScript 라이브러리</div>
<div align="center"><small>- React 공식문서 -</small></div>
### 1) 사용자 경험

웹보다는 앱이 사용자 경험이 좋은데, 웹이지만 앱처럼 만들어 주었다.

### 2) 데이터-화면 일치

Data와 화면을 일치시키는게 힘들기 때문에 그걸 React가 쉽게 해준다.

### 3) 재사용 컴포넌트

컴포넌트화 시켜서 중복을 없앤다.  (유지보수하기가 쉽다.)

# 2. 첫 리액트 컴포넌트

> 웹팩은 React를 JS 파일로 만들어준다.
>
> 쪼개진 JS 파일을 HTML이 실행킬 수 있는 JS파일로 합친다.
>

```html
<html>
    <head>
        <script crossorigin src="https://unpkg.com/react@16/umd/react.development.js"></script>
		<script crossorigin src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"></script>
    </head>
    <body>
        <div id="root"></div> 
        <!-- <div id="root"><button>Like</button></div> -->
        </button>
        <script>
            const e = React.createElement;
            class LikeButton extends React.Component { // React.Component를 상속해온다.
                constructor(props) { // class가 실행될 때 처음 실행된다.
                    super(props);
                }
                render() {
                    return e('button', null, 'Like'); // <button>Like</button>
                }
            }
        </script>
        <script> // React.Component로 구현해놓은 것을 ReactDOM이 웹에 랜더링하는 역할을 한다.
            ReactDOM.render(e(LikeButton), document.querySelector('#root'));
        </script>
    </body>
</html>
```

HTML 파일이 실행되면 위에서 부터 아래로, 왼쪽에서 부터 오른쪽으로 순차적으로 실행된다.

1. react script 불러옴
2. react-dom script 불러옴
3. div 태그 그림
4. react가 like component를 정의
5. react-dom이 react가 만든 like component를 root div에 그림

React는 component를 랜더링할 root라는게 필요하다.

# 3. HTML 속성과 상태(state)

바뀔 여지가 있는 부분이 상태(state)이다.

상태가 바뀌면 화면이 저절로 바뀐다.

```html
<html>
    <head>
        <script crossorigin src="https://unpkg.com/react@16/umd/react.development.js"></script>
		<script crossorigin src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"></script>
    </head>
    <body>
        <div id="root"></div> 
        <!-- <div id="root"><button>Like</button></div> -->
        </button>
        <script>
            const e = React.createElement;
            class LikeButton extends React.Component { // React.Component를 상속해온다.
                constructor(props) { // class가 실행될 때 처음 실행된다.
                    super(props);
                    this.state = {
                        liked: false,
					}
                }
                render() {
                    return e('button',
                          {onClick:() => {this.setState({liked: true})}, type: 'submit'},
                          this.state.liked === true ? 'Liked' : 'Like'); 
                    // <button onclick="() => {console.log('clicked')}" type="submit">Like</button>
                    // $('button').text('Liked');
                }
            }
        </script>
        <script> // React.Component로 구현해놓은 것을 ReactDOM이 웹에 랜더링하는 역할을 한다.
            ReactDOM.render(e(LikeButton), document.querySelector('#root'));
        </script>
    </body>
</html>
```

# 4. JSX와 바벨(babel)

JS가 JSX문법을 지원하지 않으므로 babel을 사용해야한다.


```html
<html>
    <head>
        <script crossorigin src="https://unpkg.com/react@16/umd/react.development.js"></script>
		<script crossorigin src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"></script>
        <script src="https://unpkg.com/babel-standalone@6/babel.min.js"></script>
    </head>
    <body>
        <div id="root"></div> 
        <!-- <div id="root"><button>Like</button></div> -->
        </button>
        <script type="text/babel">
            const e = React.createElement;
            class LikeButton extends React.Component { // React.Component를 상속해온다.
                constructor(props) { // class가 실행될 때 처음 실행된다.
                    super(props);
                    this.state = {
                        liked: false,
					}
                }
                render() {
                    return <button type="submit" onClick={() => {this.setState({ liked: true})}}>
                    	{this.state.liked === true ? 'Liked' : 'Like' }
            		</button>;
                    // JSX (JS + XML)
                    //e(
                    // 	'button',
                    // 	{onClick:() => {this.setState({liked: true})}, type: 'submit'},
                    // 	this.state.liked === true ? 'Liked' : 'Like');
                }
            }
        </script>
        <script type="text/babel">
            ReactDOM.render(<LikeButton />, document.querySelector('#root'));
        </script>
    </body>
</html>
```

JSX는 닫는 괄호를 꼭 해줘야 한다. (엄격하다)

babel이 JSX를 createElement로 바꿔준다.