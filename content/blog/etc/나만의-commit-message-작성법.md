---
title: 나만의 commit message 작성법
date: 2019-10-03 07:10:59
category: etc
---


## 꼬리말

1. 꼬리말은 optional이고 **이슈 트래커 ID**를 작성한다.
2. 꼬리말은 **"유형: #이슈번호"** 형식으로 사용한다.
3. 여러개의 이슈번호를 적을때는 쉼표로 구분합니다.
4. 이슈 트래커 유형은 다음 중 하나를 사용한다.
   - **Fixes**: 이슈 수정중 (아직 해결되지 않은 경우)
   - **Resolves**: 이슈를 해결했을 때 사용
   - **Ref**: 참고할 이슈가 있을 때 사용
   - **Related to**: 해당 커밋에 관련된 이슈번호 (아직 해결되지 않은 경우)

예) `Fixes: #45` `Reloated to: #34, #23`

## 예시

```
feat: 패킷 송신 이벤트에 관련된 로그 출력 기능 추가

커밋에 대한 자세한 설명..

Resolves: #123
Ref: #456
Related to: #48, #45
```

## Commitlint-bot 붙히기

여러 팀원들과 같이 프로젝트를 진행할때 나말고 다른 사람들이 commitlint를 맞추지 않다면 아무런 소용이 없을 겁니다. 프로젝트에 commitlint-bot을 붙혀 이러한 문제점들을 해결해보세요.

[commitlint-bot](./commitlint-bot.md)

### Reference

[좋은 git 커밋 케시지를 작성하기 위한 7가지 약속 :: TOAST Meetup!](https://meetup.toast.com/posts/106)

[Udacity Git Commit Message Style Guide :: udacity](https://udacity.github.io/git-styleguide)

[Git 사용 규칙 - Git commit 메시지 :: h3ngss0](https://tttsss77.tistory.com/58)

[커밋 메시지 가이드 :: RomuloOliveria](https://github.com/RomuloOliveira/commit-messages-guide/blob/master/README_ko-KR.md)

[style-git-commit-message :: slashsbin](https://github.com/slashsbin/styleguide-git-commit-message)