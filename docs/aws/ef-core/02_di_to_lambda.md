---
layout: default
title: "1. DI setting"
parent: "aws lambda & ef-core"
grand_parent: "AWS"
nav_order: 2
has_children: true
---

### tutorial : .net core의 dependency injection을 lambda에 적용해본다.

[Add .NET Core DI and Config Goodness to AWSLambda Functions](https://blog.tonysneed.com/2018/12/16/add-net-core-di-and-config-goodness-to-aws-lambda-functions/)

tutorial을 똑같이 따라가본다.

## 요약

### 작업링크


### 생성 세팅
* aws lambda project with test (visual studio)

### 새 파일
- ConfigurationService.cs
- IConfigurationService.cs
- EnvironmentService.cs
- IEnvironmentService.cs
- appsettings.json
### 수정
- Function.cs
- aws-lambda-tools-defaults.json
### Dependency
- Microsoft.Extensions.Configuration
- Microsoft.Extensions.Configuration.EnvironmentVariables
- Microsoft.Extensions.Configuration.FileExtensions
- Microsoft.Extensions.Configuration.Json
- Microsoft.Extensions.DependencyInejction
