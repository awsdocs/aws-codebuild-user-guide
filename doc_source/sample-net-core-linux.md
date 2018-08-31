# \.NET Core in Linux Sample for AWS CodeBuild<a name="sample-net-core-linux"></a>

This sample uses a AWS CodeBuild build environment running \.NET Core to build an executable file out of code written in C\#\. 

**Important**  
Running this sample may result in charges to your AWS account\. These include possible charges for AWS CodeBuild and for AWS resources and actions related to Amazon S3, AWS KMS, and CloudWatch Logs\. For more information, see [AWS CodeBuild Pricing](http://aws.amazon.com/codebuild/pricing), [Amazon S3 Pricing](http://aws.amazon.com/s3/pricing), [AWS Key Management Service Pricing](http://aws.amazon.com/kms/pricing), and [Amazon CloudWatch Pricing](http://aws.amazon.com/cloudwatch/pricing)\.

## Running the Sample<a name="sample-net-core-linux-running"></a>

To run this sample:

1. Create the files as described in the Directory Structure and Files sections of this topic, and then upload them to an Amazon S3 input bucket or an AWS CodeCommit, GitHub, or Bitbucket repository\. 
**Important**  
Do not upload `(root directory name)`, just the files inside of `(root directory name)`\.   
If you are using an Amazon S3 input bucket, be sure to create a ZIP file that contains the files, and then upload it to the input bucket\. Do not add `(root directory name)` to the ZIP file, just the files inside of `(root directory name)`\.

1. Create a build project, run the build, and view related build information by following the steps in [Run AWS CodeBuild Directly](how-to-run.md)\.

   If you use the AWS CLI to create the build project, the JSON\-formatted input to the`create-project` command might look similar to this\. \(Replace the placeholders with your own values\.\)

   ```
   {
     "name": "sample-dot-net-core-project",
     "source": {
       "type": "S3",
       "location": "codebuild-region-ID-account-ID-input-bucket/windows-dotnetcore.zip"
     },
     "artifacts": {
       "type": "S3",
       "location": "codebuild-region-ID-account-ID-output-bucket",
       "packaging": "ZIP",
       "name": "dot-net-core-output-artifact.zip"
     },
     "environment": {
       "type": "LINUX_CONTAINER",
       "image": "aws/codebuild/dot-net:core-2.0",
       "computeType": "BUILD_GENERAL1_SMALL"
     },
     "serviceRole": "arn:aws:iam::account-ID:role/role-name",
     "encryptionKey": "arn:aws:kms:region-ID:account-ID:key/key-ID"
   }
   ```

1. To get the build output artifact, in your Amazon S3 output bucket, download the `dot-net-core-output-artifact.zip` file to your local computer or instance\. Extract the contents to get to the executable file `HelloWorldSample.dll`, which be found in the `bin\Debug\netcoreapp2.0` directory\.

## Directory Structure<a name="sample-net-core-linux-dir"></a>

This sample assumes this directory structure\.

```
(root directory name)
    |-- buildspec.yml
    |-- HelloWorldSample.csproj
    `-- Program.cs
```

## Files<a name="sample-net-core-linux-files"></a>

This sample uses these files\.

`buildspec.yml` \(in `(root directory name)`\)

```
version: 0.2

phases:
  build:
    commands:
      - dotnet restore
      - dotnet build
artifacts:
  files:
    - bin/Debug/netcoreapp2.0/*
```

`HelloWorldSample.csproj` \(in `(root directory name)`\)

```
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>netcoreapp2.0</TargetFramework>
  </PropertyGroup>
</Project>
```

`Program.cs` \(in `(root directory name)`\)

```
using System;

namespace HelloWorldSample {
  public static class Program {
    public static void Main() {
      Console.WriteLine("Hello, World!");
    }
  }
}
```

## Related Resources<a name="w4aab9c48c37c13"></a>
+ For more information about getting started with AWS CodeBuild, see [Getting Started with AWS CodeBuild](getting-started.md)\.
+ For more information about troubleshooting problems with AWS CodeBuild, see [Troubleshooting AWS CodeBuild](troubleshooting.md)\.
+ For more information about limits in AWS CodeBuild, see [Limits for AWS CodeBuild](limits.md)\.