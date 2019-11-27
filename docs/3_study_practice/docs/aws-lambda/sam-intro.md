---
layout: default
title: "• SAM 소개"
grand_parent: "study(실용)"
parent: "aws lambda"
nav_order: 1
has_children: true
---

### **문서 목적**

* aws 기반 msa를 설계하는데 핵심 중 하나인 SAM에 대해 소개한다.
* lambda 프로그래밍과 관련되어 중요한 것을 요약한다.

SAM : Serverless Application Model

### **정의**
A serverless application is a combination of Lambda functions, event sources, and other resources that work together to perform tasks. Note that a serverless application is more than just a Lambda function—it can include additional resources such as APIs, databases, and event source mappings.
[What Is the AWS Serverless Application Model (AWS SAM)? - AWS Serverless Application Model](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/what-is-sam.html)
<br><br>

### **서비스의 시작**

2016년 11월
[Introducing the AWS Serverless Application Model](https://aws.amazon.com/about-aws/whats-new/2016/11/introducing-the-aws-serverless-application-model/)

<br>beanstalk과 같이 한개의 서버를 통째로 올리는 경우 이러한 정의가 굳이 필요하지 않았지만, 
<br>lambda 서비스 이후부터는 serverless라는 표현을 쓰기 시작했다. 
<br>대부분의 infra는 아마존이 알아서 처리하거나, 서비스로 노출시키고
<br>단순히 '단 하나의 기능'을 의미하고 있는 lambda에만 집중할 수 있도록 하는 '구조'가 있을 것이다.
<br><br>SAM은 그 하나의 덩어리를 정의하도록 해주는 template을 제공하고, 그것을 관리해 배포할 수 있는 도구를 제공한다.
<br><br>

### **SAM의 장점**
* Single-deployment configuration. (한번에 배포하는 단위)
* Extension of AWS CloudFormation. (CloudFormation: Infrastructure as a code) 
* Built-in best practices. (best practice를 따를 수 밖에 없게된다)
* Local debugging and testing. (SAM Cli가 local debugging을 돕는다)
* Deep integration with development tools (부가적인 tool이 있다. Built-in best practice와 비슷한 의미인듯)
<br><br>

### **장점 중 한번 더 : SAM은 Extension of aws CloudFormation**

[AWS CloudFormation – Model and provision all your cloud infrastructure resources](https://aws.amazon.com/cloudformation/)
* CloudFormation 정의 - infrastructure as a code
AWS CloudFormation provides a common language for you to model and provision AWS and third party application  resources in your cloud environment. AWS CloudFormation allows you to use programming languages or a simple text file to model and provision, in an automated and secure manner, all the resources needed for your applications across all regions and accounts. This gives you a single source of truth for your AWS and third party resources.
<br><br>

### **SAM Temaplate**

SAM Template이 가장 개발자에게 익숙하게 쓰일 것이며,
<br>'application modeling'을 하는 config 문서라고 생각할 수 있다.
<br>

* template file
The AWS SAM template file is a YAML or JSON configuration file that adheres to the open source AWS Serverless [Application Model specification](https://github.com/awslabs/serverless-application-model/blob/master/versions/2016-10-31.md). You use the template to declare all of the AWS resources that comprise your serverless application.


<br>아래의 포맷이 template의 기본 구성. 
<br>특히 Resources에 api나 lambda가 배치된다.
<br>

```yaml
Transform: AWS::Serverless-2016-10-31

Globals:
  set of globals

Description:
  String

Metadata:
  template metadata

Parameters:
  set of parameters

Mappings:
  set of mappings

Conditions:
  set of conditions

Resources:
  set of resources

Outputs:
  set of outputs
```
<br>

### **Resources의 항목들**
<br>
resources가 비즈니스 개발자들에게는 가장 익숙할 수 밖에 없으므로 아마존에서도 문서 한개 어치로 강조해놓았다. 쓸 수 있는 항목을 나열해 보자면 아래와 같다.
<br>
* **AWS::Serverless::Api**
<br>For most scenarios, we **recommend that you create APIs by specifying this resource type** as an event source of your AWS::Serverless::Function resource.
<br>

* **AWS::Serverless::Application**
<br>This resource type embeds a serverless application from the AWS Serverless Application Repository or from an Amazon S3 bucket as a **[nested application.](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-template-nested-applications.html)**
<br>
nested application들을 참조하는 방법 
<br>-- AWS Serverless Application Repository 주소를 참조 
<br>-- Amazon S3 bucket주소를 참조
<br>

* **AWS::Serverless::Function**
<br>Lambda function configuration
<br>가장 흔하게 쓸 수 밖에 없을 것 같다.
<br>

* **AWS::Serverless::LayerVersion**
<br>Lambda layer version이라고 봐도 되며 api version과 같은 개념으로 생각해도 된다. logical ID를 분리해 AWS CloudFormation이 old version을 덮지 않도록 유도.
<br>

* **AWS::Serverless::SimpleTable**
<br>DynamoDB table을 만든다. (그 특정한 db를 사용할 때이므로 잘 안 쓸듯?)
<br>
* **AWS::S3::Bucket (및 CloudFormation의 것들)**
<br>AWS SAM supports the full suite of resources, intrinsic functions, and other template features that are available in AWS CloudFormation.

### **SAM Template Resource 예시**
<br>
한개의 function을 정의하고 있고, 
한개의 Api event에 의해 call이 일어난다
<br>
```json
"Resources" : {
		"CreateEntity1HttpFunction" : { 
			"Type" : "AWS::Serverless::Function",
			"Properties": {
				"Handler": "MyLambdaDotNetCoreProject.Api::MyLambdaDotNetCoreProject.Api.Entity1Lambda::Create",
				"Runtime": "dotnetcore2.1",
				"MemorySize": 256,
				"Timeout": 30,
				"Role": null,
				"Policies": [ "AWSLambdaBasicExecutionRole" ],
				"Environment" : {
					"Variables" : {
						"environmentName" : { "Ref" : "environmentName" }
					}
				},
				"Events": {
					"RootGet": {
						"Type": "Api",
						"Properties": {
							"Path": "/entity1",
							"Method": "POST"
						}
					}
				}
			}
		},
    }
```

### **결론**

* SAM은 한 덩어리의 infrastructure를 정의
* SAM은 하나의 배포단위
* SAM은 하나의 비즈니스 server 단위 (가 되어야만 좋은 관리를 할 수 있다)
* SAM은 위의 항목들을 개발자끼리 공유할 수 있는 '문서'

