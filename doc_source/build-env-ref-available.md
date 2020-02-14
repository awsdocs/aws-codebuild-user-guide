# Docker Images Provided by CodeBuild<a name="build-env-ref-available"></a>

AWS CodeBuild manages the following Docker images that are available in the CodeBuild and AWS CodePipeline consoles\.


****  

| Platform | Image identifier | Definition | 
| --- | --- | --- | 
| Amazon Linux 2 | aws/codebuild/amazonlinux2\-x86\_64\-standard:2\.0 | [al2/standard/2\.0](https://github.com/aws/aws-codebuild-docker-images/tree/master/al2/x86_64/standard/2.0) | 
| Amazon Linux 2 | aws/codebuild/amazonlinux2\-aarch64\-standard:1\.0 | [al2/aarch64/standard/1\.0](https://github.com/aws/aws-codebuild-docker-images/tree/master/al2/aarch64/standard/1.0) | 
| Ubuntu 18\.04 | aws/codebuild/standard:2\.0 | [ubuntu/standard/2\.0](https://github.com/aws/aws-codebuild-docker-images/tree/master/ubuntu/standard/2.0) | 
| Ubuntu 18\.04 | aws/codebuild/standard:3\.0 | [ubuntu/standard/3\.0](https://github.com/aws/aws-codebuild-docker-images/tree/master/ubuntu/standard/3.0) | 
| Windows Server Core 2016 | aws/codebuild/windows\-base:2\.0 | N/A | 

 The latest version of each image is cached\. If you specify a more specific version, then CodeBuild provisions that version instead of the cached version\. This can result in longer build times\. For example, to benefit from caching, specify `aws/codebuild/amazonlinux2-x86_64-standard:2.0` instead of a more granular version, such as `aws/codebuild/amazonlinux2-x86_64-standard:2.0-1.0.0`\. 

 The standard image of the Ubuntu 18\.04 and Amazon Linux 2 platforms contain the following runtimes\. If you use the Amazon Linux 2 standard image 1\.0 or later or the Ubuntu standard image 2\.0 or later, you must specify your runtime in the `runtime-versions` section of your buildspec file\. For more information, see [Specify Runtime Versions in the Buildspec File](build-spec-ref.md#runtime-versions-buildspec-file)\. 


| Runtime name | Version/versions | How to specify in the buildspec file | 
| --- | --- | --- | 
| android | 28, 29 | android: 28, android: 29 | 
| docker | 18 | docker: 18 | 
| dotnet | 2\.2 | dotnet: 2\.2 | 
| dotnet \(Amazon Linux 2:2\.0 and Ubuntu Standard:3\.0 only\) | 3\.0 | dotnet: 3\.0 | 
| dotnet | 3\.1 | dotnet: 3\.1 | 
| golang | 1\.12, 1\.13 | golang: 1\.12, golang: 1\.13 | 
| nodejs | 8, 10 | nodejs: 8, nodejs: 10 | 
| nodejs \(Amazon Linux 2:2\.0 and Ubuntu Standard:3\.0 only\) | 10\.18, 12\.14 | nodejs: 10, nodejs: 12 | 
| java \(Ubuntu only\) | openjdk8, openjdk11 | java: openjdk8, java: openjdk11 | 
| java \(Amazon Linux 2 only\) | corretto8, corretto11 | java: corretto8, java: corretto11 | 
| php | 7\.3 | php: 7\.3 | 
| php | 7\.4 | php: 7\.4 | 
| python | 3\.7 | python: 3\.7 | 
| python \(Amazon Linux 2:2\.0 and Ubuntu Standard:3\.0 only\) | 3\.8 | python: 3\.8 | 
| ruby | 2\.6 | ruby: 2\.6 | 

**Note**  
The `aws/codebuild/amazonlinux2-aarch64-standard:1.0` image does not support the Android Runtime \(ART\)\.

 The base image of the Windows Server Core 2016 contains the following runtimes\. 


| Runtime name | Version in `windows-base:2.0` | 
| --- | --- | 
| dotnet | 2\.2, 3\.1 | 
| golang | 1\.13 | 
| nodejs | 10\.18, 12\.14 | 
| java | openjdk11 | 
| php | 7\.3, 7\.4 | 
| python | 3\.7 | 
| ruby | 2\.6 | 

**Note**  
 The base image of the Windows Server Core 2016 platform is available in the US East \(N\. Virginia\), US East \(Ohio\), US West \(Oregon\), and Europe \(Ireland\) regions only\. 

You can use a build specification to install other components \(for example, the AWS CLI, Apache Maven, Apache Ant, Mocha, RSpec, or similar\) during the `install` build phase\. For more information, see [Buildspec Example](build-spec-ref.md#build-spec-ref-example)\.

CodeBuild frequently updates the list of Docker images\. To get the most current list, do one of the following:
+ In the CodeBuild console, in the **Create build project** wizard or **Edit Build Project** page, for **Environment image**, choose **Managed image**\. Choose from the **Operating system**, **Runtime**, and **Runtime version** drop\-down lists\. For more information, see [Create a Build Project \(Console\)](create-project.md#create-project-console) or [Change a Build Project's Settings \(Console\)](change-project.md#change-project-console)\.
+ For the AWS CLI, run the `list-curated-environment-images` command:

  ```
  aws codebuild list-curated-environment-images
  ```
+ For the AWS SDKs, call the `ListCuratedEnvironmentImages` operation for your target programming language\. For more information, see the [AWS SDKs and Tools Reference](sdk-ref.md)\.