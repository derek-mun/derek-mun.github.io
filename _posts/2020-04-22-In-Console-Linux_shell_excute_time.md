---
layout: post
title: "Linux Shell 프로그램 실행시간 구하기"
categories:
  - In Console
tags:
  - Linux
  - Shell
  - Script
  - date
  - 실행시간 계산
comments: true
---

> 내가 작성한 쉘 혹은 기타 어플리케이션의 실행시간을 구하고싶을때 아래 방법으로 구할 수 있다.

## 프로그램 실행시간 구하기

### shell script 버전 

```shell
#!/bin/bash
StartTime=$(date +%s)   # +%s 는 date의 포맷을 지정해주는 옵션이다.

#실행할 커맨드들을 작성

EndTime=$(date +%s)

echo "It takes $(($EndTime - $StartTime)) seconds to complete this task."
```

---

### bash 쉘에서 바로 해보기 

 - 아래 명령어를 bash에서 입력해보자.
- ```time ( 실행할 명령어 )``` 를 수행시 명령어 수행 후 경과 시간이 나오는 것을 확인할 수 있다. 
 - 테스트는 명령어가 실행한다는 것을 보기위해 ```echo```를 이용하여 보이도록 하였다. 

``` time (for ((i=0;i<50;i++)) do echo "this is time testtime "%i; done) ```

![timetest1](https://cdn.jsdelivr.net/gh/derek-mun/derek-mun.github.io@master/files/post_img/time_test1.jpg)

---

### shell script + time (...) 조합으로 해보기.

 - 아래 time.sh 을 간단하게 작성해준뒤에 ```time (명령어)``` 를 이용해보자.

 ```shell
 #!/bin/bash

show(){
	echo $date1
	echo $date2
	echo $date3
	echo $date4
}
# date 뒤에 있는 +% 뒤에 문자는 format 형식을 의미한다. 
date1=$(date)
date2=$(date +"%T")
date3=$(date +"%r")
date4=$(date +"%I:%M:%S")

show

```

 - shell script 를 작성하고, ```chmod 755 time.sh``` 로 실행 가능하도록 권한을 변경해주자. 
 - bash 에 ```time (./time.sh)``` 를 입력하면, 아래와 같은 결과를 얻을 수 있다.
 
 ![time_test2](https://cdn.jsdelivr.net/gh/derek-mun/derek-mun.github.io@master/files/post_img/time_test2.jpg)




