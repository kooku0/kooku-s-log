---
title: 나름대로 고민한 request 모듈화
date: 2019-09-30 12:09:71
category: react
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

### 아직 작성중..

<https://github.com/koomg9599/react-request-module>

