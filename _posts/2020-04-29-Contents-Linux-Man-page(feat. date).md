---
layout: post
title: "Linux Man page (feat. date)"
categories:
  - Contents
tags:
  - Linux
  - Shell
  - Date format
  - Man Page
comments: true
--- 
 > Linux Man Page 에 대해서 알아보자. 
   
 Linux 시스템을 사용하다보면 많은 command 를 쓰게된다. (ex. ls, cd, grep, ps). 
 하지만 우리가 자주사용하는 명령어는 익숙해지면서 외워지지만, 
 그 명령어들의 옵션 모두를 기억할 수는 없다. 따라서 이번 포스팅에서는 [Man Page](https://en.wikipedia.org/wiki/Man_page) 에 대해서 알아보려고 한다.
 
---

## What is the Man Page?

 - ```man <command> ``` 한번쯤은 다들 사용해보았을 것이다. 한 화면에 모두 보여줄 수가 없기 때문에 man 의 기본 페이저는 [less](https://en.wikipedia.org/wiki/Less) 를 사용한다.  
일단 기본적으로 가장 많이 보는 ```ls``` command 를 이용하여 man page를 살펴보자.

```shell

LS(1)                            User Commands                           LS(1)

NAME
       ls - list directory contents

SYNOPSIS
       ls [OPTION]... [FILE]...

DESCRIPTION
       List  information  about  the FILEs (the current directory by default).
       Sort entries alphabetically if none of -cftuvSUX nor --sort  is  speci‐
       fied.

       Mandatory  arguments  to  long  options are mandatory for short options
       too.

       -a, --all
              do not ignore entries starting with .

       -A, --almost-all
              do not list implied . and ..

       --author
              with -l, print the author of each file

       -b, --escape
              print C-style escapes for nongraphic characters

       --block-size=SIZE
              scale sizes by SIZE before printing them; e.g., '--block-size=M'
              prints sizes in units of 1,048,576 bytes; see SIZE format below

       -B, --ignore-backups
              do not list implied entries ending with ~

 Manual page ls(1) line 1 (press h for help or q to quit)

```

 - 처음 사용하게되면 이게 뭔가 vim 같기도하면서 사용하는데 어려움을 겪을 수 있다. 
 - 기본 사용법 
    - ```q= 종료, space = 다음페이지, b = 이전페이지```
    - ```j = (아래), k = (위)``` 혹은 키보드 방향키를 이용하여 한줄한줄 이동.
 - 나도 이 정도 명령어 만을 이용하여 man page를 활용한다.   
 - 여기까지가 기본 설정되어 있는 less 페이저에 대한 설명이다.

---   
## Manual Sections
 
  이전에 ```man ls``` 의 출력 결과에서 제일 앞에 있는 LS(1) 을 볼수 있다. 
 ls 는 일반 명령어 이기 때문이다. 먼저 아래 표를 보자 

| section | 설명 |
|:---|:---|
| 1 | 일반 명령어 |
| 2 | 시스템 호출 |
| 3 | C 표준 라이브러리 함수 |
| 4 | 특수파일 ( 보통/dev 에서 발견되는 장치 파일)과 드라이버 |
| 5 | 파일 형식과 convensions |
| 6 | 게임과 화면 보호기 |
| 7 | 기타 |
| 8 | 시스템 관리 명령어와 데몬 |

 - ```ls``` 는 Linux 일반 명령어기때문에 자동으로 ```LS(1)``` 로 man page 가 실행이 된것이다. 
 
 - 우리가 잘 알고 있는 c 표준 라이브러리 함수 ```printf``` 를 man page 로 보자 

 - ```man printf```

```shell

PRINTF(1)                                  User Commands                                 PRINTF(1)

NAME
       printf - format and print data

SYNOPSIS
       printf FORMAT [ARGUMENT]...
       printf OPTION

DESCRIPTION
       Print ARGUMENT(s) according to FORMAT, or execute according to OPTION:

       --help display this help and exit

       --version
              output version information and exit

       FORMAT controls the output as in C printf.  Interpreted sequences are:

       \"     double quote

       \\     backslash

       \a     alert (BEL)
 Manual page printf(1) line 1 (press h for help or q to quit)

```

 - 이번엔 ```man 3 printf``` 를 입력해보자

```shell
PRINTF(3)                 Linux Programmer's Manual                 PRINTF(3)

NAME
       printf,   fprintf,  dprintf,  sprintf,  snprintf,  vprintf,  vfprintf,
       vdprintf, vsprintf, vsnprintf - formatted output conversion

SYNOPSIS
       #include <stdio.h>

       int printf(const char *format, ...);
       int fprintf(FILE *stream, const char *format, ...);
       int dprintf(int fd, const char *format, ...);
       int sprintf(char *str, const char *format, ...);
       int snprintf(char *str, size_t size, const char *format, ...);

       #include <stdarg.h>

       int vprintf(const char *format, va_list ap);
       int vfprintf(FILE *stream, const char *format, va_list ap);
       int vdprintf(int fd, const char *format, va_list ap);
       int vsprintf(char *str, const char *format, va_list ap);
       int vsnprintf(char *str, size_t size, const char *format, va_list ap);

   Feature Test Macro Requirements for glibc (see feature_test_macros(7)):

 Manual page printf(3) line 1 (press h for help or q to quit)

``` 
 
  - 두 명령어 결과의 상단부 ```PRINTF(1) , PRINTF(3)``` 의 차이도 있으며 심지어 내용도 다르다. 
  
  - 위에 있는 표와 내용을 보면 차이를 구분할 수 있을것이다. 

## man 을 활용해보자

  기본적으로 bash 에 ```date``` 를 입력하게되면 아래와 같은 결과를 얻을 수 있다.
```
derek@~ $ date
Thu Apr 30 13:16:16 KST 2020 
```
  man date 에 있는 Format 정보를 보면서 ```date``` 명령어를 실행해보자.
 
  - ```man date``` 실행 후 조금 내리다보면 아래 FORMAT 정보가 있다. 
  

```shell
       FORMAT controls the output.  Interpreted sequences are:

       %%     a literal %

       %a     locale's abbreviated weekday name (e.g., Sun)

       %A     locale's full weekday name (e.g., Sunday)

       %b     locale's abbreviated month name (e.g., Jan)

       %B     locale's full month name (e.g., January)

       %c     locale's date and time (e.g., Thu Mar  3 23:05:25 2005)

       %C     century; like %Y, except omit last two digits (e.g., 20)

       %d     day of month (e.g., 01)

       %D     date; same as %m/%d/%y


```  

   - 몇개 만 해보고 이를 활용해보자

```shell

derek@~ $ echo $(date +%a)
Thu

derek@~ $ echo $(date +%A)
Thursday

derek@~ $ echo $(date +%b)
Apr

derek@~ $ echo $(date +%B)
April

derek@~ $ echo $(date +%c)
Thu 30 Apr 2020 05:43:32 PM KST

derek@~ $ echo $(date +%C)
20

derek@~ $ echo $(date +%D)
04/30/20

derek@~ $ echo $(date +%A-%b-%d)
Thursday-Apr-30

derek@~ $ echo $(date +%b-%d-%A)
Apr-30-Thursday

derek@~ $ echo $(date +%Y-%b-%d-%A)
2020-Apr-30-Thursday

```

  - Date 를 활용하여  [shell 프로그램 시간 구하기](https://derek-mun.com/in%20console/In-Console-Linux_shell_excute_time.html) 를 작성했다.

---

