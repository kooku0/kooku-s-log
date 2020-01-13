---
title: Winston으로 Log 관리하기
date: 2020-01-05 16:01:39
category: node.js
---

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

## Create your own Logger

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
| `transports`  |    [] (_No transports_)     |          `info` 메시지의 logging targets를 설정할 수 있다.          |
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

### Reference

- [NodeJS 인기있는 Logging 모듈 Winston :: 농구하는 개발자](https://basketdeveloper.tistory.com/42)
- [winston github repo](https://github.com/winstonjs/winston)
