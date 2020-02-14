# Getting Started with CodeBuild in the Console<a name="getting-started"></a>

In this tutorial, you use AWS CodeBuild to build a collection of sample source code input files \(*build input artifacts* or *build input*\) into a deployable version of the source code \(*build output artifact* or *build output*\)\. Specifically, you instruct CodeBuild to use Apache Maven, a common build tool, to build a set of Java class files into a Java Archive \(JAR\) file\. You do not need to be familiar with Apache Maven or Java to complete this tutorial\.

You can work with CodeBuild through the CodeBuild console, AWS CodePipeline, the AWS CLI, or the AWS SDKs\. This tutorial demonstrates how to use the CodeBuild console\. for information about using CodePipeline, see [Use AWS CodePipeline with CodeBuild](how-to-create-pipeline.md)\. For information about using the AWS SDKs, see [Run AWS CodeBuild Directly](how-to-run.md)\. 

**Important**  
The steps in this tutorial require you to create resources \(for example, an S3 bucket\) that might result in charges to your AWS account\. These include possible charges for CodeBuild and for AWS resources and actions related to Amazon S3, AWS KMS, and CloudWatch Logs\. For more information, see [CodeBuild Pricing](http://aws.amazon.com/codebuild/pricing), [Amazon S3 Pricing](http://aws.amazon.com/s3/pricing), [AWS Key Management Service Pricing](http://aws.amazon.com/kms/pricing), and [Amazon CloudWatch Pricing](http://aws.amazon.com/cloudwatch/pricing)\.

## Steps<a name="getting-started-steps"></a>
+ [Step 1: Create Two S3 Buckets](getting-started-input-bucket-console.md)
+ [Step 2: Create the Source Code](getting-started-create-source-code-console.md)
+ [Step 3: Create the Buildspec File](getting-started-create-build-spec-console.md)
+ [Step 4: Upload the Source Code and the Buildspec File](getting-started-upload-source-code-console.md)
+ [Step 5: Create the Build Project](getting-started-create-build-project-console.md)
+ [Step 6: Run the Build](getting-started-run-build-console.md)
+ [Step 7: View Summarized Build Information](getting-started-monitor-build-console.md)
+ [Step 8: View Detailed Build Information](getting-started-build-log-console.md)
+ [Step 9: Get the Build Output Artifact](getting-started-output-console.md)
+ [Step 10: Clean Up](getting-started-clean-up-console.md)
+ [Wrapping Up](getting-started-next-steps-console.md)