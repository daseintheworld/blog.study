---
layout: default
title: "• DDD - Aggregate1"
grand_parent: "study(방법론)"
parent: "concepts"
nav_order: 4
has_children: true

---

## Aggregate?
<br><br>
![](https://flowframework.readthedocs.io/en/stable/_images/DomainModel-5.png)
<br><br>
앞의 내용으로부터..
1. DDD를 표현하는 것은 Ubiquitous language
2. Ubiquitous language가 이용하는 domain model,
3. Domain model에서 사용하는 가장 중요한 패턴 중의 하나가 aggregate

<br><br>그런데 항상 사람 헷갈리게 만드는 개념이므로 다양한 설명을 모아봄
<br><br>

## 1. Aggregate에 대한 다양한 설명
<br><br>

### **martin fawler - [post](https://martinfowler.com/bliki/DDD_Aggregate.html)**
* Aggregate는 Domain-Driven Design의 패턴
<br><br>
* Aggregate은 domain 모델들의 묶음이고 **단위 하나**로 취급된다. 
<br><br>
* Aggregate은 하나의 객체를 root로 가지며 내부의 다른 모델들을 관리한다.
<br><br>
* Aggregate 외부에서는 오로지 root를 통해서만 내부에 접근이 가능하다.
<br><br>

### **eric evans - [post](https://books.google.co.kr/books?id=xColAAPGubgC&pg=PA511&lpg=PA511&dq=A+cluster+of+associated+objects+that+are+treated+as+a+unit+for+the+purpose+of+data+changes.&source=bl&ots=qbZEdiVJ5t&sig=ACfU3U2sE4hUCs2sC8ZNdG_K3epyZNQQAQ&hl=en&sa=X&ved=2ahUKEwiT5bHB457mAhUGa94KHVEWAxQQ6AEwBHoECAoQAQ#v=onepage&q=A%20cluster%20of%20associated%20objects%20that%20are%20treated%20as%20a%20unit%20for%20the%20purpose%20of%20data%20changes.&f=false)**
* 데이터를 수정할 때 한 단위(unit)로 사용하기 위한 관련된 객체들의 모음
<br><br>
* 외부에서 참조하는 것은 aggregate 내의 한 모델에게만 가능하고 root라고 부른다
<br><br>
* 도메인적으로 일관성이(consistency) 있게하는 규칙은 Aggregate 경계 내에서만 적용된다.
<br><br>

### **그 외 수많은 설명충들 중 하나 - [post](https://www.infoq.com/articles/microservices-aggregates-events-cqrs-part-1-richardson/)**
<br><br>
![](https://res.infoq.com/articles/microservices-aggregates-events-cqrs-part-1-richardson/en/resources/figure3.jpg)
<br><br>

* aggregate은 domain model을 **각기 이해하기 쉬운 덩어리들로 쪼개놓은 것**이다.
<br><br>
* aggregate은 조회와 삭제의 단위라서 db에서 통쨰로 가져오고 삭제할 때도 aggregate 내의 모든게 삭제된다.
<br><br>
* aggregate끼리의 참조는 아이디를(primary key)를 이용해서만 한다. 즉, 프로그램 상에서 객체를 직접 바라보지 않고 id만 아는, 느슨한(loosely coupled) 상태 라는 것.
<br><br>
* **(중요)**하나의 트랜잭션은 단 하나의 aggregate만 만들거나 수정한다.

## 2. Consistency 문제의 등장
<br><br>
![](https://pbs.twimg.com/media/D0nSD7yX0AAru3t.jpg)

* '할머니 aggregate'와 '아이 aggregate'가 바라보는 시점이 다르면 순간적으로 (혹은 몇초~n시간까지??) 엉키는(inconsistant) 상태가 이어질 수 있다.
<br><br>
* 의료 HIS 기준 - 오더가 생성되고 얼마 지나야만 환자가 '진료를 본 상태'가 된다.
<br><br>
--> 하나의 트랜잭션이 단 하나의 aggregate만 수정하므로 발생할 수 밖에 없는 문제.
<br><br>
### **그러므로 Event-driven 방식으로 소통하여 그 갭을 메꾼다 : Eventual Consistency**
<br><br>

## 3. Bounded Contexts + Events => Microservices
<br>

[infoq 강의](https://www.infoq.com/presentations/microservices-ddd-bounded-contexts/)(50분, 스크립트 있음)
<br><br><br>
![](https://www.codeproject.com/KB/architecture/1176046/Domain-Events-02.png)
<br><br>
There's a lot of stuff in how do you identify the context, and that's the tricky part. There are several ways, to me this is really the tricky or the hard part. I use some ways as a guideline. One of the things that I do is I ask myself, "Do I need to have transactional consistency when I try to update a couple of fields together?" For example, would I ever need to update the product description and the availability in the same transaction? Probably never. If that's the case, there's a clear indication that those two things do not belong in that same context. I use that as one heuristic.

Do we need transactional consistency for updating the two fields? If not, then we don't have to. The other thing is you could split according to the departments or teams, because if the teams have a natural boundaries or rules and business behavior that they are trying to each accomplish, then that might be also a good start. You could start there and see how it goes. Of course, things are going to evolve, and the boundaries are not just like fixed lines. It's going to change based on conversations that you have with domain experts. You might find by having a conversation a concept that is not well-defined in your model, or you might find that that doesn't quite work well together, and so you might move that to a different context. These are some of the things that you're going to learn by having conversations. The whole idea is, as you learn new things about your domain, about the behavior, about the rules, you try to take that knowledge and then see how you can factor or refactor your existing models to fit that behavior.

In this case, you've got three contexts, sales, inventory, and shipping. In each of these different contexts, you can have a model for product, which is all different. You can have the same attributes, and it doesn't matter. By having this, you're using the language of the domain, and the domain people can also understand when you refer to your model as whatever property. You can say, "What should I do when the weight changes?" You are using the same term, weight, as the same term that they know and understand, so there's no communication problem. That's why it's important to try to use the language of the domain in your code.

The other way is, you can think about looking at the business processes that the business has. One of the things that domain-driven design really pushes is, look at the behavior. The interesting part of the business is in the behavior. If we can capture the behavior of the system in our code, then our code is going to be more aligned with the business. If the whole idea is for us to write software that's well aligned to the business, if we can design our code in such a way, then when they come up with a new requirement, we can go along with those changes to add more features and so on. We just can't get there by like having one model. It evolves, and as rules change, as business requirements change, we just keep learning from the domain and try to refactor our models.

In this case, it's a business process. For example, Amazon gives you $70 off if you buy the prime card. Obviously, there are some business rules that involve you buying a prime card, and it involves something in the shipping context and in the pricing context. Now you can see that there's a lot of participants. You can start to ask yourself, if you wanted to have transactional consistency, what things need to be together? You can start to form your models, and then you can see where it might fit and so on.

Splitting the boundaries is a talk by itself. My company has given me this link. You can access this, this is Udi's advanced distributed design course where he talks about finding boundaries. This is one of the talks of his course that you can have access to, which goes into larger, more detail.