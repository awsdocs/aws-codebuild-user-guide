# AWS CodeBuild Concepts<a name="concepts"></a>

The following concepts are important for understanding how CodeBuild works\.

**Topics**
+ [How CodeBuild Works](#concepts-how-it-works)
+ [Next Steps](#concepts-next-steps)

## How CodeBuild Works<a name="concepts-how-it-works"></a>

The following diagram shows what happens when you run a build with CodeBuild: 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/arch.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

1. As input, you must provide CodeBuild with a build project\. A *build project* defines how CodeBuild runs a build\. It includes information such as where to get the source code, the build environment to use, the build commands to run, and where to store the build output\. A *build environment* represents a combination of operating system, programming language runtime, and tools that CodeBuild uses to run a build\. For more information, see:
   + [Create a Build Project](create-project.md)
   + [Build Environment Reference](build-env-ref.md)

1. CodeBuild uses the build project to create the build environment\.

1. CodeBuild downloads the source code into the build environment and then uses the build specification \(build spec\), as defined in the build project or included directly in the source code\. A *build spec* is a collection of build commands and related settings, in YAML format, that CodeBuild uses to run a build\. For more information, see the [Build Spec Reference](build-spec-ref.md)\.

1. If there is any build output, the build environment uploads its output to an Amazon S3 bucket\. The build environment can also perform tasks that you specify in the build spec \(for example, sending build notifications to an Amazon SNS topic\)\. For an example, see [Build Notifications Sample](sample-build-notifications.md)\.

1. While the build is running, the build environment sends information to CodeBuild and Amazon CloudWatch Logs\.

1. While the build is running, you can use the CodeBuild console, AWS CLI, or AWS SDKs, to get summarized build information from CodeBuild and detailed build information from Amazon CloudWatch Logs\. If you use AWS CodePipeline to run builds, you can get limited build information from CodePipeline\.

## Next Steps<a name="concepts-next-steps"></a>

Now that you know more about AWS CodeBuild, we recommend that you complete the following steps:

1. **Experiment** with CodeBuild in an example scenario by following the instructions in [Getting Started](getting-started.md)\.

1. **Use** CodeBuild in your own scenarios by following the instructions in [Plan a Build](planning.md)\.