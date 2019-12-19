---
title: react-hooks 커스텀 훅 만들기
date: 2019-09-08 20:09:22
category: react
---

저는 react를 이용해서 프로젝트를 하면 거의 대부분 react-hooks를 이용합니다. 그 이유는 편하고, 모듈화를 할 수 있기 때문입니다.

특히 모듈화가 대박인데, 커스텀 훅을 만들어 사용하면 graceful한 code를 만들 수 있습니다.

이번에는 예시로 input component에 custom hook을 붙히는 작업을 해보겠습니다.

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

모두 react-hooks를 사랑합니다~ ㅎㅎ