---
layout: default
title: "https1(최다영)"
parent: "web"
grand_parent: "background"
nav_order: 1
has_children: true
# permalink: "docs/background/backend/dependency-injection"
---


# What is https
![image](https://i.imgur.com/4GHgl0T.png)

**HyperText Tranfer Protocol** : HTML 같은 문서를 웹 브라우저(Client)가 웹 서버(Server)간의 자원을 주고 받을 때 쓰는 통신 규약

**HTTP Secure Socket**
SSL(Secure Sockey Layer)프로토콜을 이용하여 보안상의 문제를 해결

> **공개키 암호화 방식**: A키로 암호화를 하면 B키로 복호화를 할 수 있고, B키로 암호화 하면 A키로 복호화 할 수 있는 방식

# SSL steps

[Link](https://www.ibm.com/support/knowledgecenter/SSFKSJ_7.1.0/com.ibm.mq.doc/sy10660a.gif)

![image](https://i.imgur.com/YIfy1wK.png)


1.   https:// 사용하여 SSL로 암호화된 페이지를 요청

2.  CA 기업은 인증서(**Public Key** 등) 를 만들고 **Private Key**로 암호화해서 제공

3.  인증서가 CA로부터 서명된 것인지 확인

4.  **Public Key** 를 사용해서 대칭 암호화키(symmetric encryption key)를 비릇한 URL, http 데이터들을 암호화해서 전송

5.  **Private Key**를 이용해서 랜덤 대칭 암호화키와 URL, http 데이터를 복호화

6.  요청받은 URL에 대한 응답을 웹브라우저로부터 받은 랜덤 대칭 암호화키를 이용하여 암호화

9. 대칭 키를 이용해서 http 데이터와 html문서를 복호화

- CA(Certificate Authority): 공개키 저장소


# Practice
OpenSSL 을 이용하여 인증기관을 만들고 Self signed certificate 를 생성하고 SSL 인증서를 발급
> **Note:**  OpenSSL 은 상용수준의 데이타암복호/SSL/TLS 라이브러리를 만들기 위한 프로젝트이다.


