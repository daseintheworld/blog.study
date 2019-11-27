---
layout: default
title: "• plan"
parent: "aws architecture"
grand_parent: "backend project"
nav_order: 0
---
## backend architecture implementation plan
<br>architecture 생성 시 각 단계 이전의 계획을 기록.
<br>--> 추후 실제 과정(progress)에서 달라진 점과 비교해보기 위함.

## 0. Goal Specs

* Lambda 하나당 REST method 한개
* Project 세팅 - Lambda, Application, Domain, Infrastructure
* Dependency Injection 체계 갖추기
* Ef core 사용한 data 관리
* Document Db를 이용한 CQRS 패턴 구현
* Message Queue (SQS) 사용한 backend 통신
* Environment 세팅(local, production)
* Unit test 가능
* Local test 가능

## 1. dotnet-lamdba template

* tmeplate
<br>aws의 visual studio toolkit을 사용
<br>template 예상: lambda dotnet project -> custom runtime template

<br>-- serverless (webapi)를 안한다면 이유:
<br>각 lambda를 분리하는게 낫다 생각.
<br><br>-- custom runtime을 선택한다면 이유
<br>아래 링크에서 보면 알 수 있듯이 custom runtime에 있는 bootstrap code로부터 logging이나 여러가지 부가 기능을 얻을 수 있는 듯
<br>[Tutorial – Publishing a Custom Runtime - AWS Lambda](https://docs.aws.amazon.com/lambda/latest/dg/runtimes-walkthrough.html)
<br>

## 2. AWS.Lambda.AspNetCoreServer?

* template: serverless template --> dotnet web api

위의 template을 이용하면 아래 링크를 보면 dotnet web api를 그대로 serverless로 사용할 수 있다.
[Creating a Serverless Application with ASP.NET Core, AWS Lambda and AWS API Gateway \| Jerrie Pelser's Blog](https://www.jerriepelser.com/blog/aspnet-core-aws-lambda-serverless-application/)

특히 아래 그림이 기존의(위) web api hosting service를 aws lambda를 이용해 serverless로(아래) 구현하는 과정을 잘 보여준다.

![normal-flow.png](https://www.jerriepelser.com/static/127492310c06cbddb85b1349a217328d/34e8a/normal-flow.png)

![serverless-flow.png](https://www.jerriepelser.com/static/159399e18c314714a29150dec753105d/34e8a/serverless-flow.png)

aws api proxy ==> lambda ==> asp dotnet hosting

* 예상 장점
<br>-- 우리가 익숙한 asp dotnet 기반 legacy code를 최소한으로 고치고 개발할 수 있는 방법이라는 생각이 든다.
* 예상 단점
<br>-- API layer가 명실상부 두개가 있는 전혀 안예쁜 구조.
<br>-- lambda가 한개가 있다 --> versioning이 문제가 된다. (어떤 http 서비스에 대해 versioning 해야한다면?)
<br>-- 이 패턴으로는 다른 event를 받아야할 때 전혀다른 패턴을 추가해야한다. 특히 message queue 서비스는 분명히 쓸텐데.
<br>-- api gateway가 단 한개가 있다 --> 한개의 function이 모든 http event를 받아들이므로 모든 logging 서비스나 monitoring system을 사용할 때 불편함이 있을 것이다 (추측)

## 3. asp dotnet core and aws serverless

분석을 해보다보니 두개의 키워드 묶음이 잘못되어있다.
<br>-- asp dotnet core는 web service hosting 특히 MVC 패턴을 구현할 때 사용하는 library
<br>-- aws의 serverless template은 SAM(serverless application model)을 구현할 때 사용하는 것

<br><br>두 개의 키워드를 이용해서 구글링 등을 하면 자동으로 visual studio aws template의 serverless/dotnet webapi template이 나온다. 그래서 마치 같이 묶어서 생각해야하는 듯한 착각을 자꾸 만들게 된다. 예를 들어 아래의 링크의 블로그를 볼 때도 그렇다. 이전 주제에서 언급했듯 사실상 hosting service를 한개의 lambda가 담당하는 구조를 그려놨다.

<br>[.NET Core 3.0 on Lambda with AWS Lambda’s Custom Runtime \| AWS Developer Blog](https://aws.amazon.com/blogs/developer/net-core-3-0-on-lambda-with-aws-lambdas-custom-runtime/)

<br>하지만 두 개념은 완전히 분리된 것이며 구현도 분리해서 해야하는 것 같다.
<br>특히나 scalable한 서비스를 기획할 것이므로 그렇다. 물론 scalable이 기술적으로 그런지, 업무적으로 그런지, 그것을 뛰어넘어 비즈니스 통찰에 있어서인지는 정의가 어렵기는 하다. 그러나 단순하게 생각하여 backend를 구성하는 기능들이 최대한 쪼개져야 인식하기 쉬운 것이 자명하다.
<br>그렇게 생각했을 때 한개의 lambda 단위가 아니라 serverless application model을 기획하는 것은 당연하며, 그렇게 기획된 application이 꼭 내부적으로 web api function을 구현할 필요가 없다. 사실상 web service hosting을을 담당하는 것이 lambda에 붙을 api proxy이며, serverless template의 "Events" section에 표현되는 것으로 충분하기 떄문이다.
<br><br>**따라서 asp dotnet core를 걷어내고 SAM, DotnetCore 기능만을 사용하여 application을 만들 수 있어야한다. 그렇게 구현한 application은 꼭 api event 뿐 아니라 다른 종류의 event에 대해서도 쉽게 extend가 될 수 있는 구조일 것이다.**

## 4. DotnetCore and SAM benchmarking

이전 주제에서 에서 이야기한 그런 관점에서 개념을 잘 정립하고 sampling을 한 것 같은 자료는 지금까지 아래 링크에서 볼 수 있다.
* blog
[Fast growing architectures with serverless and .NET Core - Samuele Resca](https://samueleresca.net/2018/12/fast-growing-architectures-with-serverless-and-net-core/)
* github
[examples/aws-dotnet-rest-api-with-dynamodb at master · serverless/examples · GitHub](https://github.com/serverless/examples/tree/master/aws-dotnet-rest-api-with-dynamodb)

아래의 그림에서 보듯이 각각의 api에 대해서 각 lambda를 정의하고 있고, github sample은 aspdotnet을 보고 있지 않다.

![](https://i2.wp.com/samueleresca.net/wp-content/uploads/2018/11/Screenshot-2018-11-25-at-00.15.33.png?resize=960%2C516&ssl=1)

* blog2
아래 링크의 내용 또한 lambda를 고려했을 때 환경설정에 대한 좋은 팁을 준다.
[Add .NET Core DI and Config Goodness to AWSLambda Functions](https://blog.tonysneed.com/2018/12/16/add-net-core-di-and-config-goodness-to-aws-lambda-functions/)

## 5. Unit Test

lambda project나 dotnet core project들을 보면 default로 xUnit 쓰므로 일단 같이 쓴다.

* environmentVariable 문제를 해결해야 할 것 같다..
test project일 때는 mock tool도 안쓰므로 appsetting이 비게 되었다.

[visual studio - Should GetEnvironmentVariable Work in xUnit Test? - Stack Overflow](https://stackoverflow.com/questions/43927955/should-getenvironmentvariable-work-in-xunit-test)
## 6. DB
Ef core를 이용해서 구현한다. 아래와 같이 여러가지 db를 사용하는 버전을 branch로 만들면 어떨까?
<br>-- aws DynamoDB로 일단 CRUD를 만든버전
<br>-- mysql, aws DynamoDB로 CQRS를 만든버전
<br>-- mysql 대신에 postgres를 써볼까?
<br>-- 그 외 database들

## 7. CQRS