---
layout: default
title: "0. lambda & dotnet core"
parent: "lambda & dotnet core"
grand_parent: "aws (serverless)"
nav_order: 0
---


아래 링크에 따르면 .net core 3.0이 "Current" version이기 때문에 lambda 정책에 따라 natively 지원하지 않는다. 대신에 Amazon.Lambda.RuntimeSupport nuget package를 운영한다고 밝혔다.

***This release of .NET Core is called a “Current” release by the .NET team which means it will have a short lifecycle of support after the next release of .NET Core. The Lambda team’s policy is to support Long Term Support (LTS) versions of a runtime so .NET Core 3.0 will not be natively supported on AWS Lambda.***

[aws link - .net core 3.0 and lambda](https://aws.amazon.com/blogs/developer/net-core-3-0-on-lambda-with-aws-lambdas-custom-runtime/)

nuget package에 대한 설명
[aws link - Amazon.Lambda.RuntimeSupport](https://aws.amazon.com/blogs/developer/announcing-amazon-lambda-runtimesupport/)



