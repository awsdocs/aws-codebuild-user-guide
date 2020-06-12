# Build environment reference for AWS CodeBuild<a name="build-env-ref"></a>

When you call AWS CodeBuild to run a build, you must provide information about the build environment\. A *build environment* represents a combination of operating system, programming language runtime, and tools that CodeBuild uses to run a build\. For information about how a build environment works, see [How CodeBuild works](concepts.md#concepts-how-it-works)\.

A build environment contains a Docker image\. For information, see [the Docker glossary](https://docs.docker.com/glossary/?term=image) on the Docker Docs website\. 

When you provide information to CodeBuild about the build environment, you specify the identifier of a Docker image in a supported repository type\. These include the CodeBuild Docker image repository, publicly available images in Docker Hub, and Amazon Elastic Container Registry \(Amazon ECR\) repositories that your AWS account has permissions to access\.
+ We recommend that you use Docker images stored in the CodeBuild Docker image repository, because they are optimized for use with the service\. For more information, see [Docker images provided by CodeBuild](build-env-ref-available.md)\. 
+ To get the identifier of a publicly available Docker image stored in Docker Hub, see [Searching for Repositories](https://docs.docker.com/docker-hub/repos/#searching-for-repositories) on the Docker Docs website\.
+ To learn how to work with Docker images stored in Amazon ECR repositories in your AWS account, see [Amazon ECR sample](sample-ecr.md)\.

In addition to a Docker image identifier, you also specify a set of computing resources that the build environment uses\. For more information, see [Build environment compute types](build-env-ref-compute-types.md)\.

**Topics**
+ [Docker images provided by CodeBuild](build-env-ref-available.md)
+ [Build environment compute types](build-env-ref-compute-types.md)
+ [Shells and commands in build environments](build-env-ref-cmd.md)
+ [Environment variables in build environments](build-env-ref-env-vars.md)
+ [Background tasks in build environments](build-env-ref-background-tasks.md)