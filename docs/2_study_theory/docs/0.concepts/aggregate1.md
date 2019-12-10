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

<br><br>Bounded context까지는 추상화 되었었다면, domain model부터는 구체적인 방법론과 그 이유가 생김.
<br><br>

## 1. Aggregate에 대한 다양한 설명
### **eric evans - [e-book 꼭 읽기를..](https://books.google.co.kr/books?id=xColAAPGubgC&pg=PA511&lpg=PA511&dq=A+cluster+of+associated+objects+that+are+treated+as+a+unit+for+the+purpose+of+data+changes.&source=bl&ots=qbZEdiVJ5t&sig=ACfU3U2sE4hUCs2sC8ZNdG_K3epyZNQQAQ&hl=en&sa=X&ved=2ahUKEwiT5bHB457mAhUGa94KHVEWAxQQ6AEwBHoECAoQAQ#v=onepage&q=A%20cluster%20of%20associated%20objects%20that%20are%20treated%20as%20a%20unit%20for%20the%20purpose%20of%20data%20changes.&f=false)**
* Aggregate는 Domain-Driven Design의 패턴
<br><br>
* 데이터를 수정할 때 한 단위(unit)로 사용하기 위한 관련된 객체들의 모음
<br><br>
* 외부에서 참조하는 것은 aggregate 내의 한 모델에게만 가능하고 root라고 부른다
<br><br>
* 도메인적으로 일관성이(consistency) 있게하는 규칙은 Aggregate 경계 내에서만 적용된다.
<br><br><br><br>

### **martin fawler - [post 3분따리 글](https://martinfowler.com/bliki/DDD_Aggregate.html)**
* Aggregate은 domain 모델들의 묶음이고 **단위 하나**로 취급된다.
<br><br>
* Aggregate은 하나의 객체를 root로 가지며 내부의 다른 모델들을 관리한다.
<br><br>
* Aggregate 외부에서는 오로지 **root를 통해서만 내부에 접근이 가능**하다.
<br><br><br><br>

