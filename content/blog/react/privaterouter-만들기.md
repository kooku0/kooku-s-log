---
title: PrivateRouter 만들기
date: 2020-03-04 14:03:70
category: react
---

로그인이 들어가는 웹사이트의 경우 PrivateRouter가 필요합니다. 만약 로그인이 되어 있지 않다면 `signin` 페이지로 redirect 해줘야 하는데 어떻게 구현을 하면 될까요?

이전 프로젝트에서 제가 구현한 방법은 다음과 같습니다.

```js
// pages/mypage.jsx

import React from 'react'
import { useSelector } from 'react-redux'
import { Redirect } from 'react-router-dom'

const MyPage = () => {
	const { isLogin } = useSelector(state => state.login)
	return {
        <div>
        {!isLogin && <Redirect to='/'/>}
    	...
        <div>
    }
}
```

위와 같이 모든 페이지에 로그인이 되어 있지 않다면 홈으로 redirect하는 코드를 삽입하였습니다.

하지만 다음과 같이 코드를 바꿀 수 있을 것 같습니다.

```js
// App.js

import React, { Component } from 'react'
import { BowserRouter as Router, Redirect, Route, Switch } from 'react-router-dom'
import PrivateRoute from './components/PrivateRouter'
import SignIn from './pages/SignIn'
import MyPage from './pages/MyPage'
import { PAGE_PATHS } from './constants'

export default class App extends Component {
    render() {
        return (
            <Router>
            	<Switch>
            		<Route path={PAGE_PATHS.SIGNIN} component={SignIn} />
            		<PrivateRoute
                        path={PAGE_PATHS.MY_PAGE}
                        redirectTo={PAGE_PATHS.SIGNIN}
                        component={MyPage}
                      />
	            </Switch>
            </Router>
        )
    }
}
```

```js
// components/PrivateRouter.jsx

import React from 'react'
import { useSelector } from 'react-redux'
import { Redirect, Route } from 'react-router-dom'

export default ({ component: Component, redirectTo, authStore, path, exact }) => {
	const { isLogin } = useSelector(state => state.login)
	return {
		<Route
        	Path={path}
		    exact={exact}
    		render={(props) =>
            	isLogin() ? (
                	<Component {..props} />
    			) : (
                    <Redirect
                    	to={redirectTo}
                    />
                )
            }
    }
}
```

이렇게 코드를 적으면 해당 페이지에서 작업을 처리하는데까지 가지 않고 바로 redirect 해주고, 코드도 훨씬 깔끔해 집니다. PrivateRouter라는 곳에서 다 처리해 주므로 중복하여 코드를 작성해야하는 부분도 없어지겠지요. 그리고 Redirect가 어디에서 이루어지는지도 쉽게 확인하고 할 수 있을 겁니다.

