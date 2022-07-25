# Docker images provided by CodeBuild<a name="build-env-ref-available"></a>

AWS CodeBuild manages the following Docker images that are available in the CodeBuild and AWS CodePipeline consoles\.


| Platform | Image identifier | Definition | 
| --- | --- | --- | 
| Amazon Linux 2 | aws/codebuild/amazonlinux2\-x86\_64\-standard:3\.0 | [al2/standard/3\.0](https://github.com/aws/aws-codebuild-docker-images/tree/master/al2/x86_64/standard/3.0) | 
| Amazon Linux 2 | aws/codebuild/amazonlinux2\-x86\_64\-standard:4\.0 | [al2/standard/4\.0](https://github.com/aws/aws-codebuild-docker-images/tree/master/al2/x86_64/standard/4.0) | 
| Amazon Linux 2 | aws/codebuild/amazonlinux2\-aarch64\-standard:1\.0 | [al2/aarch64/standard/1\.0](https://github.com/aws/aws-codebuild-docker-images/tree/master/al2/aarch64/standard/1.0) | 
| Amazon Linux 2 | aws/codebuild/amazonlinux2\-aarch64\-standard:2\.0 | [al2/aarch64/standard/2\.0](https://github.com/aws/aws-codebuild-docker-images/tree/master/al2/aarch64/standard/2.0) | 
| Ubuntu 18\.04 | aws/codebuild/standard:4\.0 | [ubuntu/standard/4\.0](https://github.com/aws/aws-codebuild-docker-images/tree/master/ubuntu/standard/4.0) | 
| Ubuntu 20\.04 | aws/codebuild/standard:5\.0 | [ubuntu/standard/5\.0](https://github.com/aws/aws-codebuild-docker-images/tree/master/ubuntu/standard/5.0) | 
| Ubuntu 22\.04 | aws/codebuild/standard:6\.0 | [ubuntu/standard/6\.0](https://github.com/aws/aws-codebuild-docker-images/tree/master/ubuntu/standard/6.0) | 
| Windows Server Core 2019 | aws/codebuild/windows\-base:2019\-1\.0 | N/A | 
| Windows Server Core 2019 | aws/codebuild/windows\-base:2019\-2\.0 | N/A | 

The base image of the Windows Server Core 2019 platform is only available in the following regions:
+ US East \(N\. Virginia\)
+ US East \(Ohio\)
+ US West \(Oregon\)
+ Europe \(Ireland\)

CodeBuild frequently updates the list of Docker images\. To get the most current list, do one of the following:
+ In the CodeBuild console, in the **Create build project** wizard or **Edit Build Project** page, for **Environment image**, choose **Managed image**\. Choose from the **Operating system**, **Runtime**, and **Runtime version** drop\-down lists\. For more information, see [Create a build project \(console\)](create-project-console.md) or [Change a build project's settings \(console\)](change-project-console.md)\.
+ For the AWS CLI, run the `list-curated-environment-images` command:

  ```
  aws codebuild list-curated-environment-images
  ```
+ For the AWS SDKs, call the `ListCuratedEnvironmentImages` operation for your target programming language\. For more information, see the [AWS SDKs and tools reference](sdk-ref.md)\.

**Topics**
+ [Available runtimes](available-runtimes.md)
+ [Runtime versions](runtime-versions.md)