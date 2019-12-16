---
layout: default
title: "• DDD: Aggregate2"
grand_parent: "study(방법론)"
parent: "concepts"
nav_order: 6
has_children: true
---

## 1. Aggregate 찾기
<br>
**지금까지는 martin fawler나 eric evans의 이야기들을 들으며 살펴보았지만 막상 aggregate를 만들 떄는 여러가지 노하우와 경험이 필요하다. 
<br>특히 van vernon의 'DDD 구현하기' 책이 무척 도움된다.
<br><br>
그러나 권위자의 말 외에 일반 개발자들끼리 커뮤니티에서 나누는 대화가 발전하여 구체적인 개념을 구성하게 마련이다. 이번 문서에서는 그들의 이야기를 토대로 aggregate에 대한 아이디어를 만들어본다.
<br><br>
### **이름 모를 논객1**
[출처(1번답변)](https://stackoverflow.com/questions/51243959/how-to-properly-define-an-aggregate-in-ddd)
### **이름 모를 논객2**
[출처(2번답변)](https://stackoverflow.com/questions/51243959/how-to-properly-define-an-aggregate-in-ddd)
### **이름 모를 논객3**
[출처(1번답변)](http://domain-driven-design.3010926.n2.nabble.com/Bounded-Contexts-and-Aggregates-td6460442.html)
<br><br>

## 2. 순서
<br><br>
* step0 한 개의 Bounded context에서 출발
<br><br>
* step1 마음을 비운다. 일단 entity나 v.o, class, method 등등의 프로그래밍은 생각치 않는다!!!
<br><br>
* step2 Bounded context 내의 한 묶음의 행위들을 나열해본다.
<br><br>
* step3 이 행위들로 Aggregate 이름을 짓고 거기 들어가야 할 개념을 **마구 흩뿌려보자**
<br><br>
* step4 (1)행위, (2)행위가 바꾸는 내용, (3)행위가 '품고' 있어야할 내용을 찾는다.
<br><br>
* step5 행위1, aggregate1, 트랜잭션1로만 활동한다는 것을 이해하고, 필요한 것을 넣고 필요 없는 것을 빼자!
<br><br><br><br>
(1) 행위 - 도메인의 언어로 하자
<br>xxx라는 화장품 데이터의 용량을 업데이트한다(X)
<br>xxx 화장품을 약간 쓴다(O)
<br><br>
(2) 행위가 바꿀 내용
<br>화장품은 기본으로 알고 있고, 거기에 용량 정보가 들어갈듯 (O)
<br>혹시 모르니까 화장품 유통기한도 넣자 (X)
<br><br>
(3) 행위가 품는 내용
<br>얼만큼 썼다(O)
<br>혹시 모르니까 쓰고나서 얼마나 예뻐졌는지도 알려주자 (X)
<br><br><br><br>
## 3. master - detail 관계에서 Aggregate 구성
<br><br>
매우 심플하지만 2년 내내 고민해왔던 아주 기본적인 구성에 대해서 논해봅니다.
<br><br>
**문제**
<br>
* master는 aggregate라 치고... **detail은 aggregate냐 아니냐?**
<br>
* ex. 다영 책임의 화장품 계획, 화장품 아이템
- 관계 짓는 규칙이 있음: '계획 1 => 아이템 max 10개' or '계획은 아이템의 가격을 알고 있음' 등등
- 그러므로 계획 aggregate에 아이템 개념을 넣고, '아이템들'을 포함했다.
- 근데 생각해보니 아이템은 각각 화장품이라서 아주 비대해질 수 밖에 없다.
- 그래서 아이템을 계획에서 분리시켜서 aggregate로 정의한다.
- **아니 근데 규칙상 계획은 아이템의 가격을 알아야되잖아!!!**
- 무한 loop
<br><br>
그림들
<br><br>
- Bounded context의 shared 개념이 쓰여야하는 것이다.
- 계획은 아이템을 하단에 가지되, 필수적으로 알아야할 것들만 안다.
- 계획 아래의 아이템은 (id, 가격)만 안다.
- 계획이 아이템 추가 --> 아이템 aggregate가 추가. 둘 사이의 간극이 있다. (consistency 문제)
