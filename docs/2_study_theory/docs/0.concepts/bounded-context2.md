---
layout: default
title: "• DDD: BoundedContext2"
grand_parent: "study(방법론)"
parent: "concepts"
nav_order: 4
has_children: true

---

bounded context와 어플리케이션 구조

## 1. Bounded Contexts 찾기
<br>
[infoq 강의](https://www.infoq.com/presentations/microservices-ddd-bounded-contexts/)(50분, 스크립트 있음)
<br><br><br>
![](https://www.codeproject.com/KB/architecture/1176046/Domain-Events-02.png)
<br><br>
* context를 찾아내는 방법이 여러가지 있는데 가장 까다롭고 어려운 부분이다.
<br><br>
* 내(강의자)가 쓰는 방법은 "이 몇가지 필드(fields)를 업데이트 했을 때 '트랜잭션의 일관성'(transactional consistency)을 꼭 유지시켜야하는가?"를 묻는 것이다. 예를 들어 상품 설명을 업데이트해도 상품이 진열 가능한지 여부와는 상관 없을 것이다.
<br><br>
* 또다른 방법은 팀에 자연스럽게 경계/규칙/비즈니스 행위들이 이미 있고 그게 중요하다고 다들 인식하는 상태라면 좋은 출발지점으로 삼을 수 있다. (우리 Cloud HIS 프로젝트의 유일한 -하지만 매우 강력한- 강점)
<br><br>
* 또한 비즈니스 프로세스를 생각해볼 수 있다. DDD에서 강하게 추천하는 것이 바로 행위(behavior)이다. 비즈니스의 재밌는 부분은 행위에 있고 그걸 코드에 녹였을 때 비즈니스와 같이가는 모델을 만들 수 있다.
<br><br><br><br>


## 2. Bounded Context는 어떻게 소통하는가?

<br><br><br>
### **지난 내용으로부터**
* 우리는 통합된(unified) 모델은 좋을 것이 없다는 사실을 알았고 서로 다른 개념 덩어리(contexts)들을 이용하면 각 모델이 잘 진화할 수 있다는 것을 알게 되었다. 그러면 이 개념들(contexts)이 서로 소통하지(communicate) 못하면 무슨 의미가 있을까? 결국 다같이(cohesive) 동작하는 실제 시스템을 만들기 위해서는 통신이 필요하고, 그게 이벤트 기반의 아키텍처가 들어오는 시점이다.  
<br><br><br>
### **이벤트**(event)
 나는(강의자) 이 시점에서 메시지(message)와 이벤트(event)를 생각하게 된다. 메시지는 커맨드와(command) 이벤트(event)로 이루어진다. 이벤트는 어떤 중요한 일이 bounded context 내에 일어났음을 옮겨줄 때 사용하는 메시지다. bounded context 사이에서 상태(state)의 변화를 옮겨갈 때 사용된다. 반대로 커맨드는 의도(intent)를 옮겨주고, 실패할 수 있다. 그게 아주 큰 차이다.
<br><br>
이벤트는 발행(publish)될 수 있고, 여러 구독자가 받을 수 있는데, 커맨드는 주로 하나의 구체적인 서비스에 전달하게 된다. 이벤트는  bounded context 사이의 소통의 대부분  이벤트를 쓰고 싶을 것이다. 
<br><br>
### **커맨드**(command)
 커맨드는 bounded context 내에서 서로 통신할 때 써서 느슨히 결합된(loosely coupled) 구조를 만들 수도 있다. 커맨드는 실패할 수도 있다. "300"을 시청했던 학생이 있나? Xerxes가 스파르타에 사절을 보냈고 "무릎 꿇어라"라고 말했고 끝이 안좋았다. 
<br><br><br>
![](https://user-images.githubusercontent.com/55048882/70911261-db7d8700-2054-11ea-994e-4ddde5311b62.gif)
<br><br>
실패한 커맨드 :)
<br><br>
결론적으로 **Bounded context는 서로 뭔가 시키지 않고 독립적이며, 그래서 이벤트를 이용해 소통한다.** 
<br><br>
(강의다 보니 쉽게 잘 설명해주는 대신 분량이 많아서.. 아래에 요약만 하였습니다... 화이팅!)

```
I want to take example of a business process, just to walk through how we would do this with domain-driven design and have this communication using events. The requirement is when an aircraft type has changed, the aircraft company wants to notify the passenger saying, "You have a new booking proposal." The passenger has the right to either accept it or cancel it,

This is an example Norwegian sent me, very kindly said, "Sorry, we canceled your flight to Rome, here's your new flight. If you want it, then you can take it. Otherwise, you can get your money back." A business process, if you look at it, it can be triggered by an event from one bounded context to the other. You've got the flight planning context saying that, "This aircraft flight has changed." The booking context can now receive that event and then go and act on whatever it needs to do. The keyword here is when.

When the domain experts or the people in the business starts to use the language when and then start describing something, there's usually event that follows that you can get out of. The thing to understand here is we said that messages help you design your systems to be loosely coupled. The whole point is we don't want to be coupled. However, we do have to be careful in how we design these schemas because when you send messages over from one context to the other context, you are going to be sharing the schema. It becomes really important what you put in those events and what you put in those messages. You could put a lot of information from one bounded context in that event, and now you are definitely coupled via the event.

What happens when this flight planning bounded context changes the schema of the event? How is that going to affect this booking context? You have to ask yourself and be very intentional about the data that is being shared from one bounded context to the other. Just because we use events and messages doesn't automatically give you Nirvana and happiness. We still need to pay attention to what we put in the messages and how we design the schema.

The other thing is, the business processes, there could be multiple messages that takes part in the same process. The booking context receives the aircraft type has changed event, but then internally it might need to do a lot of things, which might involve sending messages, "We need to rebook this flight, and we need to notify this customer," and "what happens when the customer said, "No, I don't want this. Go ahead and cancel this booking"? There's a lot of events that are going to participate in that same process.

When you have a lot of messages that are participating in the same process, you might have to have state involved, because based on the state, if the customer canceled, then you don't need to send the customer a rebooking email or whatever because they canceled it. Based on the state, you are going to take certain decisions, different decisions. It's important how we manage the state, and that's where we have this pattern called sagas, and it can come in handy.
```
### **요약**
* 내(강의자)가 겪었던 일화: 항공사가 임의로 티켓을 캔슬시키게 되어 선택권을 줌.
* 나는 티켓을 다시 받거나, 돈을 모두 다시 돌려받거나 선택을 해야함
* 이러한 경우 항공사라는 context로부터 발생한 이벤트는 나의 결정에 의해 흐름이 흘러가게 된다.
* 항공사는 메시지만 던지고 기다리면 되므로, 느슨한 결합(loosely coupled)이 된 형태가 된다.
<br><br>
* 이 때 이벤트는 양쪽 context가 같이 공유하고 있으므로 결합이(coupled) 생긴다. 
* 이벤트에 너무 많이 정보가 담기면 결합도가 높아질 수 밖에 없고, 우리는 **이벤트를 기획할 때 아주 깊이 고민해야**한다
* 이벤트는 니르바나나 유토피아로 우릴 인도하지 않는다.
<br><br>
* 다른 관점에서 보면, 하나의 비즈니스 프로세스에 대해서 여럿의 메시지가 관여하게 된다. 
* 항공사의 일이 생겨 고객에게 이벤트를 주고 캔슬시키기까지 많은 이벤트가 개입한다.
* 각 이벤트에 의해 생기는 일들이 자연스럽게 상태(state)를 정의해야 그것을 토대로 다음 판단을 할 수 있다.
* 예를 들어, 고객이 캔슬했다면 취소 상태로 티켓이 바뀌어야 한다.
* 상태를 잘 정의하고 관리하는게 매우 중요하며 상태들은 이벤트가 흘러감에 따라 계속 바뀐다. 그래서 **우리는 이것을 긴이야기/대서사(saga)라 부르며,** 항상 다루기 까다로운 주제로 생각한다.
<br><br>
## 3. Saga pattern
<br><br>
주제를 살짝 벗어난 것이지만, 우리 프로젝트가 생각치 못하고 있던 얘기이므로 간단하게나마 소개.
<br><br>
[출처](https://medium.com/@tomasz_96685/saga-pattern-and-microservices-architecture-d4b46071afcf)
<br><br>
![](https://miro.medium.com/max/403/1*eCBBG3GQdJErCGE46XaLLg.png)
<br><br>
microservice들이 각자 '멋대로' action의 흐름을 만들면 점점 dependency hell에 빠져 분석 불가능한 상태가 됨. (cloud HIS)
<br><br>
![](https://miro.medium.com/max/360/1*zV1tgxZIMguU8Giw2OqXdg.png)
<br><br>
microservice들이 프로세스를 딱 정하고 흐름을 무조건 서로 격리시키려고하면, 필연적으로 중복된 기능을 구현하는 수많은 서비스가 나옴
<br><br>
![](https://miro.medium.com/max/504/1*AbRVEDPfSuT6f3RawxydOw.png)
<br><br>
action poll을 나누고 action flow를 정의한다. 그 흐름대로 안가면 해당하는 Saga가 실패하는 것이다.