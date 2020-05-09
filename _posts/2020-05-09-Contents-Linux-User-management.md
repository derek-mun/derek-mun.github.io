---
layout: post
title: "Linux 사용자 및 그룹 관리에 대해 알아보자. "
categories:
  - Contents
tags:
  - Linux
  - User 관리
  - passwd
  - useradd
  - userdel
  - usermod
  - groupadd
  - groupdel
  - groupmod
comments: true
--- 

> Linux User 와 group 관리에 대해서 알아보자.
>> 사용자 구성요소, 사용자 관리도구 (생성부터 수정, 삭제)까지 알아보도록 하자.

# User & Group Management 

 - 리눅스나 유닉스는 다중 사용자 환경의 운영체제로 설계되었다. 그만큼 사용자가 많다면 이러한 사용자들의 대해 관리
가 필요하다.
 - 시스템에서 관리자가 사용자를 만들때마다, 어떤 사용자가 어떤 권한을 갖고 어디까지 접근 할수 있는지 결정을 해야한다.
 - 이번 포스팅에서는 이러한 사용자 관리에 대해서 공부해보자.
 
---
## 1. 사용자 구성요소 

  - 리눅스에서 파일, 프로그램은 모두 소유자가 있다. 각 사용자는 고유 식별번호 USER ID (이하 **UID**) 를 갖고 있으
며, 반드시 특정 그룹(이하 **GID**) 에 속해 있어야 한다.
  - 파일과 프로그램의 접근 권한은 앞서 말한 ```UID, GID``` 에 따라 달라진다.


> 리눅스가 사용자 정보를 저장하는 방식은 바로 텍스트 형태이다. 때문에 편집기를 이용하여 사용자 정보를 관리할수 있다.

### /etc/passwd

   - ```/etc/passwd``` 파일을 보면 아래의 형태로 사용자의 정보를 저장하고 있다.

```shell
 derek:x:1000:1000:derek,,,:/home/derek:/bin/bash

 # 보기쉽게 정리해보자
 #    derek   :   x   :  1000 :  1000 : derek,,, :  /home/derek/ :  /bin/bash :
 # 사용자 계정명 : 패스워드 :  UID  :  GID :  GECOS   :   디렉토리       :   shell    :

```
 1. 사용자 계정명
     - 말 그대로 생성한 사용자 계정명을 의미한다 .

 2. 패스워드
     - 로그인 시에 입력하는 패스워드를 의미하지만 암호화 되어있어서 x 로 표기 된다.

 3. UID (User ID)
      - 앞서 말했던 사용자 고유의 식별번호 이며, 실제 시스템에서 접근권한을 체크할때 이용하는 값이다.

 4. GID (Group ID)
      - 사용자가 소속되어있는 Group의 고유 값이다. 사용자 접근권한을 결정하는 데 중요한 역할을 한다.
      - Group ID 관련 된 부분은 ```/etc/group``` 에 정리하도록 하자.

 5. 디렉토리
     - 사용자의 홈 디렉토리를 나타낸다. 계정별로 고유의 설정파일 (ex bashrc, vimrc 등) 을 저장할 공간이 필요하다.  하지만 다른 디렉토리여도 상관없다.

 6. shell
     - xwindow 환경이 아닌곳에서 리눅스에 접속하면 처음 보이는것이 shell 이다.
     - shell에도 bash, zsh 등이 있지만, 나는 그냥 bash를 선호한다.
     - 리눅스에서 사용할 수 있는 shell 정보를 보고싶다면 ```/etc/shells``` 에 사용할 수 있는 shell 정보가 있으니 자유롭게 변경하면 된다.

### /etc/group

 - ```/etc/group``` 을 보면 한줄에 한 그룹씩 표시된다. 시스템에 접속한 사용자는 적어도 한 개 이상의 그룹에 속하며
, 사용자가 한 개의 그룹에 속해 있다면 그 그룹은 해당 사용자의 기본 그룹이다.
 - 기본 그룹을 변경하려면 ```/etc/passwd``` 에 GID 를 수정하면 된다.
 - ```/etc/group``` 에 있는 항목은 다음과 같다.

 |:---|:---|
 | Group name | 그룹 이름 |
 | Group password | 그룹에 가입시킬 때 설정하여 사용 할 수 있다. (선택사항) |
 | Group ID (GID) | 그룹 이름에 대응하는 그룹 번호 |
 | Group members| 그룹원 목록 (콤마로 구분됨) |

  - 나의 계정 그룹정보

 ```shell
 # group 의 멤버가 없다.. 혼자쓰니까 ㅎㅎ
 derek:x:1000:
 ```
