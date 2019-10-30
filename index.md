# Ef Core, Lambda

## 잠깐! 배경 이론 훑기

[dependency injection 설명, 샘플]({{ 'dependency-injection' | relative_url }})

## Tutorial 참조자료

1. [Add .NET Core DI and Config Goodness to AWSLambda Functions](https://blog.tonysneed.com/2018/12/16/add-net-core-di-and-config-goodness-to-aws-lambda-functions/)
2. [IDesignTimeDbContextFactory and Dependency Injection: A Love Story](https://blog.tonysneed.com/2018/12/20/idesigntimedbcontextfactory-and-dependency-injection-a-love-story/)
3. [Use EF Core with AWS Lambda Functions](https://blog.tonysneed.com/2018/12/21/use-ef-core-with-aws-lambda-functions/)

### 참조자료의 목적

1. aws를 사용하며 일반적 접근법이 가능한 Lambda application을 쓴다

2. 그러나 상황에 따라서 Serverless application처럼 풍부한 dependency injection system을 쓸 수 있으면 좋겠다

3. 그러기 위해선 필요한 service의 dependency injection을 직접 구현할 수 있어야겠다.

** 스터디에서는 aws lambda를 동작시키는 것과, .net core의 핵심 중 하나인 dependency injection을 배운다.