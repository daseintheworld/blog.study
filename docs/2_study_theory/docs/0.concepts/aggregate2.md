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
<br><br>
Be careful about having too strong OO mindset. The blue book and Martin Fowler post are a little bit old and the vision it provides is too narrow.

An aggregate does not need to be a class. It does not need to be persisted. Theese are implementation details. Even, sometimes, the aggregate do things that does not implies a change, just implies a "OK this action may be done".

iTollu post give you a good start: What matters is transactional boundary. The job of an aggregate is just one. Ensure invariants and domain rules in an action that, in most of the cases (remember that not always), change data that must be persisted. The transactional boundary means that once the aggregate says that something may, and has, be done; nothing in the world should contradict it because, if contradiction occurs, your aggregate is badly designed and the rule that contradict the aggregate should be part of aggregate.

So, to design aggregates, I usualy start very simple and keep evolving. Think in a static function that recives all the VO's, entities and command data (almost DTO all of them except the unique ID of the entities) needed to check domain rules for the action and returns a domain event saying that something has be done. The data of the event must contain all data that your system needs to persist the changes, if needed, and to act in consequence when the event reach to other aggregates (in the same or different bounded context).

Now start to refactoring and OO designing. Supress primitive obsession antipattern. Add constraints to avoid incorrect states of entities and VO's. That piece of code to check or calculate someting related to a entity better goes into the entity. Put your events in a diet. Put static functions that need almost the same VO's and entities to check domain rules together creating a class as aggregate root. Use repositories to create the aggregates in an always valid state. And a long etc. You know; just good OOP design, going towards no DTO's, "tell, don't ask" premise, responsibility segregation and so on.

When you finish all that work you will find your aggregates, VO's and entities perfectly designed from a domain (bounded context related) and technical view.
### **이름 모를 논객2**
[출처(2번답변)](https://stackoverflow.com/questions/51243959/how-to-properly-define-an-aggregate-in-ddd)
<br><br>
Something to keep in mind when designing aggregates is that the same entity can be an aggregate in one use case and a normal entity in another. So you can have a CarStructureAggregate that owns a list of EquipmentTypes, but you can also have an EquipmentTypeAggregate that owns other things and has its own business rules.

Remember, though, that aggregates can update their own properties but not update the properties of owned objects. For example if your CarStructureAggregate owns the list of EquipmentType, you cannot change properties of one of the equipment types in the context of updating the CarStructureAggregate. You must query the EquipmentType in its aggregate role to make changes to it. CarStructureAggregate can only add EquipmentTypes to its internal list or remove them.

Another rule of thumb is only populate aggregates one level deep unless there is an overriding reason to go deeper. In your example you would instantiate the CarStructureAggregate and fill the list of EquipmentTypes, but you would not populate any lists that each EquipmentType might own.
### **이름 모를 논객3**
[출처(1번답변)](http://domain-driven-design.3010926.n2.nabble.com/Bounded-Contexts-and-Aggregates-td6460442.html)
<br><br>
> I just want to make sure I am on the right track.
> Bounded contexts can include many aggregate roots, correct?

Correct.

> There is an aggregate boundary, but I assume that this is different > from the bounded context, which separates models of different kinds.

Aggregate boundaries are about consistency of invariants among clustered objects. Bounded contexts are about linguistic consistency, where a term in a model always means the same thing because the context is explicitly bounded.

> Order (AR) => Line(Entity) => [Product(AR)]

In principle this is true. However, your reference to Product is best accomplished by id only (inferred association), and using a copy of other parts such as description and price. Otherwise a change in Product details will change the Line and thus the Order, which may violate the aggregate boundary. Imagine that Product price changes, which ripples to violate a maximum Order total limit per Customer. Further, it is possible and even probable that Product is in a separate bounded context from Order. In that case it makes no sense to directly reference Product even if there was no danger of violating aggregate boundaries.

Vaughn

## 2. master - detail 관계에서 Aggregate 구성
<br><br>
매우 심플하지만 저는 2년 내내 고민해왔던 아주 기본적인 구성에 대해서 논해봅니다.
<br><br>
1. **문제**
* 아주 많은 관계들의 기본 구조가 master-detail / parent-children / category-items의 1-N이다.
<br>
* master는 aggregate라 치고... **detail은 aggregate일 지 아닐지 어떻게 알까?**
<br><br>
* ex. 다영 책임의 화장품 계획 - 화장품 아이템 관계
- 가정 : 계획과 아이템은 규칙이 있음: '계획 1 => 아이템 max 10개' or '계획은 아이템의 가격을 알고 있음' 등등
- 그러므로 계획 aggregate에 아이템 개념을 넣고, 그 aggregate 안에 '아이템들'을 포함했다.(나이스!)
<br><br>
- 근데 생각해보니 각각 화장품인 아이템은 아주 많은 정보를 갖고 있어서 점점 비대해질 수 밖에 없다.
- 그래서 아이템을 계획에서 분리시켜서 aggregate로 정의한다.
- **아니 근데 그럼 화장품 계획은 아이템의 가격을 어떻게 알지??**
<br><br>
- 무한 loop
<br><br>
2. **해결**
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
- Bounded context의 shared 개념이 쓰여야하는 것이다.
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