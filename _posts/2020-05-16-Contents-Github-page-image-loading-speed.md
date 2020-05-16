---
layout: post
title: "Github page 속도 (Image loading시간)에 관하여.."
categories:
  - Contents
tags:
  - github page
  - github.io
  - CDN
  - Image loading speed
  - jsdelivr
  - 성능향상
comments: true
--- 
> Github Page 의 최대 단점. 속도 문제
>> 속도에서 가장 손해 보는 이미지 로딩 문제를 해결해보자.

---

> **먼저 아래 2개의 이미지를 로딩 속도를 비교하면서, 이번 포스팅의 결과를 미리 확인해보자.**

# 1. 이미지 로딩 속도 

>> 첫 번째 이미지는 기본 Github 에 올라가있는 나의 image 파일이고 (용량 585K), 두 번째 이미지는 CDN이 적용된 이미지이다.

## 기본 github 이미지.

![github page img](/files/preview.png)
<br>

## CDN 을 이용한 같은 이미지

![github page img](https://cdn.jsdelivr.net/gh/derek-mun/derek-mun.github.io@master/files/preview.png)

---

# 2. 원인 분석 

> 지금까지 포스팅을 많이 작성한 건 아니지만, 로컬에서 확인을 하고 Github에 반영하면서 이러한 속도 문제에 대해서 생각해본 적이 없었다. 
 문득 스마트폰으로 나의 블로그에 접속해보았는데, 이미지 로딩 속도가 소름 돋게 느려서 당황했다. 
 
 - 처음에는 이미지의 사이즈가 커서 로딩이 느린 건가 하는 바보같은 걱정을 했다. wget을 이용하여 나의 github에서 이미지를 다운로드해 보았다. 

```shell
dev@~ $ wget https://github.com/derek-mun/derek-mun.github.io/blob/master/files/preview.png
--2020-05-16 13:52:41--  https://github.com/derek-mun/derek-mun.github.io/blob/master/files/preview.png
Resolving github.com (github.com)... 52.78.231.108
Connecting to github.com (github.com)|52.78.231.108|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: unspecified [text/html]
Saving to: ‘preview.png’

preview.png                      [ <=>                                        ]  67.83K  --.-KB/s    in 0.008s  

2020-05-16 13:52:41 (8.16 MB/s) - ‘preview.png’ saved [69456]
```

 - 0.008s 이면 충분한 속도인데, 왜 느리게 로딩되는 것일까. 
 - 이번엔 Github가 아닌 나의 github page에서 이미지를 받아보았다. 

```shell
dev@~ $ wget https://derek-mun.github.io/files/preview.png
--2020-05-16 14:35:23--  https://derek-mun.github.io/files/preview.png
Resolving derek-mun.github.io (derek-mun.github.io)... 185.199.111.153, 185.199.109.153, 185.199.110.153, ...
Connecting to derek-mun.github.io (derek-mun.github.io)|185.199.111.153|:443... connected.
HTTP request sent, awaiting response... 301 Moved Permanently
Location: https://derek-mun.com/files/preview.png [following]
--2020-05-16 14:35:24--  https://derek-mun.com/files/preview.png
Resolving derek-mun.com (derek-mun.com)... 185.199.108.153, 185.199.109.153, 185.199.110.153, ...
Connecting to derek-mun.com (derek-mun.com)|185.199.108.153|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 598912 (585K) [image/png]
Saving to: ‘preview.png’

preview.png                  100%[===========================================>] 584.88K   156KB/s    in 3.8s    

2020-05-16 14:35:29 (156 KB/s) - ‘preview.png’ saved [598912/598912]
```
 - Github page의 속도에 대해서 이야기는 많이 들었지만, 이렇게 느린 줄은 몰랐기에 정말 놀랐다..
 - ping을 해보아도 많이 느린 걸 알 수 있다. 

```shell
dev@~ $ ping derek-mun.github.io
PING derek-mun.github.io (185.199.108.153) 56(84) bytes of data.
64 bytes from 185.199.108.153 (185.199.108.153): icmp_seq=1 ttl=51 time=58.3 ms
64 bytes from 185.199.108.153 (185.199.108.153): icmp_seq=2 ttl=51 time=58.8 ms
64 bytes from 185.199.108.153 (185.199.108.153): icmp_seq=3 ttl=51 time=60.8 ms
64 bytes from 185.199.108.153 (185.199.108.153): icmp_seq=4 ttl=51 time=58.8 ms
64 bytes from 185.199.108.153 (185.199.108.153): icmp_seq=5 ttl=51 time=62.0 ms
64 bytes from 185.199.108.153 (185.199.108.153): icmp_seq=6 ttl=51 time=58.9 ms
64 bytes from 185.199.108.153 (185.199.108.153): icmp_seq=7 ttl=51 time=56.1 ms
64 bytes from 185.199.108.153 (185.199.108.153): icmp_seq=8 ttl=51 time=56.5 ms
64 bytes from 185.199.108.153 (185.199.108.153): icmp_seq=9 ttl=51 time=62.0 ms
64 bytes from 185.199.108.153 (185.199.108.153): icmp_seq=10 ttl=51 time=60.7 ms
^C
--- derek-mun.github.io ping statistics ---
10 packets transmitted, 10 received, 0% packet loss, time 9010ms
rtt min/avg/max/mdev = 56.149/59.351/62.076/1.987 ms
```
---

# 3. CDN

## CDN(Content Delivery Network) 이란? 

> 콘텐츠 제공 네트워크
>> 한곳에 있는 서버에 많은 사람들이 접속하게 되면 지연시간이 늘어나게 된다. 
>> 때문에 속도가 느려지는데 이를 방지하기 위해 여러 곳에 캐시서버를 두고 가까운 서버에서 전송을 받을 수 있도록 해주는 기술

 - 보통 정적인 콘텐츠(이미지, 영상, 자바스크립트 등)를 제공한다
 - [jsdelivr](https://www.jsdelivr.com/network#map) 보면 jsdelivr의 캐시서버 위치들을 볼 수 있다. 

 - **그래서 이걸 어떻게 사용해야 하는가?**

     - Github 스토리지가 Public이라면, 굳이 작업할 사항은 없다. 
     - **https://cdn.jsdelivr.net/gh/계정명/레파지토리명/파일경로**
     - 기존에 사용하던 방식에서 위와 같이 써주면 된다. 

 - 위의 경로에 파일이 없을 경우에만 github에 가서 불러오고, 자동으로 업데이트가 되어 다음번에 접근할 때는 빠르게 이용할 수 있다. 
 - 속도를 한번 비교해보자. 

```shell
dev@~ $ wget https://cdn.jsdelivr.net/gh/derek-mun/derek-mun.github.io/files/preview.png
--2020-05-16 15:04:14--  https://cdn.jsdelivr.net/gh/derek-mun/derek-mun.github.io/files/preview.png
Resolving cdn.jsdelivr.net (cdn.jsdelivr.net)... 104.16.87.20, 104.16.88.20, 104.16.89.20, ...
Connecting to cdn.jsdelivr.net (cdn.jsdelivr.net)|104.16.87.20|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 598912 (585K) [image/png]
Saving to: ‘preview.png’

preview.png                  100%[===========================================>] 584.88K  --.-KB/s    in 0.05s   

2020-05-16 15:04:15 (11.5 MB/s) - ‘preview.png’ saved [598912/598912]
```

  - 3.8s 와 0.05s 면 엄청난 차이이다. 
  - 포스팅에 이미지를 넣을 때 앞으로 위와 같은 방식으로 하면 빠른 이미지 로딩을 확인할 수 있다.

---

# 4. 정리

 - ```CDN(Content Delivery Network)```이란 웹 사이트의 접속자가 서버에서 콘텐츠를 다운로드해야 할 때, 자동으로 가장 가까운 서버에서 다운로드하도록 하는 기술이다. 
 - 이 기술을 이용하면 특정 서버에 트래픽이 집중되지 않고, 콘텐츠 전송 시간이 매우 빨라지는 장점이 있다.

---

