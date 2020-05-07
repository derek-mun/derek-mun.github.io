---
layout: post
title: "긴 명령어를 줄여서 써보자 (alias)"
categories:
  - In Console
tags:
  - Linux
  - Shell
  - grep
  - find
  - alias
  - CLI
comments: true
--- 

> 자주 사용하는 긴 명령어를 나만의 command 로 바꿔서 손쉽게 사용해보자.

---
## 1. Alias 란?
  - alias 란 별칭을 의미한다. 
    
  - 먼저 ```vi ~/.bashrc``` 를 입력해보자. 
  
```shell
# 아래는 ```~/.bashrc``` 중 alias 가 있는 부분이다. 
...
# some more ls aliases
alias ll='ls -alF'
alias la='ls -A'
alias l='ls -CF'
...

```

   - 위의 alias 의 의미는 ```ls -alF``` 커맨드를 ```ll``` 이라는 별칭을 부여하여 손쉽게 사용하겠다는 의미이다 . 
   - 아래 실행 결과를 보면 두 명령어가 동일한 결과를 나타내는 것을 볼수 있다.

 ```
derek@workspace $ ls -alF
합계 12
drwxrwxr-x  2 derek derek 4096  4월 23 22:21 ./
drwxr-xr-x 24 derek derek 4096  5월  7 19:43 ../
-rwxr-xr-x  1 derek derek  165  4월 23 21:47 time.sh*

derek@workspace $ ll
합계 12
drwxrwxr-x  2 derek derek 4096  4월 23 22:21 ./
drwxr-xr-x 24 derek derek 4096  5월  7 19:43 ../
-rwxr-xr-x  1 derek derek  165  4월 23 21:47 time.sh*
 ```  

---
## 2. 나만의 alias 만들기 

  > ```~/.bashrc``` 에 있었던 기본 alias 를 직접 수행해 보았는데, 과연 내가 자주 활용할 수 있는 alias 를 어떻게 만들지 직접 해보자. 

1. 나는 가장먼저 매번 치기 귀찮았던 ```cd ..``` 명령어에 별칭을 부여했다. 

```shell
# ~/.bashrc 에 아래 내용 추가 .

alias b='cd ..'
```
2. 수정을 하고 난뒤에 ```b``` 를 입력해보면 알수없는 명령어라고 나온다. 
3. 수정한 bashrc 내용이 적용이 안된것이다. ```. ~/.bashrc```를 입력하여 적용시키고 다시 실행해보자. 

```
derek@testdir $ pwd
/home/derek/workspace/testdir

derek@testdir $ cd ..     <-기존 명령어 수행 
derek@workspace $ pwd
/home/derek/workspace

derek@workspace $ cd testdir   <- 다시 원래 directory로 이동

derek@testdir $ pwd
/home/derek/workspace/testdir

derek@testdir $ b         <- 내가만든 alias로 수행
derek@workspace $ pwd
/home/derek/workspace
```
 - 정상적으로 동작하는 것을 알수 있다. 

---
## 3. 매개변수를 받는 command alias 

#### [grep, find 로 작업의 효율을 높여보자](../contents/Contents-how-to-use-grep,find.html) 

> 위 포스팅에 작성했던 ```grep```, ```find``` command 의 경우 매개변수를 필요로한다. 

 - 일단 별칭을 만들기전에 나만의 규칙을 정해보자.
  
  1. ```find```,```grep``` 은 명령은 수행한 경로에서 수행하자. 즉 PATH 에 "." 

  2. 별칭을 명령어 첫글자로 하기.

```shell
# 매개변수를 받기위해서는 아래와 같이 함수형으로 작성을 해야한다. 

alias g='g(){ grep -nr "$1" ./*; }; g'
alias f='f(){ find . -name "*$1*" -print | grep --color=always "^\|[^/]*$"; }; f'

#find의 경우 color가 적용이 되지않아서 추가적으로 grep을 이용하여 알아보기 쉽게 하였다. 
```

 - test
 
 ```
 # 작업 하는 경로정보와 검색을 위한 파일 정보는 아래와 같다. 

derek@workspace $ ll
합계 16
drwxrwxr-x  3 derek derek 4096  5월  7 19:48 ./
drwxr-xr-x 24 derek derek 4096  5월  7 19:43 ../
drwxrwxr-x  2 derek derek 4096  5월  7 19:48 testdir/
-rwxr-xr-x  1 derek derek  165  4월 23 21:47 time.sh*

derek@workspace $ cat time.sh
#!/bin/bash

show(){
	echo $date1
	echo $date2
	echo $date3
	echo $date4
}

date1=$(date)
date2=$(date +"%T")
date3=$(date +"%r")
date4=$(date +"%I:%M:%S")

show

# grep 을 이용한 "date" 검색 테스트 

derek@workspace $ grep -nr date ./*       <- 기존 사용법 
./time.sh:4:	echo $date1
./time.sh:5:	echo $date2
./time.sh:6:	echo $date3
./time.sh:7:	echo $date4
./time.sh:11:date1=$(date)
./time.sh:12:date2=$(date +"%T")
./time.sh:13:date3=$(date +"%r")
./time.sh:14:date4=$(date +"%I:%M:%S")

derek@workspace $ g date             <- 만들었던 alias를 이용 
./time.sh:4:	echo $date1
./time.sh:5:	echo $date2
./time.sh:6:	echo $date3
./time.sh:7:	echo $date4
./time.sh:11:date1=$(date)
./time.sh:12:date2=$(date +"%T")
./time.sh:13:date3=$(date +"%r")
./time.sh:14:date4=$(date +"%I:%M:%S")

# find 를 이용한 "time" 검색 테스트

derek@workspace $ find . -name "*time*" -print    <- 기존 사용 
./time.sh

derek@workspace $ f time                 <- 만들었던 alias 이용 
./time.sh

```
 - 아주 간편하게 검색이 가능하도록 잘 만든것 같다. 

 - 추가적으로 나는 ```vim``` 3글자가 쓰기귀찮아서 ```alias vi='vim'``` 또한 추가했다.. ㅎㅎ




---



