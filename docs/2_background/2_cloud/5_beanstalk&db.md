---
layout: default
title: "beanstalk&database(김혜수)"
parent: "cloud"
grand_parent: "background"
nav_order: 5
# permalink: "docs/background/backend/dependency-injection"
---

EC2 Beanstalk - Db 연결
===================

1. 초반 실패
-------------
 t2.micro로 설정하고 beanstalk 만들기를 계속 시도했지만 실패ㅠㅠ
두세시간은 기다리고 구글링도 하고 했는데 실패의 이유를 찾을 수 없어 그냥 구성요소도 바꿔보고 이것저것 삽질해봄
![](https://ifh.cc/g/YC8cT.png)

![](https://i.imgur.com/LZqGVuK.jpg)

반복된 삽질 끝에 진짜 어이없는 이유를 찾음 ㅠㅠ

----------
![](https://i.imgur.com/bEPoUQr.jpg)
위의 루트볼륨유형에 범용(SSD) , 크기 최소로 했는데도 뭔가 안맞은건지... 생성이 안되었던 거였음...

----------
![](https://i.imgur.com/B63XxtI.jpg)
위와 같이 루트 볼륨 유형을 기본 컨테이너로 주면 생성이 잘된다....
기본 설정 자체가 범용(SSD)로 되어 있어서 이게 기본인줄 알았는데 아닌가보다


----------
설상가상
Elastic IP address not attached to a running instance per hour (prorated)
란 이름으로 돈이 청구되니 주의하자^^


2. mysql 연결
-------------

그냥 rds 에서 db instance하나 생성해서 연결하면 끝!
처음에는 다 해본거여서 그냥 꿀! 이라고 생각했는데,
여기서도 또 삽질함 ㅠ

저번에 보안 그룹에서 인바운드 ip주소 할당했던걸 완전 까먹고
db가 계속 안붙어서
애꿏은 db instance만 수정하고 다시만들고 했음...ㅋㅋㅋ
VPC나 보안 그룹 수정했다면 까먹지 않도록 합시다...ㅎㅎㅎㅎㅎㅎㅎㅎㅎㅎㅎ휴ㅠㅠㅠㅠㅠㅠ

----------------


3. DB instance - Beanstalk 연결 및 테스트
-------------
![](https://i.imgur.com/R64dfzE.jpg)

---
![](https://i.imgur.com/KyM0Aag.jpg)

---
![](https://i.imgur.com/N8p1eaa.jpg)

---
![](https://i.imgur.com/pNWNFTK.jpg)
 장렬하게 또실패^^
 인생 무엇...

 새로 서브넷을 하나 더 따서 시도했지만
 내가 원하는 db instance에 연결되는게 아니라 db instance가 하나 더 따서 만들어짐...?
 멘탈붕괴 파사삭ㅠ

--> 여기서 RDS 들어가서 구성 바꿔보고
Beanstalk 구성설정 바꿔보고 했는데 실패했어요 ㅠㅠ...

---


5. 그 외 시도하다가 얻은정보 쪼끔
-------------
RDS로 MySQL연결하면 한글이 하단처럼 깨진다.


![](https://i.imgur.com/PYOYJMV.jpg)

---

[<i class="icon-file"></i>여기누르면 해결방법이 있으니 따라하면 된다.][1]


끝!
===









  [1]: https://winterandsnow.tistory.com/12