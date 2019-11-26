---
layout: default
title: "1. progress"
parent: "architecture"
grand_parent: "backend (msa)"
nav_order: 1
---

## backend architecture implementation progress
<br>architecture 실제 생성과정 기록.
<br>--> plan에서 예상치 못하게 흘러갔거나 새롭게 나온 주제 위주

## 1. initial project

* template
[AWS Toolkit for Visual Studio - AWS Lambda](https://docs.aws.amazon.com/lambda/latest/dg/lambda-dotnet-create-deployment-package-toolkit.html)
lambda-custom runtime 생성
* dotnet
<br>-- asp dotnet은 사용치 않는다
<br>-- dotnet core 3.0
<br>-- dotnet console program

## 2. benchmarking
[examples/aws-dotnet-rest-api-with-dynamodb at master · serverless/examples · GitHub](https://github.com/serverless/examples/tree/master/aws-dotnet-rest-api-with-dynamodb)

* 참조
<br>손수 service injection을 준비해놓은 부분이 괜찮았다. dotnet core의 dependency injection 구조가 곧 architecture의 뼈대구조이기 때문.

* 문제
<br>runtime을 npm module로 실행하던데 익숙한 방식은 아니었음.

## 2. multiple lambda 세팅

* static Main function
일단 대부분의 lambda 예시와 달리 따로 Program.cs file로 떼어 놓았음.
<br>
* Function

<br>내부의 bootstrap 파일 지움.
<br>LambdaSerializer 세팅. 당연히 json을 사용한다.

* Serverless template

<br>풍부한 application을 위해 plan대로 SAM을 쓴다.
<br>function을 두개 만들어서 정의해놓았음

* AWS .net test tool

<br>launchSettings.Json에 aws .net mock test tool로 execute하도록.

[aws-lambda-dotnet/Tools/LambdaTestTool at master · aws/aws-lambda-dotnet · GitHub](https://github.com/aws/aws-lambda-dotnet/tree/master/Tools/LambdaTestTool#installing-and-running)

<br>그 결과 실행 시 function들을 실행할 수 있음.
<br>**아래 그림에서 보듯, asp hosting을 쓰는게 아니므로 다른 test tool이 필요하고, lambda 침에서 제공하는 mock Lambda Test Tool을 사용 가능하다.(그러나 결국 unit test만으로 충분해보인다.)
<br><br>![c885e79e](https://user-images.githubusercontent.com/55048882/69588126-1464a600-102b-11ea-8d48-6ebb4d0aa8fd.png)


### 2-1 monolithic function, single-purposed function의 비교

차이점 :
<br>-- monolithic: 프로젝트 하나 내에 여러개의 lambda를 넣어 정말로 '기능 단위'를 구현
<br>-- single-purposed: 프로젝트 하나 당 lambda 한개만 넣어 parameter 등등을 갖고 분기 시킨다.
<br>위에서 얘기한 dotnet의 web api template들은 모두 monolithic function 구조를 얘기하고 있다.
<br>그러나 그 구조가 공학적으로나 업무적으로나 꼭 좋다는 보장이 없고, 아래 링크 글의 글쓴이는 오히려 single-purposed가 우월하다는 주장을 하고 있다.

[AWS Lambda — should you have few monolithic functions or many single-purposed functions? - By Yan Cui](https://hackernoon.com/aws-lambda-should-you-have-few-monolithic-functions-or-many-single-purposed-functions-8c3872d4338f)

<br>나 또한 동의하므로 그 체계를 선택했다.

## 3. dotnet 3.0 custom runtime 적용 실패

<br>아래 링크 참조

[AWS Lambda Runtimes - AWS Lambda](https://docs.aws.amazon.com/lambda/latest/dg/lambda-runtimes.html)

<br>lambda runtime은 각 language에 해당하는 환경에서 execution을 담당하는 것. dotnet은 11월말 기준 3.0이 LTS로 올라와 있지 않아서 lambda에서 공식적으로 runtime 제공을 안해주고 있었음. 대신 3.0에 대해 custom runtime기능을 nuget에서 지원해주고있길래 사용해보려고 했으나 이 또한 monolithic function으로 사용하는 방법이다.
이 방법은 아래 이유들 때문에 피하려 한다.

1. monolithic 방식은 lambda와 SAM 구조를 너무 많이 숨긴다.
2. monolithic 방식은 debugging에 특히나 문제를 줄 것이라는 생각이 든다.
3. monolithic 방식을 쓸 때 다른 방식의 event를 받아들이는 것을 같은 프로젝트 안에서 하기가 힘들다. (물론 다른 프로젝트를 열어서 할 수는 있을듯..)

* dotnet core의 release
<br>dotnet 3.0 core를 web api에 dependent한 monolithic 모델을 쓰지 않으면 custom runtime을 자체제작해야 한다는 결론이 나왔다. 그것이 불가능해보이지는 않지만 언제나 시간을 따져야 하므로..
dotnet core 3.1은 그리고 LTS버전이고 12월 배포 예정이니 빠른 시간 안에 aws 팀에서 움직여줄 것이라고 기대할 수 있기는 하다.
<br><br>따라서 dotnet 3.0은 아래 일정을 봐가면서 일단 미룬다. (12월이라니까.)

[Announcing .NET Core 3.1 Preview 2 \| .NET Blog](https://devblogs.microsoft.com/dotnet/announcing-net-core-3-1-preview-2/)

### 3-1. dotnet 2.1 기반의 구조로 세팅
* 앞의 이유로 3.0은 보류
* Resource 한개 단위가 function
<br>serverless.template은 function별로 정의하다보니 resource.method 단위로 정의하게 되었다. 어떤 방식이 적합한지는 계속 생각해봐야겠다.

### 3-2. 중간 점검

* serverless template은 솔루션 당 몇개?
<br>Serverless Application 단위이므로 배포 단위라고도 볼 수 있고 솔루션 당 한개가 자연스러운 생각이겠다.
<br>-- 그러나 뭐 하나 바뀔 때마다 전체 lambda를 배포하는게 맞는 것인가? 어려운 내용이 될 것 같다.
<br>-- local debugging 환경도 굉장히 커질 여지는 있겠다.
* function의 단위는 어떻게? 그리고 resource의 단위는 어떻게?
<br>-- 만약 resource의 단위가 곧 function의 단위가 된다면 어떨까, 문제는 어차피 function의 signature가 apiProxy이기 때문에 다른 형태의 (e.g. SQS) event가 들어와도 그 resource를 바꾼다는 정의는 못하긴 할 것이다.
<br>그래도 그렇게 나누는 것이 좀 더 편하긴 할 것 같다..

## 4. SAM, default 세팅


## 5. environment 세팅

* AspDotnet이었을 때?
<br>AspDotnet의 경우에는 appsetting 분화가 launch settings에서 "ASPNETCORE_ENVIRONMENT"이라는 변수를 이용해 알아서 사용하게한다. 실행할 때 그걸 참조해서 분화시키면 그만이다.

* lambda
<br>deploy를 할때의 변수라고 생각해야 하므로 어떤 형태로든지 publish하는 단계에서 참조가 되도록해줘야한다.
아래 링크를 참조했음 (중간 즈음)
[Add .NET Core DI and Config Goodness to AWS Lambda Functions \| Tony Sneed's Blog](https://blog.tonysneed.com/2018/12/16/add-net-core-di-and-config-goodness-to-aws-lambda-functions/)
-- testing 환경의 경우 launchsetting에서 mock test tool에 대해 "environmentVariables" : {json} 형태로 세팅 가능
-- deployment의 경우 launch-setting-tool-default.json에서 "environment-variables": {json을 serialize한 string} 형태로 가능

* SAM template
<br>문제는 현재 architecture는 serverless template을 사용하므로 그런 세팅을 template 내 resource의 각 lambda에 대해서 다 해줘야한다. 그게 소모적이므로 SAM template parameter를 쓴다.
[Parameters - AWS CloudFormation](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/parameters-section-structure.html)
deploy 상황에서 "template-parameters"에 적용해서 넣어준다.

![1979f573.png](https://user-images.githubusercontent.com/55048882/69588125-1464a600-102b-11ea-911f-f94d2dceb8ac.png)

<br>위처럼 적용해두면 다음과 같은 기능들이 있음
<br><br>-- parameter는 하단의 resources들 세팅할 때 referencing할 수 있음
<br>-- deploy하는 단계에서 template-parameters 옵션에서 쓰이며, "aws-lambda-tools-defaults.json" 안에 미리 정의해놓기 가능
<br>참고로 [이 링크](https://github.com/aws/aws-extensions-for-dotnet-cli/issues/54)에서 설명하다시피 dotnet lambda deploy-serverless 명령어의 모든 option들이 해당 파일 안에서 미리 정의해놓을 수 있다.
<br>-- allowed values를 통해 최소한 이상한 애가 들어갈 확률을 낮출 수 있음.

### 5-1. SSM parameter store (향후)
<br>db connection string이나 그 외 중요한 정보들을 code에 넣어놓는건 조금 무서울 수 있으므로 SSM(system settingup manager)의 parameter store에 넣어놓고 stack에 대해 지정해놓는게 좋긴 하겠다
[AWS Systems Manager Parameter Store - AWS Systems Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-parameter-store.html)

## 6. startup 구축

<br>-- template parameter를 이용, appsetting 파일을 골라 configuration 구축
<br>-- configuration 파일을 이용, dependency injection 체계 구축
<br>-- mediatR 추가

## 7. project들 구축

* domain
* application
* infrastructure
* 간단한 query 기본적으로 만들어봄.
<br>command나 query 둘다 mediator 패턴을 쓰면 어떨까 생각하여 request class를 만들어 추가해봄.

<br>**request class는 다시 지움. eshopOnContainer처럼 query를 그대로 써도 상관은 없을 것 같다.

## 8. test project 구축

아래 참조

[Shared Context between Tests > xUnit.net](https://xunit.net/docs/shared-context.html)
<br><br>-- 각 dependency injection이 잘되는지 테스트 가능
<br>-- environment 체계는 따로 구성해야 했음
<br>-- lambda 단위로 테스트 가능하다는 것이 큰 장점으로 보임
<br>-- automapper도 세팅하였다.

## 9. query 구축

<br>-- 따로 readmodel 정의
<br>-- 따로 IQueries 정의

## 10. autofac?

* autofac이 필요할 것인가?
<br>아래 글에 autofac을 안 쓸 경우 장단점이 나와있음
[.NET Core project without Autofac. Is it viable? \| Alex Klaus](https://alex-klaus.com/webapi-proj-without-autofac/)
<br><br>일단 안쓰는 것으로 결론 내렸다.

## 11. Db

* Mysql
<br>현재 가장 익숙하므로 써보게 된다.


* DynamoDb
먼저 aws의 dynamoDb로 crud를 해보기로했다. document db이기때문.
<br><br>-- 항상 돈이 문제
![f8260ec2.png](https://user-images.githubusercontent.com/55048882/69588128-14fd3c80-102b-11ea-987b-30e295d20814.png)
<br><br>-- java가 (local의 경우) 필요하다는 문제

* Postgres
[Getting Started \| Npgsql Documentation](http://www.npgsql.org/efcore/)

## 12. Ef core