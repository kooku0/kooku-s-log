---
title: cors
date: 2019-09-02 17:09:81
category: web
---

## Overview

HTTP 요청은 기본적으로 Cross-site HTTP Requests가 가능합니다. 다시 말하면 `<img>` 태그로 다른 도메인의 이미지 파일을 가져오거나, `<link>` 태그로 다른 도메인의 css를 가져오거나, `<script>` 태그로 다른 도메인의 javascript 라이브러리를 가져오는 것이 모두 가능합니다. 하지만 `<script></script>` 로 둘러싸여 있는 스크립트에서 생성된 Cross-Site HTTP Requests는 Same Origin Policy를 적용 받기 때문에 Cross-Site HTTP Requests가 불가능합니다. 즉, **프로토콜**, **호스트명**, **포트**가 같아야만 요청이 가능합니다.

하지만 **AJAX**가 널리 사용되면서 `<script></script>`로 둘러싸여 있는 스크립트에서 생성되는 `XMLHttpRequest`에 대해서도 Cross-Site HTTP Requests가 가능해야 한다는 요구가 늘어나자 W3C에서 CORS라는 이름의 권고안이 나오게 되었습니다.

## Same-origin policy

**동일 출처 정책**은 문서나 스크립트가 다른 출처의 리소스와 통신하는 것을 제한하는 중요한 보안 방식입니다. 이것은 잠재적 악성 문서를 격리하여, 공격을 막는데 도움을 줍니다.

출처는 위에 기술했듯이 프로토콜, 포트, 호스트가 동일하면 각 페이지의 출처도 동일합니다.

## CORS 요청의 종류

CORS 요청은 Simple/Preflight, Credential/Non-Credential의 조합으로 4가지가 존재합니다. 브라우저가 요청 내용을 분석하여 4가지 방식 중 해당하는 방식으로 서버에 요청을 날리므로, 프로그래머가 목적에 맞는 방식을 선택하고 그 조건에 맞게 코딩해야 합니다.



### reference

* [CORS :: 개인적인공간](https://brownbears.tistory.com/336)

* [Same-origin policy :: MDN web docs](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy)