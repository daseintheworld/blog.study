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
<br>

* step0 한 개의 Bounded context에서 출발

* step1 마음을 비운다. (기술적인 용어는 잊자)

* step2 Bounded context 내의 행위들을 여럿 나열해본다.

* step3 이 행위들을 묶어서 Aggregate 이름을 짓고 거기 들어가야 할 개념을 마구 흩뿌려보자

* step4 구성원을 정의한다 : (1)행위, (2)행위가 바꾸는 내용, (3)행위가 ‘품고’ 있어야할 내용을 찾는다.

* step5 행위 1 : aggregate 1 : 트랜잭션 1 로만 기능한다는 것을 염두하고, 필요한 것만 넣고 필요 없는 것을 빼자!


(1) 행위 - 도메인의 언어로 하자
xxx라는 화장품 데이터의 용량을 업데이트한다(X)
xxx 화장품을 약간 쓴다(O)

(2) 행위가 바꿀 내용
화장품은 기본으로 알고 있고, 거기에 용량 정보가 들어갈듯 (O)
혹시 모르니까 화장품 유통기한도 넣자 (X)

(3) 행위가 품는 내용
얼만큼 썼다(O)
혹시 모르니까 쓰고나서 얼마나 예뻐졌는지도 알려주자 (X)

## 2. 성과물
<br>

### 기범
<br>
![](https://user-images.githubusercontent.com/55048882/70283969-c0657880-1805-11ea-8b2a-71716ca30761.png)
<br><br>
[작업위치- lucid chart(login 필요)](https://www.lucidchart.com/invitations/accept/ce98f72c-3b1d-4c2f-b2c9-2e949ce4ceaf)
<br><br>
1. 중심 소재 설명
* 문제
<br>-- 기억력이 안좋다 (무슨 일을 누구와 했는가)
<br>-- 자료를 자꾸 잃어버린다 (인터넷 링크, 그 외 등등)
* 목표
<br>-- 삶의 궤적을 기록, 돕는 도메인 만들기
<br>
2. 주제들 설명
* thinking
<br>-- 내가 생각하던 사실, 흐름, 연관 자료의 위치, 미래의 컨텐츠 등을 기록하고 이를 돕는 기능을 만든다
* doing
<br>-- 내가 하던 것, 그 과정, 참조 자료의 위치, 해야할 일 등을 기록하고 돕는 기능을 만든다
<br>
3. 주제들끼리 경계(bound) 설명
<br>-- 1차적으로 생각하는 것과 행동하는 것을 구분함.
<br>-- 생각과 행동이 섞여서 같이 가는 경우, 어플리케이션이 도울 방법을 기준으로 세부 모델을 나눔
기록
<br>
4. 각 주제가 (1)공유하는 개념과 (2)각자만이 가지는 개념 설명
<br>(1) '경험', '분석'과 같은 것은 생각과 행위가 거의 동시에 이루어지는 것 같다.
<br>(2) 생각을 순수하게 하는 건 '고민', '결정' 등등. 행동을 순수하게 하는건 '일', '탐색' 등등.
<br><br><br><br>
### **최다영**
![](https://user-images.githubusercontent.com/55048882/70767007-fde87980-1da2-11ea-8849-b70bcf880ee3.png)
<br><br><br><br>
### **김승용**
![](https://user-images.githubusercontent.com/55048882/70767017-0771e180-1da3-11ea-8fa8-6cebd9b673d3.png)
<br><br><br><br>
### **김혜수**
![](https://user-images.githubusercontent.com/55048882/70767024-0c369580-1da3-11ea-8d0d-51dbdc3dc643.png)
<br><br><br><br>
### **박진경**
![](https://user-images.githubusercontent.com/55048882/70767288-0d1bf700-1da4-11ea-8113-73ff5a005048.png)