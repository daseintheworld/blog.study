# Ef Core, Lambda

## 잠깐! 배경 이론 훑기

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

## Tutorial

1. [Add .NET Core DI and Config Goodness to AWSLambda Functions](https://blog.tonysneed.com/2018/12/16/add-net-core-di-and-config-goodness-to-aws-lambda-functions/)
2. [IDesignTimeDbContextFactory and Dependency Injection: A Love Story](https://blog.tonysneed.com/2018/12/20/idesigntimedbcontextfactory-and-dependency-injection-a-love-story/)
3. [Use EF Core with AWS Lambda Functions](https://blog.tonysneed.com/2018/12/21/use-ef-core-with-aws-lambda-functions/)

### 참조자료 저자의 목적

1. aws를 사용하며 일반적 접근법이 가능한 Lambda application을 쓴다

2. 그러나 상황에 따라서 Serverless application처럼 풍부한 dependency injection system을 쓸 수 있으면 좋겠다

3. 그러기 위해선 필요한 service의 dependency injection을 직접 구현할 수 있어야겠다.

** 스터디에서는 aws lambda를 동작시키는 것과, .net core의 핵심 중 하나인 dependency injection을 배운다.