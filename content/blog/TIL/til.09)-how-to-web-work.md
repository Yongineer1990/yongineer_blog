---
title: TIL.09) How to web work?
date: 2020-07-27 03:55:24
category: Web
thumbnail: './images/web.jpg'
draft: false
---

![](./images/web.jpg)

# 1️⃣ 사용자가 브라우저의 주소창에 wecode.com을 입력하면

- 사용자가 입력한 주소는 `Domain` 이다.

> 💡**Domain?**
- 문자로 이루어진 고유 주소
- `ip`주소를 사용자가 외워서 접속하기 힘들기 때문에 사람의 언어인 문자로 주소를 만들게 되었다.
>
*도메인의 구조*
![](https://images.velog.io/images/yongineer1990/post/acd39681-8fcb-4a3a-8401-26def6897854/image.png)

- 브라우저는 해당 주소를 `DNS Server`로 보내 `ip` 주소를 받는다.

> 💡**DNS?**
- `Domain`을 `ip`로 변환 해주는 서비스
- 전화번호부와 같은 기능을 한다.

# 2️⃣ 브라우저는 ip를 다시 Host Server에 전송한다.

- `Host Server`는 브라우저로 부터 전달 받은 `ip`주소를 받아 해당하는 웹페이지 구성 파일들을 다시 사용자에게 전송한다.
- 이런 서비스를 Web hosting service라고 한다.

> 💡**Web hosting service**
>
>홈페이지의 구성 파일들이 인터넷에 **항상** 연결되고 언제든 요청이 오면 전송되어야 하기 때문에 **절대** 꺼지지 않는 호스트 컴퓨터 (웹 서버)에 저장되어 있다가 사용자의 요청이 오면 언제든지 응답해야 하기 때문에 해당 시스템을 일반 기업에서 구축 해놓기 어려움이 있다. 따라서 Web Hosting service를 사용하며 대표적으로 AWS, Cafe24 호스팅 센터 등이 있다.

# 3️⃣ 전달받은 홈페이지의 구성요소들을 화면에 띄운다.

끝.

✅ **한 장 요약**

![](https://images.velog.io/images/yongineer1990/post/41b403ae-085e-4264-bdcb-bbb3b5dbd1a4/image.png)
