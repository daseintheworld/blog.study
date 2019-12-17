---
layout: default
title: "• Aggregate1"
grand_parent: "성과물"
parent: "domain"
nav_order: 2

---

### **목표**

* 각자 구축한 aggreate를 공유.

## 1. 방법
* step0 한 개의 Bounded context에서 출발

* step1 마음을 비운다. (기술적인 용어는 잊자)

* step2 Bounded context 내의 행위들을 여럿 나열해본다.

* step3 이 행위들을 묶어서 Aggregate 이름을 짓고 거기 들어가야 할 개념을 마구 흩뿌려보자

* step4 구성원을 정의한다 : (1)행위, (2)행위가 바꾸는 내용, (3)행위가 ‘품고’ 있어야할 내용을 찾는다.

* step5 행위 1 : aggregate 1 : 트랜잭션 1 로만 기능한다는 것을 염두하고, 필요한 것만 넣고 필요 없는 것을 빼자!
<br><br>
## 2. 성과물
* 각 Bounded context 당 행위들 목록
<br><br>
* 각 행위에 대해 가질 상태 목록
<br><br>
* 각 Aggregate에서 쏴야하는 event 목록 (행위들을 모아서 적당히 aggregate를 만든다)
<br><br><br>
### **기범**
