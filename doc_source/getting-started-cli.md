# Getting started with AWS CodeBuild using the AWS CLI<a name="getting-started-cli"></a>

In this tutorial, you use AWS CodeBuild to build a collection of sample source code input files \(called *build input artifacts* or *build input*\) into a deployable version of the source code \(called *build output artifact* or *build output*\)\. Specifically, you instruct CodeBuild to use Apache Maven, a common build tool, to build a set of Java class files into a Java Archive \(JAR\) file\. You do not need to be familiar with Apache Maven or Java to complete this tutorial\.

You can work with CodeBuild through the CodeBuild console, AWS CodePipeline, the AWS CLI, or the AWS SDKs\. This tutorial demonstrates how to use CodeBuild with the AWS CLI\. For information about using CodePipeline, see [Use CodePipeline with CodeBuild](how-to-create-pipeline.md)\. For information about using the AWS SDKs, see [Run CodeBuild directly](how-to-run.md)\. 

**Important**  
The steps in this tutorial require you to create resources \(for example, an S3 bucket\) that might result in charges to your AWS account\. These include possible charges for CodeBuild and for AWS resources and actions related to Amazon S3, AWS KMS, and CloudWatch Logs\. For more information, see [CodeBuild pricing](http://aws.amazon.com/codebuild/pricing), [Amazon S3 pricing](http://aws.amazon.com/s3/pricing), [AWS Key Management Service pricing](http://aws.amazon.com/kms/pricing), and [Amazon CloudWatch pricing](http://aws.amazon.com/cloudwatch/pricing)\.

## Steps<a name="getting-started-cli-steps"></a>
+ [Step 1: Create two S3 buckets](getting-started-cli-input-bucket.md)
+ [Step 2: Create the source code](getting-started-cli-create-source-code.md)
+ [Step 3: Create the buildspec file](getting-started-cli-create-build-spec.md)
+ [Step 4: Upload the source code and the buildspec file](getting-started-cli-upload-source-code.md)
+ [Step 5: Create the build project](getting-started-cli-create-build-project.md)
+ [Step 6: Run the build](getting-started-cli-run-build.md)
+ [Step 7: View summarized build information](getting-started-cli-monitor-build.md)
+ [Step 8: View detailed build information](getting-started-cli-build-log.md)
+ [Step 9: Get the build output artifact](getting-started-cli-output.md)
+ [Step 10: Delete the S3 input bucket](getting-started-cli-clean-up.md)
+ [Wrapping up](getting-started-cli-next-steps.md)