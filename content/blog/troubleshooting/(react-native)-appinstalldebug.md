---
title: (react-native) appinstalldebug
date: 2020-04-15 14:04:40
category: troubleshooting
---

## Date

2020.04.15

## Tags

`React-native` `env`

## Contents

Task: app:InstallDebug FAILED

## Solutions

안드로이드 키 서명문제로 발생한 것이다.



D:경로\프로젝트명\android\app\build\intermediates\signing_config\debug\out\signing-config.json

이 경로에 있는 signing-config.json 파일을 삭제 후 다시 빌드하면 된다.

## Reference

[https://ppost.tistory.com/entry/ReactNative-%EA%B6%8C%ED%95%9C%EB%AC%B8%EC%A0%9C%EB%A1%9C-%EB%B9%8C%EB%93%9C%EA%B0%80-%EC%95%88%EB%90%A0%EB%95%8C](https://ppost.tistory.com/entry/ReactNative-권한문제로-빌드가-안될때)