---
## 2. 사용자 관리 도구

 - 기본적으로 리눅스에서는 사용자를 관리하기위한 CLI (command line interface) 를 제공한다.

### 1. **useradd** 
  - 이름이 의미하는데로 계정을 추가하는 명령어이다. 
  - 아래는 ```man useradd``` 이다.
  
```
USERADD(8)                                System Management Commands                                USERADD(8)

NAME
       useradd - create a new user or update default new user information
SYNOPSIS
       useradd [options] LOGIN

       useradd -D

       useradd -D [options]
... 
```
  - 즉 ```useradd test``` 라고 입력하면 바로 test 계정이 생성이된다. 
  - 하지만 저렇게만 하게되면 
  - 보통 유저를 생성할때 많이 사용하는 옵션은 ```-m (home directory 생성), -s SHELL (shell 지정)``` 이 있다. 이것은 계정을 만들면서 ```/home ```  경로밑에 계정 고유 폴더를 생성하는 옵션이다
  - 계정을 만들고나면 다음 명령어로 계정의 비밀번호를 설정해야한다 ```passwd [계정명]```
  
> 나도 blog 를 작성하면서 이것저것 테스트 용도로 계정을 하나 만들었다.
>> useradd -m -s /bin/bash dev

### 2. **usermod** 
  - 혹시라도 ```useradd``` 를 하는과정에서 option 을 빼먹거나 잘못만들었을경우 수정하는 명령어이다.

  - 나의 경우 ```dev``` 계정을 생성하면서 option 에 GECOS 값을 넣지않아서 ```usermod``` 명령어를 실행해볼겸 변경 해보았다. 

```shell
root@~ $ cat /etc/passwd | grep dev
dev:x:1001:1001::/home/dev:/bin/bash
  
root@~ $ usermod -c study-account dev
root@~ $ cat /etc/passwd | grep dev

dev:x:1001:1001:study-account:/home/dev:/bin/bash
```

### 3. **userdel** 

 - 지우고 싶은 계정이 있을경우 ```userdel``` 명령어를 통해서 지울수 있다. 
 - 입력방법은 ``` userdel [option] 계정명```인데 , 2개의 옵션만 기억하면 된다. 

 1. -f, --force  (현재 사용자의 소유가 아닌 파일이라도 삭제가능)
 2. -r, --remove (홈 디렉토리 와 mail spool 까지 삭제)

### 4. groupadd, groupmod, groupdel 
 - 사실 많은 사용자가 이용하는 개발서버가 아닌경우 이 명령어는 자주 사용하지 않을것이라고 생각된다. 그래도 간단하게 정리를 해보자. 
 - group 관련 명령어는 개별 사용자에 대한 것이 아닌 /etc/group 파일에 있는 그룹들을 대상으로 하는 명령어이다. 

 1. groupadd 
     - group 을 추가할 때 사용하는 명령어로 입력 방법은 ```groupadd [option] GROUP이름``` 이다.
     - option 에 **-g  gid** 는 GID를 설정하는 옵션인데 **-o (GID 중복허용) 옵션이 사용되지 않았다면, 이 값은 반드시 고유해야하며 기본적으로 1000이상의 값을 설정해야한다. 

 2. groupdel
     - group 을 삭제할 때 사용하는 명령어로 입력방법은 ```groupdel GROUP이름``` 이다. 

 3. groupmod
     - group 정보를 변경할 때 사용하는 명령어로 입력방법은 ```groupmod [option] GROUP이름``` 이다.
     - 주요 옵션은 다음과 같다. ```-g GID (GID 변경) , -n NEW_GROUP (그룹이름변경), -o (GID중복허용)``` 

---
## 3. 정리 
 - 이번 포스팅에서는 사용자 및 그룹 관리하는 법을 알아보았다. 이 포스팅에 있는 내용들은 유닉스 계열의 시스템에서도 적용되는 것들이다.
 - 이번 포스팅을 작성하면서 ```dev``` 라는 계정을 새로 만들었다. 앞으로 포스팅을 위한 작업 및 테스팅을 ```dev``` 계정에서 진행하면서 권한에 대한 부분도 공부해볼 생각이다. 
 - 추후 계정이나 그룹에 대해 새로 알게되거나, 추가할 내용이 있다면 이번 포스팅에 추가 업데이트 하도록 해야겠다. 

 ---
