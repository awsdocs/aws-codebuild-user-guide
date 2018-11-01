--------

 The procedures in this guide support the new console design\. If you choose to use the older version of the console, you will find many of the concepts and basic procedures in this guide still apply\. To access help in the new console, choose the information icon\.

--------

# AWS Lambda Sample for AWS CodeBuild<a name="sample-lambda"></a>

To define a standard model for serverless applications that use resources such as Lambda, AWS created the AWS Serverless Application Model \(AWS SAM\)\. For more information, see the [AWS Serverless Application Model](https://github.com/awslabs/serverless-application-model) repository on GitHub\.

You can use AWS CodeBuild to package and deploy serverless applications that follow the AWS SAM standard\. For the deployment step, AWS CodeBuild can use AWS CloudFormation\. To automate the building and deployment of serverless applications with AWS CodeBuild and AWS CloudFormation, you can use AWS CodePipeline\.

For more information, see [Deploying Lambda\-based Applications](https://docs.aws.amazon.com/lambda/latest/dg/deploying-lambda-apps.html) in the *AWS Lambda Developer Guide*\. To experiment with a serverless application sample that uses AWS CodeBuild along with Lambda, AWS CloudFormation, and AWS CodePipeline, see [Automating Deployment of Lambda\-based Applications](https://docs.aws.amazon.com/lambda/latest/dg/automating-deployment.html) in the *AWS Lambda Developer Guide*\.

## Related Resources<a name="w4aac11c45c41b9"></a>
+ For more information about getting started with AWS CodeBuild, see [Getting Started with AWS CodeBuild](getting-started.md)\.
+ For more information about troubleshooting problems with AWS CodeBuild, see [Troubleshooting AWS CodeBuild](troubleshooting.md)\.
+ For more information about limits in AWS CodeBuild, see [Limits for AWS CodeBuild](limits.md)\.