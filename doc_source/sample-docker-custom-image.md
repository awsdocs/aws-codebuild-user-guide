# Docker in Custom Image Sample for CodeBuild<a name="sample-docker-custom-image"></a>

This sample builds and runs a Docker image by using AWS CodeBuild and a custom Docker build image \(`docker:dind` in Docker Hub\)\. 

To learn how to build a Docker image by using a build image provided by CodeBuild with Docker support instead, see our [Docker Sample](sample-docker.md)\.

**Important**  
Running this sample may result in charges to your AWS account\. These include possible charges for CodeBuild and for AWS resources and actions related to Amazon S3, AWS KMS, and CloudWatch Logs\. For more information, see [CodeBuild Pricing](http://aws.amazon.com/codebuild/pricing), [Amazon S3 Pricing](http://aws.amazon.com/s3/pricing), [AWS Key Management Service Pricing](http://aws.amazon.com/kms/pricing), and [Amazon CloudWatch Pricing](http://aws.amazon.com/cloudwatch/pricing)\.

**Topics**
+ [Running the Sample](#sample-docker-custom-image-running)
+ [Directory Structure](#sample-docker-custom-image-dir)
+ [Files](#sample-docker-custom-image-files)
+ [Related Resources](#w39aac11c41c34c17)

## Running the Sample<a name="sample-docker-custom-image-running"></a>

To run this sample:

1. Create the files as described in the Directory Structure and Files sections of this topic, and then upload them to an Amazon S3 input bucket or an AWS CodeCommit, GitHub, or Bitbucket repository\. 
**Important**  
Do not upload `(root directory name)`, just the files inside of `(root directory name)`\.   
If you are using an Amazon S3 input bucket, be sure to create a ZIP file that contains the files, and then upload it to the input bucket\. Do not add `(root directory name)` to the ZIP file, just the files inside of `(root directory name)`\.

1. Create a build project, run the build, and view related build information by following the steps in [Run AWS CodeBuild Directly](how-to-run.md)\.

   If you use the AWS CLI to create the build project, the JSON\-formatted input to the`create-project` command might look similar to this\. \(Replace the placeholders with your own values\.\)

   ```
   {
     "name": "sample-docker-custom-image-project",
     "source": {
       "type": "S3",
       "location": "codebuild-region-ID-account-ID-input-bucket/DockerCustomImageSample.zip"
     },
     "artifacts": {
       "type": "NO_ARTIFACTS"
     },
     "environment": {
       "type": "LINUX_CONTAINER",
       "image": "docker:dind",
       "computeType": "BUILD_GENERAL1_SMALL",
       "privilegedMode": true
     },
     "serviceRole": "arn:aws:iam::account-ID:role/role-name",
     "encryptionKey": "arn:aws:kms:region-ID:account-ID:key/key-ID"
   }
   ```

1. To see the build results, look in the build's log for the string `Hello, World!`\. For more information, see [View Build Details](view-build-details.md)\.

## Directory Structure<a name="sample-docker-custom-image-dir"></a>

This sample assumes this directory structure\.

```
(root directory name)
    |-- buildspec.yml
    `-- Dockerfile
```

## Files<a name="sample-docker-custom-image-files"></a>

The base image of the operating system used in this sample is Ubuntu\. The sample uses these files\. For more information about the OverlayFS storage driver referenced in the buildspec file, see [Use the OverlayFS storage driver](https://docs.docker.com/storage/storagedriver/overlayfs-driver/)\.

`buildspec.yml` \(in `(root directory name)`\)

```
version: 0.2

phases:
  install:
    commands:
      - nohup /usr/local/bin/dockerd --host=unix:///var/run/docker.sock --host=tcp://127.0.0.1:2375 --storage-driver=overlay2&
      - timeout 15 sh -c "until docker info; do echo .; sleep 1; done"
  pre_build:
    commands:
      - docker build -t helloworld .
  build:
    commands:
      - docker images
      - docker run helloworld echo "Hello, World!"
```

**Note**  
 If the base operating system is Alpine Linux, in the `buildspec.yml` add the `-t` argument to `timeout`:   

```
- timeout -t 15 sh -c "until docker info; do echo .; sleep 1; done"
```

`Dockerfile` \(in `(root directory name)`\)

```
FROM maven:3.3.9-jdk-8
 
RUN echo "Hello World"
```

## Related Resources<a name="w39aac11c41c34c17"></a>
+ For more information about getting started with AWS CodeBuild, see [Getting Started with CodeBuild](getting-started.md)\.
+ For more information about troubleshooting problems with CodeBuild, see [Troubleshooting CodeBuild](troubleshooting.md)\.
+ For more information about limits in CodeBuild, see [Limits for CodeBuild](limits.md)\.