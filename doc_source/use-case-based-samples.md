# CodeBuild use case\-based samples<a name="use-case-based-samples"></a>

You can use these use case\-based samples to experiment with AWS CodeBuild:

[ Access token sample ](sample-access-tokens.md)  
Shows how to use access tokens in CodeBuild to connect to GitHub and Bitbucket\.

[Amazon ECR sample](sample-ecr.md)  
Uses a Docker image in an Amazon ECR repository to use Apache Maven to produce a single JAR file\.

[Amazon EFS sample](sample-efs.md)  
Shows how to configure a buildspec file so that a CodeBuild project mounts and builds on an Amazon EFS file system\.

[AWS CodeDeploy sample](sample-codedeploy.md)  
Uses Apache Maven to produce a single JAR file\. Uses CodeDeploy to deploy the JAR file to an Amazon Linux instance\. You can also use CodePipeline to build and deploy the sample\.

[ AWS CodePipeline integration with batch builds sample  AWS CodePipeline integration with multiple input sources and output artifacts sample ](sample-pipeline-multi-input-output.md)  
Shows how to use AWS CodePipeline to create a build with multiple input sources and multiple output artifacts\.

[AWS Config sample](how-to-integrate-config.md)  
Shows how to set up AWS Config\. Lists which CodeBuild resources are tracked and describes how to look up CodeBuild projects in AWS Config\.

[AWS Elastic Beanstalk sample](sample-elastic-beanstalk.md)  
Uses Apache Maven to produce a single WAR file\. Uses Elastic Beanstalk to deploy the WAR file to an Elastic Beanstalk instance\.

[Bitbucket pull request and webhook filter sample](sample-bitbucket-pull-request.md)  
Uses CodeBuild with Bitbucket as the source repository and webhooks enabled, to rebuild the source code every time a code change is pushed to the repository\.

[Build badges sample](sample-build-badges.md)  
Shows how to set up CodeBuild with build badges\.

[Build notifications sample](sample-build-notifications.md)  
Uses Apache Maven to produce a single JAR file\. Sends a build notification to subscribers of an Amazon SNS topic\.

[Create a test report using the AWS CLI sample](sample-test-report-cli.md)  
Uses the AWS CLI to create, run, and view the results of a test report\.

[Docker in custom image sample](sample-docker-custom-image.md)  
Uses a custom Docker image to produce a Docker image\.

[Docker sample](sample-docker.md)  
Uses a build image provided by CodeBuild with Docker support to produce a Docker image with Apache Maven\. Pushes the Docker image to a repository in Amazon ECR\. You can also adapt this sample to push the Docker image to Docker Hub\.

[GitHub Enterprise Server sample](sample-github-enterprise.md)  
Uses CodeBuild with GitHub Enterprise Server as the source repository, with certificates installed and webhooks enabled, to rebuild the source code every time a code change is pushed to the repository\.

[GitHub pull request and webhook filter sample](sample-github-pull-request.md)  
Uses CodeBuild with GitHub as the source repository and webhooks enabled, to rebuild the source code every time a code change is pushed to the repository\.

[ Host build output in an S3 bucket ](sample-disable-artifact-encryption.md)  
Shows how to create a static website in an S3 bucket using unencrypted build artifacts\.

[ Multiple input sources and output artifacts sample ](sample-multi-in-out.md)  
Shows how to use multiple input sources and multiple output artifacts in a build project\.

[Private registry with AWS Secrets Manager sample](sample-private-registry.md)  
Shows how to use a Docker image in a private registry as the runtime environment when building with CodeBuild The private registry credentials are stored in AWS Secrets Manager\.

[ Runtime versions in buildspec file sample ](sample-runtime-versions.md)  
Shows how to specify runtimes and their versions in the buildspec file\. This is a requirement when using the Ubuntu standard image version 2\.0\.

[Source version sample](sample-source-version.md)  
Shows how to use a specific version of your source in a CodeBuild build project\.

[Use semantic versioning to name build artifacts sample](sample-buildspec-artifact-naming.md)  
Shows how to use semantic versioning to create an artifact name at build time\.