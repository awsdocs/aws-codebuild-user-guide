# Docker images provided by CodeBuild<a name="build-env-ref-available"></a>

AWS CodeBuild manages the following Docker images that are available in the CodeBuild and AWS CodePipeline consoles\.


| Platform | Image identifier | Definition | 
| --- | --- | --- | 
| Amazon Linux 2 | aws/codebuild/amazonlinux2\-x86\_64\-standard:3\.0 | [al2/standard/3\.0](https://github.com/aws/aws-codebuild-docker-images/tree/master/al2/x86_64/standard/3.0) | 
| Amazon Linux 2 | aws/codebuild/amazonlinux2\-x86\_64\-standard:2\.0 | [al2/standard/2\.0](https://github.com/aws/aws-codebuild-docker-images/tree/master/al2/x86_64/standard/2.0) | 
| Amazon Linux 2 | aws/codebuild/amazonlinux2\-aarch64\-standard:1\.0 | [al2/aarch64/standard/1\.0](https://github.com/aws/aws-codebuild-docker-images/tree/master/al2/aarch64/standard/1.0) | 
| Ubuntu 18\.04 | aws/codebuild/standard:4\.0 | [ubuntu/standard/4\.0](https://github.com/aws/aws-codebuild-docker-images/tree/master/ubuntu/standard/4.0) | 
| Ubuntu 18\.04 | aws/codebuild/standard:3\.0 | [ubuntu/standard/3\.0](https://github.com/aws/aws-codebuild-docker-images/tree/master/ubuntu/standard/3.0) | 
| Ubuntu 18\.04 | aws/codebuild/standard:2\.0 ¹ | [ubuntu/standard/2\.0](https://github.com/aws/aws-codebuild-docker-images/tree/master/ubuntu/standard/2.0) | 
| Windows Server Core 2016 | aws/codebuild/windows\-base:2\.0 | N/A | 
| Windows Server Core 2019 | aws/codebuild/windows\-base:2019\-1\.0 | N/A | 

¹ No longer maintained after June 2020\.

 The latest version of each image is cached\. If you specify a more specific version, then CodeBuild provisions that version instead of the cached version\. This can result in longer build times\. For example, to benefit from caching, specify `aws/codebuild/amazonlinux2-x86_64-standard:3.0` instead of a more granular version, such as `aws/codebuild/amazonlinux2-x86_64-standard:3.0-1.0.0`\. 

 You can specify one or more runtimes in the `runtime-versions` section of your buildspec file\. If your runtime is dependent upon another runtime, you can also specify its dependent runtime in the buildspec file\. If you do not specify any runtimes in the buildspec file, CodeBuild chooses the default runtimes that are available in the image you use\. If you specify one or more runtimes, CodeBuild uses only those runtimes\. If a dependent runtime is not specified, CodeBuild attempts to choose the dependent runtime for you\. For more information, see [Specify runtime versions in the buildspec file](build-spec-ref.md#runtime-versions-buildspec-file)\. 

 When you specify a runtime in the `runtime-versions` section of your buildspec file, you can specify a specific version, a specific major version and the latest minor version, or the latest version\. The following table lists the available runtimes and how to specify them\. 


**Ubuntu 18\.04 and Amazon Linux 2 platforms runtimes**  
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codebuild/latest/userguide/build-env-ref-available.html)

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

The base image of the Windows Server Core 2019 contains the following runtimes\.


| Runtime name | Version in `windows-base:2019-1.0` | 
| --- | --- | 
| dotnet | 3\.1\.3 | 
| golang | 1\.14 | 
| nodejs | 12\.18 | 
| java | corretto11 | 
| php | 7\.4\.7 | 
| powershell | 7\.0\.2 | 
| python | 3\.8\.3 | 
| ruby | 2\.7 | 

**Note**  
 The base image of the Windows Server Core 2016 and Windows Server Core 2016 platforms are available in the US East \(N\. Virginia\), US East \(Ohio\), US West \(Oregon\), and Europe \(Ireland\) Regions only\. 

You can use a build specification to install other components \(for example, the AWS CLI, Apache Maven, Apache Ant, Mocha, RSpec, or similar\) during the `install` build phase\. For more information, see [Buildspec example](build-spec-ref.md#build-spec-ref-example)\.

CodeBuild frequently updates the list of Docker images\. To get the most current list, do one of the following:
+ In the CodeBuild console, in the **Create build project** wizard or **Edit Build Project** page, for **Environment image**, choose **Managed image**\. Choose from the **Operating system**, **Runtime**, and **Runtime version** drop\-down lists\. For more information, see [Create a build project \(console\)](create-project-console.md) or [Change a build project's settings \(console\)](change-project-console.md)\.
+ For the AWS CLI, run the `list-curated-environment-images` command:

  ```
  aws codebuild list-curated-environment-images
  ```
+ For the AWS SDKs, call the `ListCuratedEnvironmentImages` operation for your target programming language\. For more information, see the [AWS SDKs and tools reference](sdk-ref.md)\.