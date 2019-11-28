---
layout: default
title: "• lambda와 api구조"
grand_parent: "study(실용)"
parent: "aws lambda"
nav_order: 4
has_children: true
---

### **문서 목적**

* lambda와 http api가 어떤 구조를 가져야하는지 논의한다.
* lambda와 http 외 외부 event를 고려했을 때 프로젝트가 어떤 구조를 가져야하는지 논의한다.


### monolithic function, single-purposed function의 비교

현재 customruntime dotnet의 web api template들은 모두 monolithic function 구조를 얘기하고 있다.

그러나 그 구조가 공학적으로나 업무적으로나 꼭 좋다는 보장이 없고, 아래 링크 글의 글쓴이는 오히려 single-purposed가 우월하다는 주장을 하고 있다.

[AWS Lambda — should you have few monolithic functions or many single-purposed functions? - By Yan Cui](https://hackernoon.com/aws-lambda-should-you-have-few-monolithic-functions-or-many-single-purposed-functions-8c3872d4338f)