# Docker Images Provided by CodeBuild<a name="build-env-ref-available"></a>

AWS CodeBuild manages the following Docker images that are available in the CodeBuild and AWS CodePipeline consoles\.


****  

| Platform | Programming language or framework | Image identifier | Definition | 
| --- | --- | --- | --- | 
| Ubuntu 18\.04 | \(standard image\) | aws/codebuild/standard:2\.0 | [ubuntu/standard/2\.0](https://github.com/aws/aws-codebuild-docker-images/tree/master/ubuntu/standard/2.0) | 
| Ubuntu 18\.04 | \(standard image\) | aws/codebuild/standard:1\.0 | [ubuntu/standard/1\.0](https://github.com/aws/aws-codebuild-docker-images/tree/master/ubuntu/standard/1.0) | 
| Windows Server Core 2016 | \(base image\) | aws/codebuild/windows\-base:1\.0 | N/A | 

 The standard image of the Ubuntu 18\.04 platform contains the following programming languages\. If you use the Ubuntu standard image 2\.0, you can specify your runtime in the `runtime-versions` section of your buildspec file\. For more information, see [Specify Runtime Versions in the Buildspec File](build-spec-ref.md#runtime-versions-buildspec-file)\. 


****  

| Programming language | Runtime version/versions | 
| --- | --- | 
| Ruby | 2\.x | 
| Python | 3\.x | 
| PHP | 7\.x | 
| Node | 8\.x, 10\.x | 
| Java | 8, 11 | 
| Golang | 1\.x | 
| \.NET Core | 2\.x | 
| Docker | 18\.x | 
| Android | 28\.x | 

**Note**  
 The base image of the Windows Server Core 2016 platform is available in the US East \(N\. Virginia\), US East \(Ohio\), US West \(N\. California\), and EU \(Ireland\) regions only\. 

You can use a build specification to install other components \(for example, the AWS CLI, Apache Maven, Apache Ant, Mocha, RSpec, or similar\) during the `install` build phase\. For more information, see [Build Spec Example](build-spec-ref.md#build-spec-ref-example)\.

CodeBuild frequently updates the list of Docker images\. To get the most current list, do one of the following:
+ In the CodeBuild console, in the **Create build project** wizard or **Edit Build Project** page, for **Environment image**, choose **Managed image**\. Choose from the **Operating system**, **Runtime**, and **Runtime version** drop\-down lists\. For more information, see [Create a Build Project \(Console\)](create-project.md#create-project-console) or [Change a Build Project's Settings \(Console\)](change-project.md#change-project-console)\.
+ For the AWS CLI, run the `list-curated-environment-images` command:

  ```
  aws codebuild list-curated-environment-images
  ```
+ For the AWS SDKs, call the `ListCuratedEnvironmentImages` operation for your target programming language\. For more information, see the [AWS SDKs and Tools Reference](sdk-ref.md)\.