### **부가 설명 - [post](https://www.infoq.com/articles/microservices-aggregates-events-cqrs-part-1-richardson/)**
<br><br>
![](https://res.infoq.com/articles/microservices-aggregates-events-cqrs-part-1-richardson/en/resources/figure3.jpg){:height="400px" width="300px"}
<br><br>
** 위 이미지에서 '?'의 의미는?

* aggregate끼리의 참조는 아이디를(primary key)를 이용해서만 한다. 즉, 프로그램 상에서 객체를 직접 바라보지 않고 id만 아는, 느슨한(loosely coupled) 상태 라는 것.
<br><br>
* **(중요)**하나의 트랜잭션은 **단 하나의 aggregate만** 만들거나 수정한다.
<br><br>

** 의문: 그렇다면 aggregate끼리 연관관계 혹은 소통을 어떻게 하는가?
<br><br><br>
### **Event-driven 방식으로 소통하여 그 갭을 메꾼다 : Eventual Consistency**
<br><br><br>

## 2. Bounded Contexts + Events => Microservices
<br>
[infoq 강의](https://www.infoq.com/presentations/microservices-ddd-bounded-contexts/)(50분, 스크립트 있음)
<br><br><br>
![](https://www.codeproject.com/KB/architecture/1176046/Domain-Events-02.png)
<br><br>
1. Boundary 찾기
* context를 찾아내는 방법이 여러가지 있는데 가장 까다롭고 어려운 부분이다.
<br><br>
* 내가 쓰는 방법은 "이 몇가지 필드(fields)를 업데이트 했을 때 '트랜잭션의 일관성'(transactional consistency)을 꼭 유지시켜야하는가?"를 묻는 것이다. 예를 들어 상품 설명을 업데이트해도 상품이 진열 가능한지 여부와는 상관 없을 것이다.
<br><br>
* 또다른 방법은 팀에 자연스럽게 경계/규칙/비즈니스 행위들이 이미 있고 그게 중요하다고 다들 인식하는 상태라면 좋은 출발지점으로 삼을 수 있다. (우리 Cloud HIS 프로젝트의 유일한 -하지만 매우 강력한- 강점)
<br><br>
* 또한 비즈니스 프로세스를 생각해볼 수 있다. DDD에서 강하게 추천하는 것이 바로 행위(behavior)이다. 비즈니스의 재밌는 부분은 행위에 있고 그걸 코드에 녹였을 때 비즈니스와 같이가는 모델을 만들 수 있다.
<br><br><br><br>
2. Bounded Context는 어떻게 소통하는가?
<br><br>
Just to get a quick check. We're at a place now, we've clearly identified unified models are not great because having one model to rule them all is a bad idea, and we clearly understand that we need to have different context, and by having different contexts, models can live in the context and evolve freely and so on. What's the point of having all these contexts if we don't communicate between the contexts? In order to have cohesive behavior for your system to be an actual system, these contexts need to communicate. To me, that's where event-driven architecture comes in. You want to have this communication mechanism in such a way, it reduces complexity, it's loosely coupled, so you can evolve it and you can scale it, etc.

To me, that's where messages and events come in. If we look at messages, you can classify them as commands or events. These are the two main category of messages that you can use to communicate between bounded context. Events are a message that conveys something of significance has happened in the business. It's used to communicate the state change from one bounded context to the other. Commands, on the other hand are convey intent, and commands can fail. That's the huge difference.

Events are something that get published, so multiple subscribers are going to receive it, versus a command is usually sent to one particular service or context or something. Typically, you want to use events as a communication mechanism between bounded contexts. You can use commands as a way to decouple or have loosely coupled communication style within the bounded context. The one thing about commands is that it can fail. Has everyone watched "300" in this room? When Xerxes sends his messenger to Sparta, to Leonidas and say, "You shall bend your knee to Xerxes," it didn't go so well. If you think about bounded context, they are two separate things, and they don't tell each other what to do. You can think about bounded context as isolated things. They do communicate but using events.

I want to take example of a business process, just to walk through how we would do this with domain-driven design and have this communication using events. The requirement is when an aircraft type has changed, the aircraft company wants to notify the passenger saying, "You have a new booking proposal." The passenger has the right to either accept it or cancel it,

This is an example Norwegian sent me, very kindly said, "Sorry, we canceled your flight to Rome, here's your new flight. If you want it, then you can take it. Otherwise, you can get your money back." A business process, if you look at it, it can be triggered by an event from one bounded context to the other. You've got the flight planning context saying that, "This aircraft flight has changed." The booking context can now receive that event and then go and act on whatever it needs to do. The keyword here is when.

When the domain experts or the people in the business starts to use the language when and then start describing something, there's usually event that follows that you can get out of. The thing to understand here is we said that messages help you design your systems to be loosely coupled. The whole point is we don't want to be coupled. However, we do have to be careful in how we design these schemas because when you send messages over from one context to the other context, you are going to be sharing the schema. It becomes really important what you put in those events and what you put in those messages. You could put a lot of information from one bounded context in that event, and now you are definitely coupled via the event.

What happens when this flight planning bounded context changes the schema of the event? How is that going to affect this booking context? You have to ask yourself and be very intentional about the data that is being shared from one bounded context to the other. Just because we use events and messages doesn't automatically give you Nirvana and happiness. We still need to pay attention to what we put in the messages and how we design the schema.

The other thing is, the business processes, there could be multiple messages that takes part in the same process. The booking context receives the aircraft type has changed event, but then internally it might need to do a lot of things, which might involve sending messages, "We need to rebook this flight, and we need to notify this customer," and "what happens when the customer said, "No, I don't want this. Go ahead and cancel this booking"? There's a lot of events that are going to participate in that same process.

When you have a lot of messages that are participating in the same process, you might have to have state involved, because based on the state, if the customer canceled, then you don't need to send the customer a rebooking email or whatever because they canceled it. Based on the state, you are going to take certain decisions, different decisions. It's important how we manage the state, and that's where we have this pattern called sagas, and it can come in handy.
<br><br><br><br>

## 3. Consistency 문제의 등장
<br><br>
-- BESTCare2.0은 서비스적으로나 시스템적으로 'whole in one.' 트랜잭션 하나에서 말 그대로 모든 database가 접근이 가능했다.
<br>
-- 위 방법은 익히 알다시피 복잡도를 너무 향상시켜왔고 그래서 DDD 방법론에서는 aggregate이 트랜잭션 단위가 된다.
<br><br><br>
![](https://pbs.twimg.com/media/D0nSD7yX0AAru3t.jpg)
<br><br>
### 위 그림에서 본질적이고 **인식론적인** 모순이 읽혀야 한다.
### MSA 기반, 혹은 더 근본적으로 **'data가 세계의 상태를 나타내는'** 컴퓨터 기술 기반 서비스에서 트랜잭션이 분리된다는 것은 현실의 논리에 맞지 않는 상태가 반드시 생길 수 밖에 없다는 것을 의미한다!
<br><br>
* 의료 HIS 기준에서의 문제 - a 서비스가 오더를 생성하고 얼마 지나야만 다른 b,c,d 서비스의 상태가 '진료를 본 상태' 등등으로 바뀐다.
<br><br>
--> b,c,d 서비스 사용자의 불만이 나타난다면, 이 상황을 어떻게 해결해야 하는가?
<br>
--> 가장 전통적인 대답은 '실시간으로 a서비스를 체크해본다'는 것이며 이 시각은 이미 위 그림에 드러난 세계관을 읽지 못하는 것이다.
<br><br><br>
그리고 당연하게도 현재는 이 전통적인 대답을 하는 사람들이 더 많은 상태. 각각의 독립된 세계(서버)가 서로 다른 시간축에서 활동하며, 우리는 '서로의 세계를 침범 안 시키는 선에서' 최대한 그 간극을 서비스의 형태로 줄일려고 노력하는 것. 그것이 MSA 기반 서비스의 가장 중요한 핵심이라 생각한다.(기범)

