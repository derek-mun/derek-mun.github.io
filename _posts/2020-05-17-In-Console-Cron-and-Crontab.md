---
layout: post
title: "Cron 과 Crontab을 배워보자 (Ubuntu 18.04)"
categories:
  - In Console
tags:
  - Linux
  - Ubuntu 18.04
  - Shell
  - cron
  - crontab
  - 예약실행
  - 자동 실행
comments: true
--- 

> 자동으로 무언가를 몇 월 며칠 몇 시에 실행시키고 싶을 때는 어떻게 해야 할까? 
>> 사용자가 특정 날짜와 시간에 지정한 프로그램을 실행할 수 있도록 도와주는 cron을 알아보자.

---

# 1. cron 이란?

 - 사용자가 특정 날짜와 시간에 지정한 프로그램을 실행할 수 있도록 도와준다. 
 - ```cron```을 사용하면 시스템을 자동화하거나 정기적으로 log를 생성하거나 주기적으로 무언가를 수행할 때 효율적이다.
 - ```cron```서비스는 1분마다 깨어나서 사용자의 ```crontab```파일을 확인한다. 
 - ```crontab``` 파일은 특정 날짜와 사간에 실행하길 원하는 이벤트의 목록을 포함한다.

---

# 2. crontab 파일
 
 - 7개의 필드 구성하여 지정가능 (```Minute(분) | Hour(시간) | Dom(일) | Month(월) | Day_Of_Week(DoW) | User | Command```)
 - 만약 특정 열에 복수 항목을 넣고 싶다면, 쉼표 ```,```로 구분된 형태로 작성하면 되며, 자세한 내용을 아래 표를 참고하자.  
 
 | 필드 | 값 설명 |
 | :--- | :---|
 | m (minute) | 0~59 값이며, 분을 의미 |
 | h (hour) | 0~24 값이며, 시간을 의미 |
 | dom (day of month) | 1~31 값이며, 일을 의미 | 
 | mon (month) | 1~12 값이며, 월을 의미 |
 | dow (day of week) | 0~7 값이며, 0 = 일요일, 1= 월요일 ... 6=토요일 |
 | user | user (계정명) 을 의미 |
 | command | 실행할 명령어를 작성 |

 - 각 필드에는 ```'*', '-', ',', '/'``` 값을 사용할 수 있다.

 - shell 에 ```cat /etc/crontab```을 입력하여 직접 살펴보자
 
```shell 
dev@~ $ cat /etc/crontab 
# /etc/crontab: system-wide crontab
# Unlike any other crontab you don't have to run the `crontab'
# command to install the new version when you edit this file
# and files in /etc/cron.d. These files also have username fields,
# that none of the other crontabs do.

SHELL=/bin/sh
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

# m h dom mon dow user	command
17 *	* * *	root    cd / && run-parts --report /etc/cron.hourly
25 6	* * *	root	test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.daily )
47 6	* * 7	root	test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.weekly )
52 6	1 * *	root	test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.monthly )
#
```

 - line 12에 ``` 17 *	* * *	~ ```에서 *의 의미는 매월, 매일, 매분, 매시간을 의미한다. 
 - line 13,14,15 를 보면 ```/usr/sbin/anacron```라는 것이 있다. 
     - cron과 같이 동작하는 프로그램으로 cron 작업이 실행되는 것을 보장하기 위해 (서버 중지 등에의한 미동작 방지) 사용되는 도구이다. 
 - line 12-15 까지 ``` cron.hourly, cron.daily, cron.weekly, cron.monthly```등이 있는 걸 볼 수 있는데, ```system cron directory```라고 한다. 
     - cron은 주기적으로 실행할 내용을 ```system cron directory```에 넣어놓고 작동한다. 

--- 

# 3. cron의 동작 방식. 

 1. ```crond``` 데몬이 실행되면 ```/etc/crontab```파일을 읽어 들이고, 아래 내용들을 수행한다. 
    - /etc/cron.hourly 
    - /etc/cron.daily
    - /etc/cron.weekly
    - /etc/cron.monthly 
 2. ```/var/spool/cron```에 있는 파일들을 읽어 들임. (각 user별 cron 설정)
 3. ```cron```에 의해 수행되지 못한 작업을 ```anacron```에 듸해 수행을 재시도함
 4. ```/var/log/cron```에 cron의 로그파일을 기록

--- 

# 4. cron 테스트
 - 1분마다 time을 찍는 script를 작성하고 cron에 등록하여 자동실행 되도록 설정해보자. 
 - 먼저 crontab 을 쉽게 설정할 수 있는 ```crontab```에 대해서 알아보자. 
 - ```man crontab```

```
CRONTAB(1)                                  General Commands Manual                                 CRONTAB(1)

NAME
       crontab - maintain crontab files for individual users (Vixie Cron)

SYNOPSIS
       crontab [ -u user ] file
       crontab [ -u user ] [ -i ] { -e | -l | -r }

DESCRIPTION
       crontab  is  the program used to install, deinstall or list the tables used to drive the cron(8) daemon
       in  Vixie  Cron.   Each  user  can  have  their  own  crontab,  and   though   these   are   files   in
       /var/spool/cron/crontabs, they are not intended to be edited directly.
... 
```
 
 - 내용을 보면 ```crontab -e```를 수행하면, 현재 계정의 crontab을 설정할 수 있다고한다. 
 - 처음엔 아무것도 존재하지 않기 때문에 아래와 같이 나오게 된다. 
 
```shell
# Edit this file to introduce tasks to be run by cron.
# 
# Each task to run has to be defined through a single line
# indicating with different fields when the task will be run
# and what command to run for the task
# 
# To define the time you can provide concrete values for
# minute (m), hour (h), day of month (dom), month (mon),
# and day of week (dow) or use '*' in these fields (for 'any').# 
# Notice that tasks will be started based on the cron's system
# daemon's notion of time and timezones.
# 
# Output of the crontab jobs (including errors) is sent through
# email to the user the crontab file belongs to (unless redirected).
# 
# For example, you can run a backup of all your user accounts
# at 5 a.m every week with:
# 0 5 * * 1 tar -zcf /var/backups/home.tgz /home/
# 
# For more information see the manual pages of crontab(5) and cron(8)
# 
# m h  dom mon dow   command

```
 - 제일 아래에 1분에 한번 씩 수행되도록 ```* * * * * /home/dev/time.sh >> /home/dev/time.txt``` 을 작성 해보자. 
 - time.sh 의 내용은 ```echo $(date +"%r")``` 로 시간 출력을 위한 것이다. 
 - 실행 결과를 보면 아래와 같다. 

```shell
dev@~ $ tail -f time.txt
오후 08시 00분 01초
오후 08시 01분 01초
오후 08시 02분 01초
오후 08시 03분 01초
오후 08시 04분 01초
오후 08시 05분 01초
오후 08시 06분 01초
오후 08시 07분 01초
오후 08시 08분 01초
```
---

# 5 . 정리 
 - ```cron```은 사용자가 특정 날짜와 시간에 지정한 프로그램을 실행할 수 있도록 도와주는 기능이다. 
 - ```cron``` 기능을 어디에 활용할 수 있을까?
     - 정기적인 백업
     - 기타 편의성 스크립트 수행. (일정시간에 apt update 수행 등)





 



