# AWS CodeBuild Samples<a name="samples"></a>

 These use case\-based samples can be used to experiment with AWS CodeBuild: 


****  

| Name | Description | 
| --- | --- | 
| [Amazon ECR Sample](sample-ecr.md) | Uses a Docker image in an Amazon ECR repository to use Apache Maven to produce a single JAR file\. | 
| [Docker Sample](sample-docker.md) | Uses a build image provided by AWS CodeBuild with Docker support to produce a Docker image with Apache Maven\. Pushes the Docker image to a repository in Amazon ECR\. You can adapt this sample to push the Docker image to Docker Hub\. | 
| [GitHub Enterprise Sample](sample-github-enterprise.md) | Uses AWS CodeBuild with GitHub Enterprise as the source repository, with certificates installed and webhooks enabled, to rebuild the source code every time a code change is pushed to the repository\. | 
| [GitHub Pull Request Sample](sample-github-pull-request.md) | Uses AWS CodeBuild with GitHub as the source repository and webhooks enabled, to rebuild the source code every time a code change is pushed to the repository\. | 
| [Use AWS Config with AWS CodeBuild Sample](how-to-integrate-config.md) | Shows how to set up AWS Config\. Lists which AWS CodeBuild resources are tracked and describes how to look up AWS CodeBuild projects in AWS Config\. | 
| [ Host Build Output in an Amazon S3 Bucket ](sample-disable-artifact-encryption.md) | Shows how to create a static website in an Amazon S3 bucket using unencrypted build artifacts\. | 
| [Build Badges Sample](sample-build-badges.md) | Shows how to set up AWS CodeBuild with build badges\. | 
| [Using Semantic Versioning to Name Build Artifacts Sample](sample-buildspec-artifact-naming.md) | Shows how to use semantic versioning to create an artifact name at build time\. | 
| [Build Notifications Sample](sample-build-notifications.md) | Uses Apache Maven to produce a single JAR file\. Sends a build notification to subscribers of an Amazon SNS topic\. | 
| [Docker in Custom Image Sample](sample-docker-custom-image.md) | Uses a custom Docker image to produce a Docker image\. | 
| [AWS CodeDeploy Sample](sample-codedeploy.md) | Uses Apache Maven to produce a single JAR file\. Uses AWS CodeDeploy to deploy the JAR file to an Amazon Linux instance\. You can also use AWS CodePipeline to build and deploy the sample\. | 
| [AWS Lambda Sample](sample-lambda.md) | Uses AWS CodeBuild, Lambda, AWS CloudFormation, and AWS CodePipeline to build and deploy a serverless application that follows the AWS Serverless Application Model \(AWS SAM\) standard\. | 
| [Elastic Beanstalk Sample](sample-elastic-beanstalk.md) | Uses Apache Maven to produce a single WAR file\. Uses Elastic Beanstalk to deploy the WAR file to an Elastic Beanstalk instance\. | 

You can use these code\-based samples to experiment with AWS CodeBuild:


****  

| Name | Description | 
| --- | --- | 
| [C\+\+ Sample](sample-c-plus-plus-hw.md) | Uses C\+\+ to output a single \.out file\. | 
| [Go Sample](sample-go-hw.md) | Uses Go to output a single binary file\. | 
| [Maven Sample](sample-maven-5m.md) | Uses Apache Maven to produce a single JAR file\. | 
| [Node\.js Sample](sample-nodejs-hw.md) | Uses Mocha to test whether an internal variable in code contains a specific string value\. Produces a single \.js file\. | 
| [Python Sample](sample-python-hw.md) | Uses Python to test whether an internal variable in code is set to a specific string value\. Produces a single \.py file\. | 
| [Ruby Sample](sample-ruby-hw.md) | Uses RSpec to test whether an internal variable in code is set to a specific string value\. Produces a single \.rb file\. | 
| [Scala Sample](sample-scala-hw.md) | Uses sbt to produce a single JAR file\. | 
| [Java Sample](sample-war-hw.md) | Uses Apache Maven to produce a single WAR file\. | 
| [Windows Samples](sample-windows.md) | Uses the Microsoft \.NET Framework or \.NET Core to build an executable file from C\#, F\#, or Visual Basic code\. | 
| [\.NET Core in Linux Sample](sample-net-core-linux.md) | Uses \.NET Core to build an executable file out of code written in C\#\. | 