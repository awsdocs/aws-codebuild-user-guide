# Docker Images Provided by CodeBuild<a name="build-env-ref-available"></a>

AWS CodeBuild manages the following Docker images that are available in the CodeBuild and AWS CodePipeline consoles\.


****  

| Platform | Image identifier | Definition | 
| --- | --- | --- | 
| Amazon Linux 2 | aws/codebuild/amazonlinux2\-x86\_64\-standard:1\.0 | [al2/standard/1\.0](https://github.com/aws/aws-codebuild-docker-images/tree/master/al2/x86_64/standard/1.0) | 
| Ubuntu 18\.04 | aws/codebuild/standard:1\.0 | [ubuntu/standard/1\.0](https://github.com/aws/aws-codebuild-docker-images/tree/master/ubuntu/standard/1.0) | 
| Ubuntu 18\.04 | aws/codebuild/standard:2\.0 | [ubuntu/standard/2\.0](https://github.com/aws/aws-codebuild-docker-images/tree/master/ubuntu/standard/2.0) | 
| Windows Server Core 2016 | aws/codebuild/windows\-base:1\.0 | N/A | 
| Windows Server Core 2016 | aws/codebuild/windows\-base:2\.0 | N/A | 

 The standard image of the Ubuntu 18\.04 and Amazon Linux 2 platforms contain the following runtimes\. If you use the Amazon Linux 2 standard image 1\.0 or the Ubuntu standard image 2\.0, you must specify your runtime in the `runtime-versions` section of your buildspec file\. For more information, see [Specify Runtime Versions in the Buildspec File](build-spec-ref.md#runtime-versions-buildspec-file)\. 


| Runtime name | Version/versions | How to specify in the buildspec file | 
| --- | --- | --- | 
| android | 28 | android: 28 | 
| docker | 18 | docker: 18 | 
| dotnet | 2\.2 | dotnet: 2\.2 | 
| golang | 1\.12 | golang: 1\.12 | 
| nodejs | 8, 10 | nodejs: 8, nodejs: 10 | 
| java \(Ubuntu only\) | openjdk8, openjdk11 | java: openjdk8, java: openjdk11 | 
| corretto \(Amazon Linux 2 only\) | corretto8, corretto11 | java: corretto8, java: corretto11 | 
| php | 7\.3 | php: 7\.3 | 
| python | 3\.7 | python: 3\.7 | 
| ruby | 2\.6 | ruby: 2\.6 | 

 The base image of the Windows Server Core 2016 contains the following runtimes\. 


| Runtime name | Version in `windows-base:1.0` | Version in `windows-base:2.0` | 
| --- | --- | --- | 
| dotnet | 2\.1 | 2\.2 | 
| golang | 1\.11 | 1\.12 | 
| nodejs | 9\.16 | 10\.16 | 
| java | openjdk8 | openjdk11 | 
| php | 7\.2 | 7\.3 | 
| python | 3\.6 | 3\.7 | 
| ruby | 2\.4 | 2\.6 | 

**Note**  
 The base image of the Windows Server Core 2016 platform is available in the US East \(N\. Virginia\), US East \(Ohio\), US West \(Oregon\), and EU \(Ireland\) regions only\. 

You can use a build specification to install other components \(for example, the AWS CLI, Apache Maven, Apache Ant, Mocha, RSpec, or similar\) during the `install` build phase\. For more information, see [Build Spec Example](build-spec-ref.md#build-spec-ref-example)\.

CodeBuild frequently updates the list of Docker images\. To get the most current list, do one of the following:
+ In the CodeBuild console, in the **Create build project** wizard or **Edit Build Project** page, for **Environment image**, choose **Managed image**\. Choose from the **Operating system**, **Runtime**, and **Runtime version** drop\-down lists\. For more information, see [Create a Build Project \(Console\)](create-project.md#create-project-console) or [Change a Build Project's Settings \(Console\)](change-project.md#change-project-console)\.
+ For the AWS CLI, run the `list-curated-environment-images` command:

  ```
  aws codebuild list-curated-environment-images
  ```
+ For the AWS SDKs, call the `ListCuratedEnvironmentImages` operation for your target programming language\. For more information, see the [AWS SDKs and Tools Reference](sdk-ref.md)\.