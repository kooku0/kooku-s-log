---
title: (react-native) apptransformnativelibswithmergejnilibsfordebug
date: 2020-04-15 13:04:06
category: troubleshooting
---

## Date

2020.04.15

## Tags

`React-native` `env`

## Contents

dependency build error (apptransformnativelibswithmergejnilibsfordebug)

## Solutions

**Step 1: Delete Your Build Folders**

1. Delete build folder from android
2. delete build folder from android/app

**Step 2: Clean Your Project**

I am assuming your terminal on your project root

```shell
$ cd android$ gradlew clean$ cd ..
```

**Step 3: Run Your Project**

```shell
$ npx react-native run-android
```

## Reference

https://stackoverflow.com/questions/57194244/task-apptransformnativelibswithmergejnilibsfordebug-failed

https://medium.com/@tiwarishani/task-app-transformnativelibswithmergejnilibsfordebug-failed-in-react-native-83bdbc43ce79