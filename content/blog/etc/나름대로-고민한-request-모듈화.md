---
title: 나름대로 고민한 request 모듈화
date: 2019-09-30 12:09:71
category: etc
---

며칠 전 Spring 서버 개발자 친구의 코드를 보고 잠깐 고민에 빠진적이 있습니다. 저는 보통 request 함수를 따로 빼서 요청하는 값에 따라 구분해서 사용하는데 이 개발자 친구는 완전히 모듈화 하여 사용했습니다. 예를 들어보자면 저는 다음과 같이 작성했다면

```sheel
utils
|--header
requests
|--book-info
|--book
```

제 친구는 이렇게 작성하더군요.

```shell
HttpRequest
|--HttpRequestStartLine
|--HttpRequestHeader
|--HttpRequestBody
|--HttpRequest
```

누가 봐도 밑에 있는게 더 보기 좋죠? ㅎㅎ

저는 header 파일에서 token들을 넣고 requests의 함수에서 불러와 header를 setting하여 사용하였습니다. 즉 기능별로 모듈화를 하였죠. 

하지만 밑의 경우는 Request부분 모두를 하나씩 분리해서 사용했습니다.

이것을 보고 제가 가다듬어 사용한다면 조금 멋지게 모듈화를 할 수 있겠다는 생각이 들어 시도를 해보았습니다.

당연히 이렇게 나눌때는 TypeScript를 사용하는게 좀 더 깔끔하기에 사용하였습니다.

> Explore에 대한 자비는 없습니다.

Refectoring~

코드가 깔끔하지 않아 계속 수정하고 있습니다.

좋은 코드를 짜는 건 어렵네요.

## Http Request Module

**Example**

```javascript
import HttpRequest from 'src/module/http-request'

export const getBooks = async () => {
  const request = new HttpRequest('/', 'GET', {
    params: { name: 'hello' },
    headers: { Authentication: 'token' },
  })
  request.setErrorHandler(alert)
  const response = await request.sendData()
  console.log(response)
}

export const postBook = async () => {
  const request = new HttpRequest('/', 'POST', { body: { b1: 'book1' } })
  request.setErrorHandler(alert)
  const response = await request.sendData()
  console.log(response)
}

function alert(statusCode: number) {
  console.log(`error ${statusCode}`)
}
```

**http request class**

```javascript
import HttpRequestBody from './http-request-body'
import HttpRequestHeader from './http-request-header'

type THttpMethod = 'POST' | 'GET' | 'DELETE' | 'UPDATE'
interface IOptions {
  headers?: Object
  params?: Object
  body?: Object
}

class HttpRequest {
  private method: THttpMethod
  private header: HttpRequestHeader
  private body: HttpRequestBody
  private url: string
  private xhttp: XMLHttpRequest
  private errorHandler: Function | null
  private baseUrl: string
  private query: string

  constructor(
    requestUrl: string,
    httpMethod: THttpMethod,
    options?: IOptions,
    errorFunction?: Function,
  ) {
    this.baseUrl =
      process.env.NODE_ENV === 'development' ? 'http://localhost:4000' : 'http://google.com'
    this.url = requestUrl
    this.method = httpMethod
    this.query = ''
    if (options) {
      this.setOptions(options)
    } else {
      this.header = new HttpRequestHeader()
      this.body = new HttpRequestBody()
    }
    this.xhttp = new XMLHttpRequest()
    this.errorHandler = errorFunction || null
  }
  private setOptions(options: Object) {
    this.header = new HttpRequestHeader(options['headers'])
    this.body = new HttpRequestBody(options['body'])
    const params = options['params']
    return (this.query = params ? this.parseParams(params) : '')
  }
  private parseParams(params: Object) {
    const queryString = Object.keys(params)
      .map(k => encodeURIComponent(k) + '=' + encodeURIComponent(params[k]))
      .join('&')
    return '?' + queryString
  }
  public setBaseUrl(baseUrl: string) {
    this.baseUrl = baseUrl
  }
  set httpMethod(httpRequestMethod: THttpMethod) {
    this.method = httpRequestMethod
  }
  get httpMethod(): THttpMethod {
    return this.method
  }
  set requestUrl(url: string) {
    this.url = url
  }
  get requestUrl(): string {
    return this.url
  }
  public setHeader(key: string, value: string) {
    this.header.setHeader(key, value)
  }
  public setBody(key: string, value: object | string) {
    this.body.setBody(key, value)
  }
  public setErrorHandler(errorHandler: Function) {
    this.errorHandler = errorHandler
  }
  private setHeaderAtXMLHttpRequest() {
    this.xhttp.setRequestHeader('Content-Type', 'application/json')
    const headers = this.header.getHeader()
    for (let key in headers) {
      this.xhttp.setRequestHeader(key, headers[key])
    }
  }
  private requestCallBack(resolve: any, reject: any) {
    this.xhttp.onreadystatechange = () => {
      const { readyState, status, response } = this.xhttp
      if (readyState === XMLHttpRequest.DONE) {
        if (status === 200) {
          resolve(response)
        } else {
          this.errorHandler ? this.errorHandler(status) : null
          reject('Promise Error')
        }
      }
    }
  }
  public sendData(): Promise<JSON | Error | undefined | null> {
    const { xhttp, method, baseUrl, url, body, query } = this
    xhttp.open(method, baseUrl + url + query, true)
    this.setHeaderAtXMLHttpRequest()
    return new Promise((resolve, reject) => {
      xhttp.send(body.getBody())
      this.requestCallBack(resolve, reject)
    })
  }
}

export default HttpRequest

```

<https://github.com/koomg9599/react-request-module>