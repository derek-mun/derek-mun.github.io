---
layout: post
title: "grep, find 로 작업의 효율을 높여보자 "
categories:
  - Contents
tags:
  - Linux
  - Shell
  - grep
  - find
  - tip
comments: true
--- 

> linux 에서 가장 많이 사용하는 grep, find에 대해 알아보자. 
>> 상세한 설명이 필요한 사람은 ```man page```를 참고하자. 가장 자주 쓰는 option 위주의 포스팅이다

---
## FIND 
 - find(찾다) 는 명령어 그대로 찾는 명령어이다. 
 
 - 기본형식은 ```find [PATH] -name [FILE]``` 이다 

 - 명령어 옵션 및 example은 ```man find``` 를 참고하자. 

 - 아래는 내가 제일 자주 사용하는 커맨드를 정리한 내용이다. 


| :--- | :--- |
| **명령어**| **설명** |
| ```find . -name [FILE]``` | 현재 디렉토리 아래 모든 파일 및 하위 디렉토리에서 [FILE] 검색 |
| ```find / -name [FILE]``` | 루트 디렉토리 아래 모든 파일 및 하위 디렉토리에서 [FILE] 검색 |
| ```find . -name [FILE] > [SAVE_FILE]```| 현재 디렉토리 아래 모든 파일 및 하위 디렉토리에서 [FILE] 검색 결과를 [SAVE_FILE] 에 저장 |

  - 위의 [FILE] 부분에 ```*.txt```, ```*file*``` 등으로 활용할 수 있다. 
  
---

## GREP 
 - ```grep``` 은 ```find``` 와 다르게 입력으로 전달된 내용에서 특정 문자열을 찾고자 할때 사용한다.

 - 기본형식은 ```grep [OPTION...] PATTERN [FILE...]``` 

 - man page를 보면 상당히 많은 옵션과 내용들이 있다. 그 중 몇 가지만 보자면 
 
 | :--- | :---|
 | **옵션** | ** 설명** |
 | -e | 매칭을 위한 PATTERN 전달 |
 | -i | 대/소문자 무시 |
 | -w | 단어 단위로 매칭 |
 | -x | 라인(line) 단위로 매칭 |
 | -m | 최대 검색 결과 갯수 제한 |
 | -n | 검색 결과 출력 라인 앞에 라인 번호 출력 |
 | -H | 검색 결과 출력 라인 앞에 파일 이름 표시 |
 | -I | 바이너리 파일은 검사하지 않음 |
 | -r | 하위 디렉토리 탐색 |
 
 - 나의 경우에는 ```grep -r [FILE] ./*``` 을 가장 많이 사용한다. 

## In console [TODO]
 - alias 를 이용하여 자주 사용하는 command를 쉽게 사용해보자. 
 
 - [손쉬운 find, grep]()
  
 