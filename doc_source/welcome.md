--------

A new console design is available for this service\. Although the procedures in this guide were written for the older version of the console, you will find many of the concepts and basic procedures in this guide still apply\.

--------

# What Is AWS CodeBuild?<a name="welcome"></a>

AWS CodeBuild is a fully managed build service in the cloud\. AWS CodeBuild compiles your source code, runs unit tests, and produces artifacts that are ready to deploy\. AWS CodeBuild eliminates the need to provision, manage, and scale your own build servers\. It provides prepackaged build environments for the most popular programming languages and build tools such as Apache Maven, Gradle, and more\. You can also customize build environments in AWS CodeBuild to use your own build tools\. AWS CodeBuild scales automatically to meet peak build requests\.

AWS CodeBuild provides these benefits:
+ **Fully managed** – AWS CodeBuild eliminates the need to set up, patch, update, and manage your own build servers\.
+ **On demand** – AWS CodeBuild scales on demand to meet your build needs\. You pay only for the number of build minutes you consume\.
+ **Out of the box** – AWS CodeBuild provides preconfigured build environments for the most popular programming languages\. All you need to do is point to your build script to start your first build\.

For more information, see [AWS CodeBuild](https://aws.amazon.com/codebuild/)\.

**Topics**
+ [How to Run AWS CodeBuild](#welcome-quick-look)
+ [Pricing for AWS CodeBuild](#welcome-pricing)
+ [How Do I Get Started with AWS CodeBuild?](#welcome-getting-started)
+ [AWS CodeBuild Concepts](concepts.md)

## How to Run AWS CodeBuild<a name="welcome-quick-look"></a>

You can run AWS CodeBuild by using the AWS CodeBuild or AWS CodePipeline console\. You can also automate the running of AWS CodeBuild by using the AWS Command Line Interface \(AWS CLI\) or the AWS SDKs\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/overview.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

To run AWS CodeBuild by using the AWS CodeBuild console, AWS CLI, AWS SDKs, see [Run AWS CodeBuild Directly](how-to-run.md)\.

As the following diagram shows, you can add AWS CodeBuild as a build or test action to the build or test stage of a pipeline in AWS CodePipeline\. AWS CodePipeline is a continuous delivery service that enables you to model, visualize, and automate the steps required to release your code\. This includes building your code\. A *pipeline* is a workflow construct that describes how code changes go through a release process\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/pipeline.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

To use AWS CodePipeline to create a pipeline and then add an AWS CodeBuild build or test action, see [Use AWS CodePipeline with AWS CodeBuild](how-to-create-pipeline.md)\. For more information about AWS CodePipeline, see the [AWS CodePipeline User Guide](https://docs.aws.amazon.com/codepipeline/latest/userguide/)\.

## Pricing for AWS CodeBuild<a name="welcome-pricing"></a>

For information, see [AWS CodeBuild Pricing](http://aws.amazon.com/codebuild/pricing)\.

## How Do I Get Started with AWS CodeBuild?<a name="welcome-getting-started"></a>

We recommend that you complete the following steps:

1. **Learn** more about AWS CodeBuild by reading the information in [Concepts](concepts.md)\.

1. **Experiment** with AWS CodeBuild in an example scenario by following the instructions in [Getting Started](getting-started.md)\.

1. **Use** AWS CodeBuild in your own scenarios by following the instructions in [Plan a Build](planning.md)\.