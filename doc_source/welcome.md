# What is AWS CodeBuild?<a name="welcome"></a>

AWS CodeBuild is a fully managed build service in the cloud\. CodeBuild compiles your source code, runs unit tests, and produces artifacts that are ready to deploy\. CodeBuild eliminates the need to provision, manage, and scale your own build servers\. It provides prepackaged build environments for popular programming languages and build tools such as Apache Maven, Gradle, and more\. You can also customize build environments in CodeBuild to use your own build tools\. CodeBuild scales automatically to meet peak build requests\.

CodeBuild provides these benefits:
+ **Fully managed** – CodeBuild eliminates the need to set up, patch, update, and manage your own build servers\.
+ **On demand** – CodeBuild scales on demand to meet your build needs\. You pay only for the number of build minutes you consume\.
+ **Out of the box** – CodeBuild provides preconfigured build environments for the most popular programming languages\. All you need to do is point to your build script to start your first build\.

For more information, see [AWS CodeBuild](https://aws.amazon.com/codebuild/)\.

**Topics**
+ [How to run CodeBuild](#welcome-quick-look)
+ [Pricing for CodeBuild](#welcome-pricing)
+ [How do I get started with CodeBuild?](#welcome-getting-started)
+ [AWS CodeBuild concepts](concepts.md)

## How to run CodeBuild<a name="welcome-quick-look"></a>

You can use the AWS CodeBuild or AWS CodePipeline console to run CodeBuild\. You can also automate the running of CodeBuild by using the AWS Command Line Interface \(AWS CLI\) or the AWS SDKs\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/overview.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

To run CodeBuild by using the CodeBuild console, AWS CLI, or AWS SDKs, see [Run AWS CodeBuild directly](how-to-run.md)\.

As the following diagram shows, you can add CodeBuild as a build or test action to the build or test stage of a pipeline in AWS CodePipeline\. AWS CodePipeline is a continuous delivery service that you can use to model, visualize, and automate the steps required to release your code\. This includes building your code\. A *pipeline* is a workflow construct that describes how code changes go through a release process\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/pipeline.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

To use CodePipeline to create a pipeline and then add a CodeBuild build or test action, see [Use CodePipeline with CodeBuild](how-to-create-pipeline.md)\. For more information about CodePipeline, see the [AWS CodePipeline User Guide](https://docs.aws.amazon.com/codepipeline/latest/userguide/)\.

The CodeBuild console also provides a way to quickly search for your resources, such as repositories, build projects, deployment applications, and pipelines\. Choose **Go to resource** or press the `/` key, and then enter the name of the resource\. Any matches appear in the list\. Searches are case insensitive\. You only see resources that you have permissions to view\. For more information, see [Viewing resources in the console](console-resources.md)\. 

## Pricing for CodeBuild<a name="welcome-pricing"></a>

For information, see [CodeBuild pricing](https://aws.amazon.com/codebuild/pricing)\.

## How do I get started with CodeBuild?<a name="welcome-getting-started"></a>

We recommend that you complete the following steps:

1. **Learn** more about CodeBuild by reading the information in [Concepts](concepts.md)\.

1. **Experiment** with CodeBuild in an example scenario by following the instructions in [Getting started using the console](getting-started.md)\.

1. **Use** CodeBuild in your own scenarios by following the instructions in [Plan a build](planning.md)\.