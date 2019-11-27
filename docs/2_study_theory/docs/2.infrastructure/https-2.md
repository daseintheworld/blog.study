---
layout: default
title: "• https-2(최다영)"
parent: "infrastructure"
grand_parent: "study(방법론)"
nav_order: 4
has_children: true

---

# API를 배포 후 내 도메인과 연결
사용자 지정 도메인 이름을 API의 호스트 이름으로 설정하려면 API 소유자인 사용자가 사용자 지정 도메인 이름에 대한 SSL/TLS 인증서를 제공해야함.

[참고: AWS Certificate Manager에서 인증서 준비](https://docs.aws.amazon.com/ko_kr/apigateway/latest/developerguide/how-to-custom-domains-prerequisites.html)
**ACM을 이용한 SSL 적용- Route53에서 갖고 있는 도메인과 ELB를 연동**
1. 도메인을 구입
2. API Gateway에 내 도메인 설정하기
<br>![사용자지정도메인이름](https://user-images.githubusercontent.com/55048882/69301047-4c00d600-0c58-11ea-88c5-393b131d4a70.png)
<br>
3. 도메인과  AWS route53  (DNS웹 서비스) 연결
<br>![route53](https://user-images.githubusercontent.com/55048882/69301048-4c00d600-0c58-11ea-9a6e-7de05b9ecbcd.png)
<br>
4. AWS ACM을 통해서 구입한 도메인의 SSL Certificate를 발급(1분컷)
5. ELB에 SSL Listener 생성
6. ELB security group 설정
<br>
![사설인증서불러오기](https://user-images.githubusercontent.com/55048882/69301050-4c996c80-0c58-11ea-9d1f-98d330a7971d.png)
<br>Amazon RDS를 통해 호스팅되는 MySQL 데이터베이스에 SSL 연결을 설정가능
<br>Amazon RDS는 각 RDS 데이터베이스에 대한 Secure Sockets Layer(SSL인증서를 자동으로 생성하여 SSL을 통한 클라이언트 연결을 활성화가능
<br>[ MySQL DB 인스턴스와 함께 SSL 사용](https://docs.aws.amazon.com/ko_kr/AmazonRDS/latest/UserGuide/CHAP_MySQL.html#MySQL.Concepts.SSLSupport)
<br>[SSL/TLS를 사용하여 DB  인스턴스에 대한 연결 암호화](https://docs.aws.amazon.com/ko_kr/AmazonRDS/latest/UserGuide/UsingWithRDS.SSL.html)

