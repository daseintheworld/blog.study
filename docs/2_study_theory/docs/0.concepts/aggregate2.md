---
layout: default
title: "• DDD - Aggregate2"
grand_parent: "study(방법론)"
parent: "concepts"
nav_order: 5
has_children: true

---


![](https://3.bp.blogspot.com/-NMgicII-Om4/WRaSUWBv42I/AAAAAAAAAyo/dQaSkoScdPATCm5BrXC3f3F34W-48lTqwCLcB/w1200-h630-p-k-no-nu/DDD-car-aggregate.png)

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
