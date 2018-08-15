# C\+\+ Hello World Sample for AWS CodeBuild<a name="sample-c-plus-plus-hw"></a>

This C\+\+ sample produces as build output a single binary file named `hello.out`\.

**Important**  
Running this sample may result in charges to your AWS account\. These include possible charges for AWS CodeBuild and for AWS resources and actions related to Amazon S3, AWS KMS, and CloudWatch Logs\. For more information, see [AWS CodeBuild Pricing](http://aws.amazon.com/codebuild/pricing), [Amazon S3 Pricing](http://aws.amazon.com/s3/pricing), [AWS Key Management Service Pricing](http://aws.amazon.com/kms/pricing), and [Amazon CloudWatch Pricing](http://aws.amazon.com/cloudwatch/pricing)\.

**Topics**
+ [Running the Sample](#sample-c-plus-plus-hw-running)
+ [Directory Structure](#sample-c-plus-plus-hw-dir)
+ [Files](#sample-c-plus-plus-hw-files)
+ [Related Resources](#w4ab1b9c52c11c15)

## Running the Sample<a name="sample-c-plus-plus-hw-running"></a>

To run this sample:

1. Create the files as described in the Directory Structure and Files sections of this topic, and then upload them to an Amazon S3 input bucket or an AWS CodeCommit, GitHub, or Bitbucket repository\. 
**Important**  
Do not upload `(root directory name)`, just the files inside of `(root directory name)`\.   
If you are using an Amazon S3 input bucket, be sure to create a ZIP file that contains the files, and then upload it to the input bucket\. Do not add `(root directory name)` to the ZIP file, just the files inside of `(root directory name)`\.

1. Create a build project, run the build, and view related build information by following the steps in [Run AWS CodeBuild Directly](how-to-run.md)\.

   If you use the AWS CLI to create the build project, the JSON\-formatted input to the`create-project` command might look similar to this\. \(Replace the placeholders with your own values\.\)

   ```
   {
     "name": "sample-c-plus-plus-project",
     "source": {
       "type": "S3",
       "location": "codebuild-region-ID-account-ID-input-bucket/CPlusPlusSample.zip"
     },
     "artifacts": {
       "type": "S3",
       "location": "codebuild-region-ID-account-ID-output-bucket",
       "packaging": "ZIP",
       "name": "CPlusPlusOutputArtifact.zip"
     },
     "environment": {
       "type": "LINUX_CONTAINER",
       "image": "aws/codebuild/ubuntu-base:14.04",
       "computeType": "BUILD_GENERAL1_SMALL"
     },
     "serviceRole": "arn:aws:iam::account-ID:role/role-name",
     "encryptionKey": "arn:aws:kms:region-ID:account-ID:key/key-ID"
   }
   ```

1. To get the build output artifact, open your Amazon S3 output bucket\.

1. Download the `CPlusPlusOutputArtifact.zip` file to your local computer or instance, and then extract the contents of the file\. In the extracted contents, get the `hello.out` file\. 

## Directory Structure<a name="sample-c-plus-plus-hw-dir"></a>

This sample assumes this directory structure\.

```
(root directory name)
    |-- buildspec.yml
    `-- hello.cpp
```

## Files<a name="sample-c-plus-plus-hw-files"></a>

This sample uses these files\.

`buildspec.yml` \(in `(root directory name)`\)

```
version: 0.2

phases:
  install:
    commands:
      - apt-get update -y
      - apt-get install -y build-essential
  build:
    commands:
      - echo Build started on `date`
      - echo Compiling the C++ code...
      - g++ hello.cpp -o hello.out
  post_build:
    commands:
      - echo Build completed on `date`
artifacts:
  files:
    - hello.out
```

`hello.cpp` \(in `(root directory name)`\)

```
#include <iostream>

int main()
{
  std::cout << "Hello, World!\n";
}
```

## Related Resources<a name="w4ab1b9c52c11c15"></a>
+ For more information about getting started with AWS CodeBuild, see [Getting Started with AWS CodeBuild](getting-started.md)\.
+ For more information about troubleshooting problems with AWS CodeBuild, see [Troubleshooting AWS CodeBuild](troubleshooting.md)\.
+ For more information about limits in AWS CodeBuild, see [Limits for AWS CodeBuild](limits.md)\.