--------

 The procedures in this guide support the new console design\. If you choose to use the older version of the console, you will find many of the concepts and basic procedures in this guide still apply\. To access help in the new console, choose the information icon\.

--------

# Build Environment Reference for AWS CodeBuild<a name="build-env-ref"></a>

When you call AWS CodeBuild to run a build, you must provide information about the build environment\. A *build environment* represents a combination of operating system, programming language runtime, and tools that AWS CodeBuild uses to run a build\. For information about how a build environment works, see [How AWS CodeBuild Works](concepts.md#concepts-how-it-works)\.

A build environment contains a Docker image\. For information, see [Docker Glossary: Image](https://docs.docker.com/glossary/?term=image) on the Docker Docs website\. 

When you provide information to AWS CodeBuild about the build environment, you specify the identifier of a Docker image in a supported repository type\. These include the AWS CodeBuild Docker image repository, publicly available images in Docker Hub, and Amazon Elastic Container Registry \(Amazon ECR\) repositories in your AWS account:
+ We recommend that you use Docker images stored in the AWS CodeBuild Docker image repository, because they are optimized for use with the service\. For more information, see [Docker Images Provided by AWS CodeBuild](build-env-ref-available.md)\. 
+ To get the identifier of a publicly available Docker image stored in Docker Hub, see [Searching for Images](https://docs.docker.com/docker-hub/repos/#searching-for-images) on the Docker Docs website\.
+ To learn how to work with Docker images stored in Amazon ECR repositories in your AWS account, see [Amazon ECR Sample](sample-ecr.md)\.

In addition to a Docker image identifier, you also specify a set of computing resources that the build environment uses\. For more information, see [Build Environment Compute Types](build-env-ref-compute-types.md)\.

**Topics**
+ [Docker Images Provided by AWS CodeBuild](build-env-ref-available.md)
+ [Build Environment Compute Types](build-env-ref-compute-types.md)
+ [Shells and Commands in Build Environments](build-env-ref-cmd.md)
+ [Environment Variables in Build Environments](build-env-ref-env-vars.md)
+ [Background Tasks in Build Environments](build-env-ref-background-tasks.md)