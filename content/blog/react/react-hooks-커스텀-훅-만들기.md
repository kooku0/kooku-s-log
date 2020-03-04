---
title: react-hooks 커스텀 훅 만들기
date: 2019-09-08 20:09:22
category: react
---

솔직히 이 글을 읽는 것 보다는 공식문서를 읽는게 더 좋습니다.

참고할만한 링크는 다음과 같습니다.

* https://ko.reactjs.org/docs/hooks-overview.html#building-your-own-hooks
* https://ko.reactjs.org/docs/hooks-rules.html#eslint-plugin

저는 react를 이용해서 프로젝트를 하면 거의 대부분 react-hooks를 이용합니다. 그 이유는 편하고, 모듈화를 할 수 있기 때문입니다.

특히 모듈화가 대박인데, 커스텀 훅을 만들어 사용하면 graceful한 code를 만들 수 있습니다.

이번에는 예시로 input component에 custom hook을 붙히는 작업을 해보겠습니다.

먼저 custom hook에 대해서 몇 가지 알아야 하는 내용이 있습니다. 이 내용은 공식문서을 참조하였으니 더 알고 싶으신 분들은 [공식문서](https://ko.reactjs.org/docs/hooks-overview.html#building-your-own-hooks)를 확인하시면 될 것 같습니다.

> Custom Hook은 기능이라기보다는 컨벤션(convention)에 가깝습니다. 이름이 ”`use`“로 시작하고, 안에서 다른 Hook을 호출한다면 그 함수를 custom Hook이라고 부를 수 있습니다. `useSomething`이라는 네이밍 컨벤션은 linter 플러그인이 Hook을 인식하고 버그를 찾을 수 있게 해줍니다.

*즉, 커스텀 훅은 네이밍을 `use~` 로 작성해야한다는 것 입니다. 이것만 주의하시면 될 것 같습니다.*

*위의 공식문서에서 언급한 것 처럼 [`eslint-plugin-react-hooks`](https://www.npmjs.com/package/eslint-plugin-react-hooks)는 하시면 좋습니다!*

먼저 input component를 만들어 보겠습니다.

```javascript
import React from 'react'

function App() {
  return <input className="input_box" /> 
}

export default App
```

이제 custom hook을 만들어보겠습니다.

```javascript
import { useState } from 'react'

const useInput = (initValue) => {

  const [value, setValue] = useState(initValue)
  
  const inputEvent = {
    value: value,
    onChange: ({ target }) => {
      setValue(target.value)
    },
    onKeyDown: ({ key }) => {
      switch (key) {
        case 'ArrowUp':
          alert('ArrowUp')
          break
        case 'ArrowDown':
          alert('ArrowDown')
          break
        case 'Enter':
          alert('Enter')
          break
        default:
          break
      }
    },
    onClick: () => {
      alert('Click')
    },
  }
  return { inputEvent }
}

export default useInput
```

입력시 useState를 통해 value가 바뀌게 하였으며, inputEvent까지 예시로 몇개 적어주었습니다.

커스텀 훅을 만들었으니 이제 input component에 붙여보겠습니다.

```javascript
import React from 'react'
import useInput from 'hooks/use-input'

function App() {
  const { inputEvent } = useInput('');
  return <input className="input_box" {...inputEvent} /> 
}

export default App
```

react hooks를 사용하지 않았다면 setState를 이용했어야하는 작업이 모듈화를 통해 훨씬 간결해지고 reuseable해졌습니다.

만약 react-hooks를 사용하지 않고 state를 사용하면 어떻게 작성이 될까요?

```javascript
import React from 'react'

function App() {
  state={
      value: ''
  }
  handleChange = ({ target }) => {
    this.setState({ value: target.value })
  }
  return (
    <input
      className="input_box"
      value={this.state.value}
	  onChange={this.handleChange}
    /> 
  )
}

export default App
```

위에 보다 훨씬 양이 많은 걸 확인 할 수 있습니다~, 코드 재사용도 불가능 하구요!

모두 react-hooks를 사랑합시다~ ㅎㅎ