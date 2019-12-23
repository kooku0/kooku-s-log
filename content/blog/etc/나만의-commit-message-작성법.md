---
title: 나만의 commit message 작성법
date: 2019-10-03 07:10:59
category: etc
---

Git을 사용하면서 Commit message를 적을때 항상 머뭇거리곤 합니다. 과연 잘 작성했는지 혹시라도 잘못 작성하여 쪽팔림을 당하진 않을까? 등등

영어능력이 떨어지는 저는 커밋 메세지 작성하는게 여간 힘든게 아니더군요.

그래서 이번에 저만의 커밋 메세지 작성법을 만들어보려고 합니다.

밑의 내용은 [좋은 git 커밋 케시지를 작성하기 위한 7가지 약속 :: TOAST Meetup!](https://meetup.toast.com/posts/106)에 적혀있는 커밋 메시지 작성법입니다. 아마 대부분의 분들이 이 내용에 따라 작성하고 있을 것이라 생각합니다.

### 좋은 git 커밋 메시지를 작성하기 위한 7가지 약속

> 이하 약속은 커밋 메시지를 `English`로 작성하는 경우에 최적화되어 있습니다. 한글 커밋 메시지를 작성하는 경우에는 더 유연하게 적용하셔도 좋을 것 같네요.

1. 제목과 본문을 한 줄 띄워 분리하기
2. 제목은 영문 기준 50자 이내로
3. 제목 첫글자를 대문자로
4. 제목 끝에 `.` 금지
5. 제목은 `명령조`로
6. 본문은 영문 기준 72자마다 줄 바꾸기
7. 본문은 `어떻게`보다 `무엇을`, `왜`에 맞춰 작성하기

# 나의 커밋 메시지 작성법

저는 조금 다른 방법을 적용해보려고 합니다.

참고한 곳은 다음과 같습니다.

* [Git 사용 규칙 - Git commit 메시지 :: h3ngss0](https://tttsss77.tistory.com/58)
* [Udacity Git Commit Message Style Guide :: udacity](https://udacity.github.io/git-styleguide)
* [style-git-commit-message :: slashsbin](https://github.com/slashsbin/styleguide-git-commit-message)

## 공통 규칙

1. **유형**은 **영어 or emoji**로, **제목**은 **한글**로 작성한다.
2. **메시지 본문**에 모든 **변경 사항을 상세히 작성**한다.

## 커밋 메시지 구성

모든 커밋 메시지는 다음과 같이 **세 영역으로 구성**되며, 각 영영은 **빈 줄로 분리**된다.

* 제목 줄
* 본문 (제목 만으로 표현이 가능할 때에는 생략 가능)
* 꼬리말 (관련 이슈가 없으면 생략 가능)

```
유형: 제목

본문

꼬리말
```

## 유형

유형들이 복합적으로 포함되어 있을 경우, 되도록 커밋을 분리한다. 분리가 어려운 경우 위 순서상 상위 항목의 유형으로 작성한다. (eg. 기능과 테스트가 모두 포함된 경우 기능으로 작성)

- **feat**: 기능 추가, 삭제, 변경(or ✨ emoji) - 제품 코드 수정 발생
- **fix**: 버그 수정(or 🚑 emoji) - 제품 코드 수정 발생
- **docs**: 문서 추가, 삭제, 변경(or 📚 emoji) - 코드 수정 없음
- **style**: 코드 형식, 정렬, 주석 등의 변경, eg; 세미콜론 추가(or 🎨 emoji) - 제품 코드 수정 발생, 하지만 동작에 영향을 주는 변경은 없음
- **refactor**: 코드 리펙토링, eg. renaming a variable(or 🚜 emoji) - 제품코드 수정 발생
- **test**: 테스트 코드 추가, 삭제, 변경 등(or 🔬 emoji) - 제품 코드 수정 없음. 테스트 코드에 관련된 모든 변경에 해당
- **etc**: 위에 해당하지 않는 모든 변경, eg. 빌드 스크립트 수정, 패키지 배포 설정 변경 - 코드 수정 없음

## 제목

1. 제목 줄은 **50자 내로 작성**한다.

2. 제목은 **개조식 구문으로 작성**한다.
3. 제목 줄은 **"유형: 제목"** 의 형식으로 작성한다.
4. 제목 뒤에 특수문자는 삽입하지 않습니다. 예) . ? !

예) "feat: 로그 기능 출력 기능 추가"

> **개조식 구문**
>
> 완전한 서술형으로 문장을 종결하는 것이 아니라 간결하고 요점적인 단어로 서술되는 문장형태로서, 내용을 길게 풀어서 표현하지 않고 중요하고 핵심적인 요소만 간추려 항목별로 나열하듯이 표현하는 방식
>
> \- *국립국어원*  \-

## 본문

1. 본문은 **한 줄 당 72자 내**로 작성한다.
2. 본문 내용은 양에 구애받지 않고 **최대한 상세히 작성**한다.
3. 본문 내용은 어떻게 변경했는지 보다 **무엇을 변경했는지** 또는 **왜 변경했는지**를 설명한다.

## 꼬리말

1. 꼬리말은 optional이고 **이슈 트래커 ID**를 작성한다.
2. 꼬리말은 **"유형: #이슈번호"** 형식으로 사용한다.
3. 여러개의 이슈번호를 적을때는 쉼표로 구분합니다.
4. 이슈 트래커 유형은 다음 중 하나를 사용한다.
   * **Fixes**: 이슈 수정중 (아직 해결되지 않은 경우)
   * **Resolves**: 이슈를 해결했을 때 사용
   * **Ref**: 참고할 이슈가 있을 때 사용
   * **Related to**: 해당 커밋에 관련된 이슈번호 (아직 해결되지 않은 경우)

예) `Fixes: #45` `Reloated to: #34, #23`

## 예시

```
feat: 패킷 송신 이벤트에 관련된 로그 출력 기능 추가

커밋에 대한 자세한 설명..

Resolves: #123
Ref: #456
Related to: #48, #45
```

## 커밋 메시지 작성시 사용할만한 Emoji

| Emoji | Raw Emoji Code     | Description                                                  |
| ----- | ------------------ | ------------------------------------------------------------ |
| 🎨     | `:art:`            | 코드의 **형식** / 구조를 개선 할 때                          |
| 📰     | `:newspaper:`      | **새 파일을** 만들 때                                        |
| 📝     | `:pencil:`         | **사소한 코드 또는 언어**를 변경할 때                        |
| 🐎     | `:racehorse:`      | **성능을** 향상시킬 때                                       |
| 📚     | `:books:`          | **문서를** 쓸 때                                             |
| 🐛     | `:bug:`            | **버그** reporting할 때, [`@FIXME`](https://github.com/slashsbin/styleguide-todo-grammar#bug-report)주석 태그 삽입 |
| 🚑     | `:ambulance:`      | **버그를** 고칠 때                                           |
| 🐧     | `:penguin:`        | **리눅스에서** 무언가를 고칠 때                              |
| 🍎     | `:apple:`          | **Mac OS에서** 무언가를 고칠 때                              |
| 🏁     | `:checkered_flag:` | **Windows에서** 무언가를 고칠 때                             |
| 🔥     | `:fire:`           | **코드 또는 파일 제거**할 때 , `@CHANGED`주석 태그와 함께    |
| 🚜     | `:tractor:`        | **파일 구조를 변경할** 때 . 🎨과 함께 사용                    |
| 🔨     | `:hammer:`         | 코드를 **리팩토링** 할 때                                    |
| ☔️     | `:umbrella:`       | **테스트를** 추가 할 때                                      |
| 🔬     | `:microscope:`     | **코드 범위를** 추가 할 때                                   |
| 💚     | `:green_heart:`    | **CI** 빌드를 고칠 때                                        |
| 🔒     | `:lock:`           | **보안을** 다룰 때                                           |
| ⬆️     | `:arrow_up:`       | **종속성을** 업그레이드 할 때                                |
| ⬇️     | `:arrow_down:`     | **종속성을** 다운 그레이드 할 때                             |
| ⏩     | `:fast_forward:`   | 이전 버전 / 지점에서 **기능**을 **전달할** 때                |
| ⏪     | `:rewind:`         | 최신 버전 / 지점에서 **기능**을 **백 포트** 할 때            |
| 👕     | `:shirt:`          | **linter** / strict / deprecation 경고를 제거 할 때          |
| 💄     | `:lipstick:`       | **UI** / style 개선시                                        |
| ♿️     | `:wheelchair:`     | **접근성을** 향상시킬 때                                     |
| 🚧     | `:construction:`   | **WIP** (진행중인 작업)에 커밋, `@REVIEW`주석 태그와 함께 사용 |
| 💎     | `:gem:`            | New **Release**                                              |
| 🔖     | `:bookmark:`       | 버전 **태그**                                                |
| 🎉     | `:tada:`           | **Initial** Commit                                           |
| 🔈     | `:speaker:`        | **로깅을** 추가 할 때                                        |
| 🔇     | `:mute:`           | **로깅을** 줄일 때                                           |
| ✨     | `:sparkles:`       | **새로운** 기능을 소개 할 때                                 |
| ⚡️     | `:zap:`            | 도입 할 때 **이전 버전과 호환되지 않는** 특징, `@CHANGED`주석 태그 사용 |
| 💡     | `:bulb:`           | 새로운 **아이디어**, `@IDEA`주석 태그                        |
| 🚀     | `:rocket:`         | 배포 / **개발 작업** 과 관련된 모든 것                       |
| 🐘     | `:elephant:`       | **PostgreSQL** 데이터베이스 별 (마이그레이션, 스크립트, 확장 등) |
| 🐬     | `:dolphin:`        | **MySQL** 데이터베이스 특정 (마이그레이션, 스크립트, 확장 등) |
| 🍃     | `:leaves:`         | **MongoDB** 데이터베이스 특정 (마이그레이션, 스크립트, 확장 등) |
| 🏦     | `:bank:`           | **일반 데이터베이스** 별 (마이그레이션, 스크립트, 확장명 등) |
| 🐳     | `:whale:`          | **도커** 구성                                                |
| 🤝     | `:handshake:`      | **파일을 병합** 할 때                                        |

## 다음으로 볼 내용

[협업할 때 사용하는 Commitlint-bot](https://kooku.netlify.com/etc/%ED%98%91%EC%97%85%ED%95%A0%20%EB%95%8C-%EC%82%AC%EC%9A%A9%ED%95%98%EB%8A%94-commitlint-bot/)

### Reference

[좋은 git 커밋 케시지를 작성하기 위한 7가지 약속 :: TOAST Meetup!](https://meetup.toast.com/posts/106)

[Udacity Git Commit Message Style Guide :: udacity](https://udacity.github.io/git-styleguide)

[Git 사용 규칙 - Git commit 메시지 :: h3ngss0](https://tttsss77.tistory.com/58)

[커밋 메시지 가이드 :: RomuloOliveria](https://github.com/RomuloOliveira/commit-messages-guide/blob/master/README_ko-KR.md)

[style-git-commit-message :: slashsbin](https://github.com/slashsbin/styleguide-git-commit-message)