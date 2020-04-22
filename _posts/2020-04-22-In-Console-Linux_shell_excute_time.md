---
layout: post
title: "Linux Shell 프로그램 실행시간 구하기"
date: 2020-04-21
categories:
  - In Console
tags:
  - Linux
  - Shell
  - Script
comments: true
---

> 내가 작성한 쉘 혹은 기타 어플리케이션의 실행시간을 구하고싶을때 아래 방법으로 구할 수 있다.

## 프로그램 실행시간 구하기

```shell
#!/bin/bash
export PATH=$PATH:/sbin:/bin:/usr/sbin:/usr/bin
StartTime=$(date +%s)

#실행할 커맨드들을 작성

EndTime=$(date +%s)

echo "It takes $(($EndTime - $StartTime)) seconds to complete this task."
```
