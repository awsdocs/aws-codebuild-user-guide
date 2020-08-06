# Plan a build in AWS CodeBuild<a name="planning"></a>

Before you use AWS CodeBuild, you must answer these questions:

1. **Where is the source code stored?** CodeBuild currently supports building from the following source code repository providers\. The source code must contain a build specification \(buildspec\) file\. A *buildspec* is a collection of build commands and related settings, in YAML format, that CodeBuild uses to run a build\. You can declare a buildspec in a build project definition\.   
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codebuild/latest/userguide/planning.html)

1. **Which build commands do you need to run and in what order?** By default, CodeBuild downloads the build input from the provider you specify and uploads the build output to the bucket you specify\. You use the buildspec to instruct how to turn the downloaded build input into the expected build output\. For more information, see the [Buildspec reference](build-spec-ref.md)\.

1. **Which runtimes and tools do you need to run the build?** For example, are you building for Java, Ruby, Python, or Node\.js? Does the build need Maven or Ant or a compiler for Java, Ruby, or Python? Does the build need Git, the AWS CLI, or other tools? 

   CodeBuild runs builds in build environments that use Docker images\. These Docker images must be stored in a repository type supported by CodeBuild\. These include the CodeBuild Docker image repository, Docker Hub, and Amazon Elastic Container Registry \(Amazon ECR\)\. For more information about the CodeBuild Docker image repository, see [Docker images provided by CodeBuild](build-env-ref-available.md)\.

1. **Do you need AWS resources that aren't provided automatically by CodeBuild? If so, which security policies do those resources need?** For example, you might need to modify the CodeBuild service role to allow CodeBuild to work with those resources\. 

1. **Do you want CodeBuild to work with your VPC?** If so, you need the VPC ID, the subnet IDs, and security group IDs for your VPC configuration\. For more information, see [Use AWS CodeBuild with Amazon Virtual Private Cloud](vpc-support.md)\.

After you have answered these questions, you should have the settings and resources you need to run a build successfully\. To run your build, you can:
+ Use the AWS CodeBuild console, AWS CLI, or AWS SDKs\. For more information, see [Run CodeBuild directly](how-to-run.md)\.
+ Create or identify a pipeline in AWS CodePipeline, and then add a build or test action that instructs CodeBuild to automatically test your code, run your build, or both\. For more information, see [Use CodePipeline with CodeBuild](how-to-create-pipeline.md)\.