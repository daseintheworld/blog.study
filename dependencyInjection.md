---
title: dependency injection
permalink: /dependency-injection/
---

# Dependency Injection

### Inversion of control

[Wiki](https://en.wikipedia.org/wiki/Inversion_of_control)

In software engineering, inversion of control (IoC) is a programming principle. IoC inverts the flow of control as compared to traditional control flow. In IoC, custom-written portions of a computer program receive the flow of control from a generic framework. A software architecture with this design inverts control as compared to traditional procedural programming: **in traditional programming, the custom code that expresses the purpose of the program calls into reusable libraries to take care of generic tasks, but with inversion of control, it is the framework that calls into the custom, or task-specific, code.**

Inversion of control is used to increase **modularity** of the program and make it extensible, and has applications in object-oriented programming and other programming paradigms. The term was used by Michael Mattsson in a thesis, taken from there by Stefano Mazzocchi and popularized by him in 1999 in a defunct Apache Software Foundation project, Avalon, then further popularized in 2004 by Robert C. Martin and Martin Fowler.

The term is related to, but different from, the dependency inversion principle, which concerns itself with decoupling dependencies between high-level and low-level layers through shared abstractions. The general concept is also related to event-driven programming in that it is often implemented using IoC, so that the custom code is commonly only concerned with the handling of events, whereas the event loop and dispatch of events/messages is handled by the framework or the runtime environment.

![Image](https://www.intertech.com/Blog/wp-content/uploads/2016/05/IoC.png)

- Control flow를 뒤집는다는 것이 '일반적인' 의미.
<br>
<br>

### Dependency Injection

[Wiki](https://en.wikipedia.org/wiki/Dependency_injection)

n software engineering, dependency injection is a technique whereby one object supplies the dependencies of another object. A "dependency" is an object that can be used, for example as a service. **Instead of a client specifying which service it will use, something tells the client what service to use. The "injection" refers to the passing of a dependency (a service) into the object (a client) that would use it.** The service is made part of the client's state. Passing the service to the client, rather than allowing a client to build or find the service, is the fundamental requirement of the pattern.

***The intent behind dependency injection is to achieve Separation of Concerns of construction and use of objects. This can increase readability and code reuse.***


![Image](https://i.stack.imgur.com/4uPbD.jpg)

### Sample [Link](https://github.com/daseintheworld/sample.netcore-dependencyinjection)

아래와 같이 dependency injection을 안 쓸 경우에는 직접 infrastructure를 참조해야한다.

{% gist ff7a236eabda9126c855dc9235ddf044 %}