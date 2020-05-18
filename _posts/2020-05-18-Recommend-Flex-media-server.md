---
layout: post
title: "앱 추천: 내 pc에 있는 Media파일을 크롬캐스트로 시청해보자 (Flex Player, Airflow)"
categories:
  - Etc
tags:
  - Mac 크롬캐스트 플레이어
  - 크롬캐스트 미디어플레이어
  - Mac Flex Media Server
  - 크롬캐스트
  - 맥용 앱 추천
  - airflow
comments: true
---

> 내가 보유하고 있는 Media파일을 크롬캐스트로 볼 수 있게 해주는 어플리케이션 
>> Airflow VS Flex Media Server 

---
 
 - 이 포스팅은 Mac OS 기준으로 포스팅이 작성되어 있다. 

## 1. 나의 사용성 분석 및 요구사항. 

 - 몇일 전만 하더라도, 맥북을 데스크탑처럼 사용하기 때문에 TV로 영화를 보기위해서는 USB에 영화를 옮기거나 ```HDMI```를 맥북에 직접 연결하여 사용하였다.  
 - 유튜브를 많이 보기 때문에 자연스럽게 크롬캐스트에 관심을 갖게 되었고, 결국 크롬캐스트를 주문하게 되었다. 
 - 나는 아직 ```Netflix``` 를 이용하지 않기때문에, 넓은 화면으로 보고싶은 ```Youtube``` 영상의 경우에만 크롬캐스트를 이용했다.
 - ```내가 가지고 있는 Media 파일을 크롬캐스트로는 볼 수 없을까?``` 하는 의문이 들었다. 
 - Google 검색을 통해 아래 2개의 크롬캐스트 3rd 파티 어플리케이션을 찾아냈다. 
    1. Airflow
    2. Flex Media Server

---

## 2. Airflow 
> [AirFlow](https://airflowapp.com/)

![AirFlow Image](https://cdn.jsdelivr.net/gh/derek-mun/derek-mun.github.io@master/files/post_img/Airflow.jpg){: width="600px"}

### 장점. 
 - 위에 사진을 보면 알 수 있듯이 정말 간단하고 필요한 것만 있다.
 - 초보자도 아주 쉽게 크롬캐스트로 영상을 스트리밍 할 수 있는 좋은 애플리케이션이다. 
 - MP4, MOV, AVI, MKV, WMV 등 다양한 동영상 포맷을 지원하고, 애플TV에서 자체적으로 재생할 수 없는 동영상 또한 재생 가능하다.
 - .smi (자막) 파일 지원을 자체적으로 하고 있다.
 - 추가적으로, Window 사용자도 사용할 수 있다. 

### 단점. 
 
 - 라이센스를 구매하지 않으면, 플레이타임이 20분으로 제한된다. (다시 재생버튼을 누르면 재생 가능) 
 - 즉, 20분마다 계속 재생버튼을 눌러줘야 하는데 정말 불편하다. 

### 총평
 
 - UI 도 너무 마음에 들고 사용하기 너무쉬운 애플리케이션이라서 구매하고 싶었다. 
 - 하지만, 한 가지 앱만 사용해보고 구매하기에는 너무 미련한 거 같아서 다른 앱을 찾아보았다. 

 ---

## 3. Flex Media Server
> [Flex Media Server](https://www.plex.tv/)

![Flex Media Server Image](https://cdn.jsdelivr.net/gh/derek-mun/derek-mun.github.io@master/files/post_img/Flex-media-server.jpg){: width="800px"}

### 장점 
 - 내 PC에 Midea Server를 열어서 제공한다. 
 - Flex 이름이 가장 마음에 든다. 

### 단점
 - 개발자가 아닌 일반 사용자들이 사용하기에는 Airflow에 비해서 어렵다. 
 - 웹 페이지 기반의 UI이기 때문에, 기존에 알고 있던 동영상 플레이어와는 전혀 다른 UI를 갖는다.

### 총평 
 - ```Flex Media Server```는 UI 주소를 보면 알 수 있듯 ```127.0.0.1:32400/web/index.html``` 실제 Media server가 내 localhost 32400 포트에서 돌고있는 것이다.
 - 개발자 입장에서는 뭔가 더욱 이끌렸다.. (사실 무료라서 일지도 모른다.)
 
---

## 3. 정리
> 나의 선택은? 

### 나의 사용성
 - 나는 한 달에 1~2번 영상을 다운로드해 시청할 뿐, 많이 사용하지 않는다. 
 - 솔직히 Airflow가 무료였다면 고민할 필요도 없이 ```Airflow```를 사용했겠지만, 사실 나의 사용 빈도수가 높지 않기 때문에 ```Flex Media Server```를 사용하려고 한다.
 - 영상을 정말 많이 다운받아서 시청하는 사람들이라면, ```Airflow```는 18.99$을 지불하면서 사용하는 게 전혀 아깝지 않은 선택이라고 생각된다.

---
 


 


