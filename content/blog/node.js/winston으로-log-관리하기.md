---
title: Winston으로 Log 관리하기
date: 2020-01-05 16:01:39
category: node.js
---

> 이 문서는 [winston Git Repo](https://github.com/winstonjs/winston)의 README를 번역한 문서입니다. 맨 마지막에 실제 제가 작성한 sample 코드가 있으니 그대로 사용하는 것도 추천합니다.

## winston이란

winston은 simple하고 universal한 logging 라이브러리로 다중전송을 지원합니다. 각 winston logger에는 여러 레벨로 구성된 다중 전송이 있을 수 있습니다. 예를 들어 오류 로그를 데이터비이스와 콘솔 또는 로컬 파일에 동시에 저장, 출력할 수 있습니다.

winston은 logging process 일부를 분리하여 더 유연하고 확장 가능하도록 하는 것을 목표로 합니다. 로그 포멧 & 레벨을 유연하게 지원하고, API가 transport logging(즉, 로그 저장/색인화 방법, 사용자 지정 전송 추가)을 분리하였습니다.

## Usage

`winston`을 사용하는 방법은 자신의 logger를 만드는 것 입니다. 이는 `winston.createLogger`를 이용해서 쉽게 만들 수 있습니다.

```javascript
const winston = require('winston')

const logger = winston.createLogger({
  level: 'info',
  format: winston.format.json(),
  defaultMeta: { service: 'user-serice' },
  transports: [
    // - 'error' level의 모든 logs는 'error.log' 파일에 기록합니다.
    // - 'info' level의 모든 logs는 'conbined.log' 파일에 기록합니다.
    new winston.transports.File({ filename: 'error.log', level: 'error' }),
    new winston.transports.File({ filename: 'conbined.log ' }),
  ],
})

// production 상태가 아니라면 `console`에 다음과 같은 format으로 출력합니다.
// ${info.level}: ${info.message} JSON.stringify{{ ...rest }}
//

if (process.env.NODE_ENV !== 'production') {
  logger.add(
    new winston.transports.console({
      format: winston.format.simple(),
    })
  )
}
```

## Logging

`winston`의 Logging level은 [RFC5454](https://tools.ietf.org/html/rfc5424)에서 정한 심각한 정도에 따라 분류됩니다.

```javascript
const levels = {
  error: 0,
  warn: 1,
  info: 2,
  http: 3,
  verbose: 4,
  debug: 5,
  silly: 6,
}
```

### Create your own Logger

`winston.createLogger`를 이용해서 logger를 생성할 수 있습니다.

```javascript
const logger = winston.createLogger({
  transports: [
    new winston.transports.Console(),
    new winston.transports.File({ filename: 'conbinded.log' }),
  ],
})
```

logger's parameters:

|     Name      |           Default           |                             Description                             |
| :-----------: | :-------------------------: | :-----------------------------------------------------------------: |
|    `level`    |           `info`            |         `info.level`보다 작거나 동등한 레벨을 Logging한다.          |
|   `levels`    | `winston.config.npm.levels` |              Levels(colors)는 log 우선순위를 보여준다.              |
|   `format`    |    `winston.format.json`    |                         `info` 메시지 포멧                          |
| `transports`  |   `[]` (_No transports_)    |          `info` 메시지의 logging targets를 설정할 수 있다.          |
| `exitOnError` |           `true`            | 만약 false라면 exceptions을 처리할 때 process.exit를 하지 않습니다. |
|   `silent`    |           `false`           |                만약 true라면 모든 logs가 억제됩니다.                |

`createLogger`에서 제공된 levels는 반환된 `logger`의 methods로 정의되어 있습니다.

```javascript
// Logging
logger.log({
  level: 'info',
  message: 'Hello distributed log files!',
})

logger.info('Hello again distributed logs')
```

`logger`의 transports들을 추가하거나 삭제할 수 도 있습니다.

```javascript
const files = new winston.transports.File({ filenmae: 'conbined.log' })
const console = new winston.transports.console()

logger
  .clear() // Remove all transports
  .add(console) // Add console transports
  .add(files) // Add file transport
  .remove(console) // Remove console transport
```

`winston.Logger`는 `configure` methods를 사용해서 이전에 설정한 configure를 reconfigure 할 수 있습니다.

```javascript
const logger = winston.createLogger({
  level: 'info',
  transports: [
    new winston.transports.Console(),
    new winston.transports.File({ filename: 'combined.log' }),
  ],
})
// 이전의 configuration을 새로운 configuration으로 교체합니다.
const DailyRotatFile = require('winston-daily-rotate-file')
logger.confire({
  level: 'verbose',
  transports: [new DailyRotateFile(opts)],
})
```

### Creating child loggers

기존에 존재하는 loggers에 metadata를 override 함으로써 자식로거를 생성할 수 있습니다.

```javascript
const logger = winston.createLogger({
  transports: [new winston.transports.Console()],
})

const childLogger = logger.child({ requestId: '451' })
```

### Streams, `ObjectMode`, and `info` objects

`winston`에서 `Logger`와 `Transport` 인스턴스들은 `[objectMode](https://nodejs.org/api/stream.html#stream_object_mode)`로 취급됩니다.

지정된 형식으로 제공되는 `info`의 매개 변수는 하나의 로그 메세지를 나타내며 mutable 특성을 지닙니다. 그리고 객체는 항상 `level`과 `message` 속성을 가져야합니다.

```javascript
const info = {
  level: 'info', // logging message의 레벨
  message: 'Hey! Log something?', // 로그에 찍힌 메세지 설명
}
```

**level과 message** 이외의 속성들은 모두 "`meta`"로 간주됩니다.

```javascript
const { level, message, ...meta } = info
```

`logform`에 있는 몇몇의 속성들은 자체적으로 추가적인 속성들을 가집니다.

|  Property   | Format added by |               Description               |
| :---------: | :-------------: | :-------------------------------------: |
|   `splat`   |    `splat()`    | `%d %s`스타일 메시지에 대한 문자열 간격 |
| `timestamp` |  `timestamp()`  |        메시지를 수신한 timestamp        |
|   `label`   |    `label()`    |      각 메시지에 대한 커스텀 라벨       |
|    `ms`     |     `ms()`      |  이전 로그 메시지 이후 경과된 시간(ms)  |

사용자로써 원하는 속성을 자유롭게 추가할 수 있습니다. - _내부 상태는 `symbol` 속성에 의해 유지됩니다._

- `Symbol.for('level')` **(READ-ONLY)** : `level` 속성과 동일합니다. **모든 코드에서 immutable로 취급됩니다.**
- `Symbol.for('message')` : "finalizing formats"에 의해 설정된 전체 문자열 메시지
  - `json`
  - `logstash`
  - `printf`
  - `prettyPrint`
  - `simple`
- `Symbol.for('splat')` : 추가적인 문자열 보간 인수. _`splat()` 포멧으로만 사용된다._

이러한 Symbol들은 `triple-beam`이라는 패키지에 들있습니다. 그래서 `logform` 사용자들은 이를 이용해 다음과 같이 symbol들을 사용할 수 있습니다.

```javascript
const { LEVEL, MESSAGE, SPLAT } = require('triple-beam')

console.log(LEVEL === Symbol.for('level'))
// true

console.log(MESSAGE === Symbol.for('message'))
// true

console.log(SPLAT === Symbol.for('splat'))
// true
```

> **NOTE:** `meta` 객체에 있는 `{ message }` 속성은 이미 제공된 `msg` 속성에 자동으로 연결됩니다. 예를들어 아래의 'world'는 'hello'에 연결됩니다.:
>
> ```javascript
> logger.log('error', 'hello', { message: 'world' })
> logger.info('hello', { message: 'world' })
> ```

## Formats

`winston`에서의 Formats은 `winston.format`으로 접근할 수 있습니다. 이것은 `winston`에서 여러 개로 분리된 모듈중에 하나인 `logform`에 있습니다. 이것은 유연하게 format을 작성할 수 있게 하는 것을 도와줍니다.
현재 `node` 버전의 template 문자열은 매우 성능이 뛰어나며 사용자 포멧을 권장합니다. 만약 로그의 format을 지정하고 싶다면 `winston.format.printf`를 사용하시면 됩니다.

```javascript
const { createLogger, format, transports } = require('winston')
const { combine, timestamp, label, printf } = format

const myFormat = printf(({ level, message, label, timestamp }) => {
  return `${timestamp} [${label}] ${level}: ${message}`
})

const logger = createLogger({
  format: combine(label({ label: 'right meow!' }), timestamp(), myFormat),
  transports: [new transports.Console()],
})
```

### Combining formats

여러 format들은 `format.combine`을 통해서 하나의 format으로 합칠 수 있습니다. `format.combine`은 `opts`가 없으므로 이전에 만들어진 결합된 format 반환만 합니다.

```javascript
const { createLogger, format, transports } = require('winston')
const { combine, timestamp, label, prettyPrint } = format

const logger = createLogger({
  format: combine(label({ label: 'right meow!' }), timestamp(), prettyPrint()),
  transports: [new transports.Console()],
})

logger.log({
  level: 'info',
  message: 'What time is the testing at?',
})
// Outputs:
// { level: 'info',
//   message: 'What time is the testing at?',
//   label: 'right meow!',
//   timestamp: '2017-09-30T03:57:26.875Z' }
```

### String interpolation

`log` method는 `util.format`을 통하여 문자열 보간을 제공합니다. **이것은 `format.splat()`.**을 사용해야만 합니다.
아래는 `format.splat`을 사용하여 메시지의 문자열 보간법으로 형식을 정의한 후 `format.simple`을 사용하여 전체 정보 메시지를 직렬화하는 예 입니다.

```javascript
const { createLogger, format, transports } = require('winston')
const logger = createLogger({
  format: format.combine(format.splat(), format.simple()),
  transports: [new transports.Console()],
})

// info: test message my string {}
logger.log('info', 'test message %s', 'my string')

// info: test message 123 {}
logger.log('info', 'test message %d', 123)

// info: test message first second {number: 123}
logger.log('info', 'test message %s, %s', 'first', 'second', { number: 123 })
```

### Filtering `info` Objects

log를 기록할 때 간단한 false값을 반환함으로써 `info` 객체를 필터링하길 원한다면 다음과 같이 사용하면 됩니다.

```javascript
const { createLogger, format, transports } = require('winston')

// Ignore log messages if they have { private: true }
const ignorePrivate = format((info, opts) => {
  if (info.private) {
    return false
  }
  return info
})

const logger = createLogger({
  format: format.combine(ignorePrivate(), format.json()),
  transports: [new transports.Console()],
})

// Outputs: {"level":"error","message":"Public error to share"}
logger.log({
  level: 'error',
  message: 'Public error to share',
})

// Messages with { private: true } will not be written when logged.
logger.log({
  private: true,
  level: 'error',
  message: 'This is super secret - hide it.',
})
```

```javascript
const { format } = require('winston')
const { combine, timestamp, label } = format

const willNeverThrow = format.combine(
  format(info => {
    return false
  })(), // Ignores everything
  format(info => {
    throw new Error('Never reached')
  })()
)
```

### Creating custom formats

_생략_

## Logging Levels

`winston`에서의 logging level은 [RFC5424](https://tools.ietf.org/html/rfc5424)에서 정의한 순서를 따르고 있습니다. :_레벨들은 가장 중요한 것부터 덜 중요한 것 까지 오른차순으로 정의되어 있습니다._
각 레벨에는 특정 숫자의 우선순위가 부여되며 우선순위가 높을수록 메시지는 더 중요한 것으로 간주됩니다. 예를 들어, `syslog`의 레벨은 0부터 7까지 우선순위가 지정됩니다.

```json
{
  "emerg": 0,
  "alert": 1,
  "crit": 2,
  "error": 3,
  "warning": 4,
  "notice": 5,
  "info": 6,
  "debug": 7
}
```

유사하게 `npm` 로그 레벨은 0 부터 9까지 입니다.

```json
{
  "error": 0,
  "warn": 1,
  "info": 2,
  "http": 3,
  "verbose": 4,
  "debug": 5,
  "silly": 6
}
```

`winston`에서 사용할 level을 정의하지 않으면 위의 `npm` level을 사용하게 됩니다.

### Using Logging Levels

로그 메시지 level 설정은 두가지 방법으로 수행할 수 있습니다. logging level을 보여주는 문자열을 log() 함수를 통하여 보내거나 모든 winston Logger에 정의된 level 정의 함수를 사용하면 됩니다.

```javascript
//
// Any logger instance
//
logger.log('silly', "127.0.0.1 - there's no place like home")
logger.log('debug', "127.0.0.1 - there's no place like home")
logger.log('verbose', "127.0.0.1 - there's no place like home")
logger.log('info', "127.0.0.1 - there's no place like home")
logger.log('warn', "127.0.0.1 - there's no place like home")
logger.log('error', "127.0.0.1 - there's no place like home")
logger.info("127.0.0.1 - there's no place like home")
logger.warn("127.0.0.1 - there's no place like home")
logger.error("127.0.0.1 - there's no place like home")

//
// Default logger
//
winston.log('info', "127.0.0.1 - there's no place like home")
winston.info("127.0.0.1 - there's no place like home")
```

_생략_

### Using Custom Logging Levels

추가적으로 `npm`, `syslog`, `cli` 레벨들을 `winston`에서 미리 정의할 수 있으며 또한 자신이 만든 것을 선택할 수도 있습니다.

```javascript
const myCustomLevels = {
  levels: {
    foo: 0,
    bar: 1,
    baz: 2,
    foobar: 3,
  },
  colors: {
    foo: 'blue',
    bar: 'green',
    baz: 'yellow',
    foobar: 'red',
  },
}

const customLevelLogger = winston.createLogger({
  levels: myCustomLevels.levels,
})

customLevelLogger.foobar('some foobar level-ed message')
```

이 데이터 구조에는 약간의 반복이 있지만 색상을 사용하지 않으려는 경우 간단히 캡슐화를 사용할 수 있습니다. 색을 가지려면 로거 자체에 레벨을 전달하는 것 외에 다음과 같이 Winston에게 알려야 합니다.

```javascript
winston.addColors(myCustomLevels.colors)
```

이렇게 하면 컬러화 폼펙터를 사용하여 로거가 사용자 지정 레벨의 log를 적절하게 색칠하고 스타일을 설정할 수 있습니다.
추가적으로 배경색과 폰트 스타일을 변경시킬수도 있습니다.

```javascript
baz: 'italic yellow',
foobar: 'bold red cyanBG'
```

가능한 옵션들은 다음과 같습니다.

- Font styles: `bold`, `dim`, `italic`, `underline`, `inverse`, `hidden`, `strikethrough`.

- Font foreground colors: `black`, `red`, `green`, `yellow`, `blue`, `magenta`, `cyan`, `white`, `gray`, `grey`.

- Background colors: `blackBG`, `redBG`, `greenBG`, `yellowBG`, `blueBG`, `magentaBG`, `cyanBG`, `whiteBG`.

### Colorizing Standard logging levels

표준 logging level에 색을 추가하기 위해서는 다음과 같이 작업해야 합니다.

```javascript
winston.format.combine(winston.format.colorize(), winston.format.json())
```

## 마무리

여기까지 하겠습니다. ㅋㅋ 너무 힘드네요. 하지만 이정도만 알아도 충분할 것으로 예상됩니다. 더욱 알고싶으신 분들은 밑의 winston github repo를 참고하시면 됩니다.

추가로 제가 작성한 코드입니다.

```javascript
// src/logger.ts
import { createLogger, format, transports } from 'winston'

const { combine, timestamp, prettyPrint, printf, label } = format

const myFormat = printf(({ level, message, label, timestamp }) => {
  return `${timestamp} [${label}] ${level}: ${message}`
})

const options = {
  file: {
    level: 'info',
    filename: `logs/test.log`,
    handleExceptions: true,
    json: false,
    maxsize: 5242880, // 5MB
    maxFiles: 5,
    colorize: false,
    format: combine(label({ label: 'express' }), timestamp(), myFormat),
  },
  console: {
    level: 'debug',
    handleExceptions: true,
    json: false,
    colorize: true,
    format: combine(label({ label: 'express' }), timestamp(), myFormat),
  },
}

const logger = createLogger({
  level: 'info',
  format: combine(format.json(), timestamp(), prettyPrint()),
  transports: [new transports.File(options.file)],
})

if (process.env.NODE_ENV !== 'production') {
  logger.add(new transports.Console(options.console))
}
export default logger
```

```javascript
// src/app.ts
import * as express from 'express'
import * as http from 'http'
import logger from './logger'

// Shutdown codes
const errShutdown = async (server: http.Server, err?: Error) => {
  logger.error(`Stopping server with error: ${err}`)
  await server.close()
  process.exit(1)
}

async function runServer() {
  const app = express()
  const server = app.listen(5000, () => {
    logger.info('Server app listening on port 5000!')
  })
  try {
  } catch (e) {
    errShutdown(server, e)
    throw e
  }
}
runServer()
  .then(() => {
    logger.info('run succesfully')
  })
  .catch((ex: Error) => {
    logger.error('Unable run:', ex)
  })
```

### Reference

- [NodeJS 인기있는 Logging 모듈 Winston](https://basketdeveloper.tistory.com/42)
- [winston github repo](https://github.com/winstonjs/winston)
