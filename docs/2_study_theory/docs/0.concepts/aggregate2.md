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
### **무명의 논객1**
[출처(1번답변)](https://stackoverflow.com/questions/51243959/how-to-properly-define-an-aggregate-in-ddd)
<br><br>
객체지향을 너무 머릿속에 염두하지는 말자. 교과서와 martin fowler의 이야기는 이미 시간이 조금 지났고 너무 좁은 범위를 다루고 있어. aggregate는 class일 필요도 없고 심각하게 지켜질 필요도 없어. 어떤 경우에는 변화할 필요 없이 "오키, 하던 일이 잘됐다" 정도만 담을 수도 있고.
<br><br>
**중요한건 트랜잭션의 경계**(boundary)야. aggregate의 일은 딱 하나야. 불변식과(invariants) 도메인 규칙을 대부분 상황에서 지키고 중요한 데이터를 관리하는 것. 트랜잭션 경계가 의미하는 것은 aggregate가 무언가를 '아마', 그리고 '이미' 한 것을 의미해. 세상 어느 것도 그것에 모순되면(contradict) 안되고, 만약 그런 일이 일어났다면 aggregate 설계가 잘못된 것이라서 그 모순을 aggregate 내로 끌어들여 해결해야돼.
<br><br>
그래서 aggregate를 설계할 때 나는 아주 간단하게 시작해서 점점 진화시켜. **단순한(static) 함수**를 생각해서 (1)모든 VO(value object)나 엔티티나(entity) 커맨드 데이터를 받아들이고, (2) **도메인 규칙에 의해서 그 행위를 체크**하고 (3) 뭔가가 일어났다고 **도메인 이벤트를 날리는 것을** 그려봐. (4)이벤트의 데이터는 그 시스템의 정합성이 지켜지도록(persist the changes) 하는데 필요한 모든 정보를 가지고 있어야하고, 다른 aggregate에 결국 도달할 거야. 그 다른 aggregate는 같은 bounded context내 에서든 다른 bounded context든 예외는 없어.
<br>
--생략--
<br><br><br>
### **무명의 논객2**
[출처(2번답변)](https://stackoverflow.com/questions/51243959/how-to-properly-define-an-aggregate-in-ddd)
<br><br>
aggregate을 설계할 때 항상 생각해야할 것은 **어느 aggregate에 소속되어있는 엔티티가 다른 곳에 가서는 aggregate로 역할을 할 수 있다**는 것이다. 그래서 CarStructureAggregate는 EquipmentTypes를 포함할 수 있으면서도  EquipmentTypeAggregate를 기획해 자기 나름의 것들을 넣고 비즈니스 규칙을 넣을 수 있다.
<br><br>
그래서 이 aggregate들이 자기네 구성원(properties)만 수정할 수 있다는 것을 명심해야한다. 만약 CarStructureAggregate가 EquipmentType를 수정하고 싶다고 CarStructureAggregate를 수정하는 상황에서 수행할 수는 없고 반드시 EquipmentType aggregate를 조회해와야한다. CarStructureAggregate는 아마 단순한 add/remove 로직만 갖고 있을 것이다.
<br>
--생략--
<br><br><br>
### **무명의 논객3**
[출처(1번답변)](http://domain-driven-design.3010926.n2.nabble.com/Bounded-Contexts-and-Aggregates-td6460442.html)
<br><br>
> 내가 잘 하고 있는지 알려줘 제발.
> Bounded context가 여러 aggregate roots를 가질 수 있지?
<br><br>
**답변**
<br>응
<br><br>

> aggregate 경계가 있는데 나는 이게 bounded context와는 다른 경계라고 생각하고 있어
<br><br>
**답변**
<br> aggregate 경계는 객체들을 모아놓은 것들에(clustered objects) 대해 불변식(invariant)을 적용하는 거에 관한 거야. Bounded context는 언어적인(linguistic) 표현에서의 일관성(consistency)를 얘기하는거고, 거기 안에서 표현하는 모델은 **항상 정확히 같은 그것을** 가리키고 있어야해.
<br><br>

> 이 구조가 맞을까? Order (AR) => Line(Entity) => [Product(AR)]
<br>--- 참고. AR은 augmented reality ([link](https://whatis.techtarget.com/definition/augmented-reality-AR))
<br><br>
**답변**
<br>
이론적으로는 맞아. 하지만 product는 id를 통해서 간접적으로(inferred association) 관계를 가져가는 것이 바람직해보여. 그러지 않으면 **product의 변화가 Line과 Order를 변화시킬 건데, 그게 aggregate의 경계에 대한 정의를 위배**하는 거야.(왜 그런지 깊이 고민하면 좋겠습니다-기범). 예를 들어 Product의 가격이 변화하면 고객 당 order의 한계 가격을 위배하게될 수도 있어. 그러므로 Order로부터 분리된 Bounded context를 Product가 가져가는 것이 좋아보여. 그러면 직접적으로 product를 참조하고 있을 필요가 없게돼.
<br><br>

## 2. master - detail 관계에서 Aggregate 구성
<br><br>
위의 설명들을 고려하여 아주 간단하지만, 저는 2년 내내 고민해왔던 가장 기본적인 구성에 대해서 논해봅니다.
<br><br>
1. **문제**
* 아주 많은 관계들의 기본 구조가 1-N이다.(master-detail / parent-children / category-items)
<br>
* 일단 master는 aggregate라 치고... 그렇다면 **detail은 aggregate일 지 아닐지 어떻게 알까?**
<br><br>
* Ex. 다영 책임의 화장품 계획 - 화장품 아이템 관계
- 가정. 계획과 아이템을 잇는 규칙이 있음: '계획 1 => 아이템 max 10개' or '계획은 아이템의 가격을 알고 있음' 등등
- 그러므로 보통 계획 aggregate에 아이템 개념을 넣고, 그 aggregate 안에 '아이템들'을 포함한다. 이제 계획이 아이템을 체크한다. (나이스!)
<br><br>
- 근데 생각해보니 각각 화장품인 아이템은 아주 많은 정보를 갖고 있어서 점점 비대해질 수 밖에 없다.
- 그래서 아이템을 계획에서 분리시켜서 aggregate로 정의한다.
- **아니 근데 그럼 화장품 계획은 아이템의 가격을 어떻게 알지??**
<br><br>
- 무한 loop
<br><br>
2. **이미지로 문제와 해결 과정을 설명**
<br><br>
* **행위를 도출**
![](https://user-images.githubusercontent.com/55048882/70953471-b9175800-20ac-11ea-952a-2d73400f01c1.png)
<br><br>
* **aggregate를 합치자 아이템이 커질 것 같다**
![](https://user-images.githubusercontent.com/55048882/70953488-c3d1ed00-20ac-11ea-8587-8e106ccf312d.png)
<br><br>
* **aggregate를 분리하자 몇몇 규칙을 사용하지 못한다**
![](https://user-images.githubusercontent.com/55048882/70953495-cf251880-20ac-11ea-84c2-b1d48050241e.png)
<br><br>
* **최종 형태**
![](https://user-images.githubusercontent.com/55048882/70953511-dd733480-20ac-11ea-9c1e-52818c43f120.png)
<br><br>
- Bounded context의 **shared 개념이 화장품 아이템의 가격**이 된다.
- 계획은 아이템을 하단에 가지되, 필수적으로 알아야할 것들만 안다.
- 계획 아래의 아이템은 (id, 가격)만 안다 --> 항상 최대한 작은 규모를 유지하게 된다.
<br><br>
3. **부가문제**
<br><br>
* **부가 문제 1**
-- 계획의 (아이템 추가) ---시간 간격---> (아이템 aggregate의 추가).
<br>트랜잭션 분리가 되면 생기는 둘 사이의 간극의 문제?
<br>전형적인 consistency 문제이며, 일단 다음 단계에서 고민해야한다.
<br><br>
* **부가 문제 2**
-- 만약 그럼에도 화장품 계획의 아이템이 아주 많아져서 aggregate 데이터가 너무 쌓이면?
<br>대부분의 상황은 lazy-loading의 기법으로 해결가능하며 ef core도 가지고 있다.
<br>그러나 어떤 경우에는 원천적으로 안어울릴 경우도 있는데, 이는 도메인이 잘못 짜였거나 (아주) 드물게 애초에 DDD가 안 어울리는 모델일 수 있다.