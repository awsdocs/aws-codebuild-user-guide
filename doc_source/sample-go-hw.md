# Go Hello World Sample for AWS CodeBuild<a name="sample-go-hw"></a>

This Go sample produces as build output a single binary file named `hello`\.

**Important**  
Running this sample may result in charges to your AWS account\. These include possible charges for AWS CodeBuild and for AWS resources and actions related to Amazon S3, AWS KMS, and CloudWatch Logs\. For more information, see [AWS CodeBuild Pricing](http://aws.amazon.com/codebuild/pricing), [Amazon S3 Pricing](http://aws.amazon.com/s3/pricing), [AWS Key Management Service Pricing](http://aws.amazon.com/kms/pricing), and [Amazon CloudWatch Pricing](http://aws.amazon.com/cloudwatch/pricing)\.

**Note**  
 Did you know you can use AWS Cloud9 to work with the code in this topic? AWS Cloud9 is an online, cloud\-based integrated development environment \(IDE\) you can use to write, run, debug, and deploy code—using just a browser from an internet\-connected machine\. AWS Cloud9 includes a code editor, debugger, terminal, and essential tools, such as the AWS CLI and Git\. In many cases, you don't need to install files or configure your development machine to start working with code\. Learn more in the [AWS Cloud9 User Guide](http://docs.aws.amazon.com/cloud9/latest/user-guide/)\.

**Topics**
+ [Running the Sample](#sample-go-hw-running)
+ [Directory Structure](#sample-go-hw-dir)
+ [Files](#sample-go-hw-files)
+ [Related Resources](#w3ab1b9c50c27c17)

## Running the Sample<a name="sample-go-hw-running"></a>

To run this sample:

1. Create the files as described in the Directory Structure and Files sections of this topic, and then upload them to an Amazon S3 input bucket or an AWS CodeCommit, GitHub, or Bitbucket repository\. 
**Important**  
Do not upload `(root directory name)`, just the files inside of `(root directory name)`\.   
If you are using an Amazon S3 input bucket, be sure to create a ZIP file that contains the files, and then upload it to the input bucket\. Do not add `(root directory name)` to the ZIP file, just the files inside of `(root directory name)`\.

1. Create a build project, run the build, and view related build information by following the steps in [Run AWS CodeBuild Directly](how-to-run.md)\.

   If you use the AWS CLI to create the build project, the JSON\-formatted input to the`create-project` command might look similar to this\. \(Replace the placeholders with your own values\.\)

   ```
   {
     "name": "sample-go-project",
     "source": {
       "type": "S3",
       "location": "codebuild-region-ID-account-ID-input-bucket/GoSample.zip"
     },
     "artifacts": {
       "type": "S3",
       "location": "codebuild-region-ID-account-ID-output-bucket",
       "packaging": "ZIP",
       "name": "GoOutputArtifact.zip"
     },
     "environment": {
       "type": "LINUX_CONTAINER",
       "image": "aws/codebuild/golang:1.7.3",
       "computeType": "BUILD_GENERAL1_SMALL"
     },
     "serviceRole": "arn:aws:iam::account-ID:role/role-name",
     "encryptionKey": "arn:aws:kms:region-ID:account-ID:key/key-ID"
   }
   ```

1. To get the build output artifact, open your Amazon S3 output bucket\.

1. Download the `GoOutputArtifact.zip` file to your local computer or instance, and then extract the contents of the file\. In the extracted contents, get the `hello` file\. 

## Directory Structure<a name="sample-go-hw-dir"></a>

This sample assumes this directory structure\.

```
(root directory name)
    |-- buildspec.yml
    `-- hello.go
```

## Files<a name="sample-go-hw-files"></a>

This sample uses these files\.

`buildspec.yml` \(in `(root directory name)`\)

```
version: 0.2

phases:
  build:
    commands:
      - echo Build started on `date`
      - echo Compiling the Go code...
      - go build hello.go 
  post_build:
    commands:
      - echo Build completed on `date`
artifacts:
  files:
    - hello
```

`hello.go` \(in `(root directory name)`\)

```
package main
import "fmt"

func main() {
  fmt.Println("hello world")
  fmt.Println("1+1 =", 1+1)
  fmt.Println("7.0/3.0 =", 7.0/3.0)
  fmt.Println(true && false)
  fmt.Println(true || false)
  fmt.Println(!true)
}
```

## Related Resources<a name="w3ab1b9c50c27c17"></a>
+ For more information about getting started with AWS CodeBuild, see [Getting Started with AWS CodeBuild](getting-started.md)\.
+ For more information about troubleshooting problems with AWS CodeBuild, see [Troubleshooting AWS CodeBuild](troubleshooting.md)\.
+ For more information about limits in AWS CodeBuild, see [Limits for AWS CodeBuild](limits.md)\.