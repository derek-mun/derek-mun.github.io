---
layout: post
title: "Linux 압축에 대해서 알아보자 (tar, gzip)"
categories:
  - Contents
tags:
  - tar
  - gzip
  - Linux
  - 압축
  - 압축해제
  - 명령어
  - command
  - 압축 옵션
comments: true
--- 
> 은근 자주 사용하게 되는 명령어 tar에 대해서 알아보자.
>> -cvzf , - xvzf 등 옵션을 항상 까먹거나 헷갈리지 않게 하는 방법.

# 1. tar 명령어 (tape archiver)

 - ```tar``` 는 우리가 흔히 ```window os```에서 사용하던 ```알집```이라고 생각하면 된다. 즉, 여러 개의 파일을 하나로 묶거나 풀 때 사용하는 명령.
 - ```"Tape ARchiver"```의 앞 글자를 조합하여 ```tar```라고 명명이 됨
 - **Linux 시스템에서 tar 명령은 묵여지기 전 파일들 각각의 속성과 심볼릭 링크 등을 유지할 수 있기 때문이다. (배포 용도로 많이 사용됨)**
 - 보통 리눅스에서 "파일들을 압축한다." 혹은 "tar로 압축한다"라는 표현을 많이 쓰는데, 정확히는 ```tar```는 압축을 하지 않는다.
 - 오직 하나로 묶는 역할만 수행한다. 
 - 아래 테스트로 용량을 비교해보자.

```
dev@tar_test $ ls -al
total 1048
drwxrwxr-x 2 dev dev   4096 May 23 16:46 .
drwxr-xr-x 6 dev dev   4096 May 23 16:41 ..
-rw-rw-r-- 1 dev dev 210925 May 23 16:42 tar1.txt
-rw-rw-r-- 1 dev dev 210925 May 23 16:36 tar2.txt
-rw-rw-r-- 1 dev dev 210925 May 23 16:36 tar3.txt
-rw-rw-r-- 1 dev dev 210925 May 23 16:36 tar4.txt
-rw-rw-r-- 1 dev dev 210925 May 23 16:37 tar5.txt

dev@tar_test $ tar cvf nozip.tar *
tar1.txt
tar2.txt
tar3.txt
tar4.txt
tar5.txt

dev@tar_test $ tar cvzf zip.tar.gz *.txt
tar1.txt
tar2.txt
tar3.txt
tar4.txt
tar5.txt

dev@tar_test $ ls -alh
total 2.2M
drwxrwxr-x 2 dev dev 4.0K May 23 16:47 .
drwxr-xr-x 6 dev dev 4.0K May 23 16:41 ..
-rw-rw-r-- 1 dev dev 1.1M May 23 16:47 nozip.tar
-rw-rw-r-- 1 dev dev 206K May 23 16:42 tar1.txt
-rw-rw-r-- 1 dev dev 206K May 23 16:36 tar2.txt
-rw-rw-r-- 1 dev dev 206K May 23 16:36 tar3.txt
-rw-rw-r-- 1 dev dev 206K May 23 16:36 tar4.txt
-rw-rw-r-- 1 dev dev 206K May 23 16:37 tar5.txt
-rw-rw-r-- 1 dev dev  91K May 23 16:47 zip.tar.gz
``` 
 - 압축 옵션에 대해서 아래 다시 살펴보고, ```nozip.tar```와 ```zip.tar.gz```의 용량을 비교해보자. 
 - 1.1M 와 91k의 사이즈 차이면 많은 차이가 있다. 

> ```z option```을 주면 무조건 확장자를 ```.gz```을 해야 하는 건가? 
>> 아니다, 해보면 알겠지만 만들어진다. 하지만, 이 압축 파일을 푸는 사람에게 어떤 옵션으로 압축되어 있는지 알리는 용도로 통상적으로 사용한다.

---

# 2. tar 명령어 대표 옵션 및 사용 예제

```
dev@tar_test $ tar --help
Usage: tar [OPTION...] [FILE]...
...

```

## 대표 옵션
 - 기본적으로 아래 명령어만 있어도 문제없이 사용 가능하다. 
 - 이외에 자세한 옵션은 필요에 따라 ```man tar```를 참고하자.

1. (-f) : 대상 tar archive 지정 (**기본 옵션**)
2. (-c) : tar 생성, 기존 archive는 덮어 쓰기. (**묶을 때 사용**)
3. (-x) : tar archive에서 파일 추출, (**풀 때 사용**)
4. (-v) : 처리되는 과정을 나열하도록 함
5. (-z) : gzip 압축 적용. (위의 예제에서 사용)
6. (-C) : 지정된 디렉토리에 묶음 풀기
7. (-t) : archive에 포함된 내용 확인하기

## 사용 예제

```s
 tar cvf tar_name.tar *                     # 현재 디렉토리 모든 파일과 디렉토리를 tar_name.tar로 묶기
 tar cvf tar_name.tar [PATH]                # [PATH] 대상을 tar_name.tar로 묶기 
 tar cvf tar_name.tar [FILE1] [FILE2] ...   # [FILE1] [FILE2] ... 파일을 지정하여 tar_name.tar로 묶기
 tar xvf tar_name.tar -C [PATH]             # tar_name.tar 를 [PATH] 에 풀기. 
 tar tvf tar_name.tar                       # tar_name.tar 내용 살펴보기.
 tar cvzf tar_name.tar.gz *                 # 현재 디렉토리를 tar로 묶고 gzip으로 압축 파일명 -> tar_name.tar.gz
 tar xvzf tar_name.tar.gz                   # tar_name.tar.gz 을 현재 디렉토리에 풀기.
```
---

# 3. 정리 

 - 이 포스팅은 과거에 매번 까먹고 google에서 검색하며, 매번 명령을 수행하던 나의 과거가 생각나서 한번 정리해 보았다. 
 - 가장 많이 사용하는 ```tar cvzf```와 ```tar xvzf``` 만 기억하고, ```man tar```혹은 ```tar --help```를 통해 살펴보자. 

---