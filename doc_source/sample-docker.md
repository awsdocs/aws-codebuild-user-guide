# Docker sample for CodeBuild<a name="sample-docker"></a>

This sample produces as build output a Docker image and then pushes the Docker image to an Amazon Elastic Container Registry \(Amazon ECR\) image repository\. You can adapt this sample to push the Docker image to Docker Hub\. For more information, see [Adapting the sample to push the image to Docker Hub](#sample-docker-docker-hub)\.

To learn how to build a Docker image by using a custom Docker build image \(`docker:dind` in Docker Hub\), see our [Docker in custom image sample](sample-docker-custom-image.md)\.

This sample was tested referencing `golang:1.12`\.

This sample uses the new multi\-stage Docker builds feature, which produces a Docker image as build output\. It then pushes the Docker image to an Amazon ECR image repository\. Multi\-stage Docker image builds help to reduce the size of the final Docker image\. For more information, see [Use multi\-stage builds with Docker](https://docs.docker.com/engine/userguide/eng-image/multistage-build/)\.

**Important**  
Running this sample might result in charges to your AWS account\. These include possible charges for AWS CodeBuild and for AWS resources and actions related to Amazon S3, AWS KMS, CloudWatch Logs, and Amazon ECR\. For more information, see [CodeBuild pricing](http://aws.amazon.com/codebuild/pricing), [Amazon S3 pricing](http://aws.amazon.com/s3/pricing), [AWS Key Management Service pricing](http://aws.amazon.com/kms/pricing), [Amazon CloudWatch pricing](http://aws.amazon.com/cloudwatch/pricing), and [Amazon Elastic Container Registry pricing](http://aws.amazon.com/ecr/pricing)\.

**Topics**
+ [Running the sample](#sample-docker-running)
+ [Directory structure](#sample-docker-dir)
+ [Files](#sample-docker-files)
+ [Adapting the sample to push the image to Docker Hub](#sample-docker-docker-hub)
+ [Related resources](#acb-more-info)

## Running the sample<a name="sample-docker-running"></a>

**To run this sample**

1. If you already have an image repository in Amazon ECR you want to use, skip to step 3\. Otherwise, if you are using an IAM user instead of an AWS root account or an administrator IAM user to work with Amazon ECR, add this statement \(between *\#\#\# BEGIN ADDING STATEMENT HERE \#\#\#* and *\#\#\# END ADDING STATEMENT HERE \#\#\#*\) to the user \(or IAM group the user is associated with\)\. Using an AWS root account is not recommended\.This statement allows the creation of Amazon ECR repositories for storing Docker images\. Ellipses \(`...`\) are used for brevity and to help you locate where to add the statement\. Do not remove any statements, and do not type these ellipses into the policy\. For more information, see [Working with inline policies using the AWS Management Console](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_inline-using.html#AddingPermissions_Console) in the *IAM User Guide*\. 

   ```
   {
     "Statement": [
       ### BEGIN ADDING STATEMENT HERE ###
       {
         "Action": [
           "ecr:CreateRepository"
         ],
         "Resource": "*",
         "Effect": "Allow"
       },
       ### END ADDING STATEMENT HERE ###
       ...
     ],
     "Version": "2012-10-17"
   }
   ```
**Note**  
The IAM entity that modifies this policy must have permission in IAM to modify policies\.

1. Create an image repository in Amazon ECR\. Be sure to create the repository in the same AWS Region where you create your build environment and run your build\. For more information, see [Creating a repository](https://docs.aws.amazon.com/AmazonECR/latest/userguide/repository-create.html) in the *Amazon ECR User Guide*\. This repository's name must match the repository name you specify later in this procedure, represented by the `IMAGE_REPO_NAME` environment variable\.

1. Add this statement \(between *\#\#\# BEGIN ADDING STATEMENT HERE \#\#\#* and *\#\#\# END ADDING STATEMENT HERE \#\#\#*\) to the policy you attached to your AWS CodeBuild service role\. This statement allows CodeBuild to upload Docker images to Amazon ECR repositories\. Ellipses \(`...`\) are used for brevity and to help you locate where to add the statement\. Do not remove any statements, and do not type these ellipses into the policy\. 

   ```
   {
     "Statement": [
       ### BEGIN ADDING STATEMENT HERE ###
       {
         "Action": [
           "ecr:BatchCheckLayerAvailability",
           "ecr:CompleteLayerUpload",
           "ecr:GetAuthorizationToken",
           "ecr:InitiateLayerUpload",
           "ecr:PutImage",
           "ecr:UploadLayerPart"
         ],
         "Resource": "*",
         "Effect": "Allow"
       },
       ### END ADDING STATEMENT HERE ###
       ...
     ],
     "Version": "2012-10-17"
   }
   ```
**Note**  
The IAM entity that modifies this policy must have permission in IAM to modify policies\.

1. Create the files as described in the "Directory structure" and "Files" sections of this topic, and then upload them to an S3 input bucket or an AWS CodeCommit, GitHub, or Bitbucket repository\. 
**Important**  
Do not upload `(root directory name)`, just the files inside of `(root directory name)`\.   
If you are using an S3 input bucket, be sure to create a ZIP file that contains the files, and then upload it to the input bucket\. Do not add `(root directory name)` to the ZIP file, just the files inside of `(root directory name)`\.

1. Follow the steps in [Run CodeBuild directly](how-to-run.md) to create a build project, run the build, and view build information\.

    If you use the console to create your project:

   1.  For **Operating system**, choose **Ubuntu**\. 

   1.  For **Runtime**, choose **Standard**\. 

   1.  For **Image**, choose **aws/codebuild/standard:4\.0**\. 

   1.  Because you use this build project to build a Docker image, select **Privileged**\. 
**Note**  
By default, Docker containers do not allow access to any devices\. Privileged mode grants a build project's Docker container access to all devices\. For more information, see [Runtime Privilege and Linux Capabilities](https://docs.docker.com/engine/reference/run/#runtime-privilege-and-linux-capabilities) on the Docker Docs website\.

   1.  Add the following environment variables: 
      +  AWS\_DEFAULT\_REGION with a value of *region\-ID* 
      +  AWS\_ACCOUNT\_ID with a value of *account\-ID* 
      +  IMAGE\_TAG with a value of Latest 
      +  IMAGE\_REPO\_NAME with a value of *Amazon\-ECR\-repo\-name* 

   If you use the AWS CLI to create the build project, the JSON\-formatted input to the `create-project` command might look similar to this\. \(Replace the placeholders with your own values\.\)

   ```
   {
     "name": "sample-docker-project",
     "source": {
       "type": "S3",
       "location": "codebuild-region-ID-account-ID-input-bucket/DockerSample.zip"
     },
     "artifacts": {
       "type": "NO_ARTIFACTS"
     },
     "environment": {
       "type": "LINUX_CONTAINER",
       "image": "aws/codebuild/standard:4.0",
       "computeType": "BUILD_GENERAL1_SMALL",
       "environmentVariables": [
         {
           "name": "AWS_DEFAULT_REGION",
           "value": "region-ID"
         },
         {
           "name": "AWS_ACCOUNT_ID",
           "value": "account-ID"
         },
         {
           "name": "IMAGE_REPO_NAME",
           "value": "Amazon-ECR-repo-name"
         },
         {
           "name": "IMAGE_TAG",
           "value": "latest"
         }
       ],
       "privilegedMode": true
     },
     "serviceRole": "arn:aws:iam::account-ID:role/role-name",
     "encryptionKey": "arn:aws:kms:region-ID:account-ID:key/key-ID"
   }
   ```

1. Confirm that CodeBuild successfully pushed the Docker image to the repository:

   1. Open the Amazon ECR console at [https://console\.aws\.amazon\.com/ecr/](https://console.aws.amazon.com/ecr/)\.

   1. Choose the repository name\. The image should be listed in the **Image tag** column\.

## Directory structure<a name="sample-docker-dir"></a>

This sample assumes this directory structure\.

```
(root directory name)
    |-- buildspec.yml
    `-- Dockerfile
```

## Files<a name="sample-docker-files"></a>

This sample uses these files\.

`buildspec.yml` \(in `(root directory name)`\)

**Note**  
If you are using a version of Docker earlier than 17\.06, remove the `--no-include-email` option\.

```
version: 0.2

phases:
  pre_build:
    commands:
      - echo Logging in to Amazon ECR...
      - $(aws ecr get-login --no-include-email --region $AWS_DEFAULT_REGION)
  build:
    commands:
      - echo Build started on `date`
      - echo Building the Docker image...          
      - docker build -t $IMAGE_REPO_NAME:$IMAGE_TAG .
      - docker tag $IMAGE_REPO_NAME:$IMAGE_TAG $AWS_ACCOUNT_ID.dkr.ecr.$AWS_DEFAULT_REGION.amazonaws.com/$IMAGE_REPO_NAME:$IMAGE_TAG      
  post_build:
    commands:
      - echo Build completed on `date`
      - echo Pushing the Docker image...
      - docker push $AWS_ACCOUNT_ID.dkr.ecr.$AWS_DEFAULT_REGION.amazonaws.com/$IMAGE_REPO_NAME:$IMAGE_TAG
```

`Dockerfile` \(in `(root directory name)`\)

```
FROM golang:1.12-alpine AS build
#Install git
RUN apk add --no-cache git
#Get the hello world package from a GitHub repository
RUN go get github.com/golang/example/hello
WORKDIR /go/src/github.com/golang/example/hello
# Build the project and send the output to /bin/HelloWorld 
RUN go build -o /bin/HelloWorld

FROM golang:1.12-alpine
#Copy the build's output binary from the previous build container
COPY --from=build /bin/HelloWorld /bin/HelloWorld
ENTRYPOINT ["/bin/HelloWorld"]
```

## Adapting the sample to push the image to Docker Hub<a name="sample-docker-docker-hub"></a>

To push the Docker image to Docker Hub instead of Amazon ECR, edit this sample's code\.
**Note**  
If you are using a version of Docker earlier than 17\.06, remove the `--no-include-email` option\.

1. Replace these Amazon ECR\-specific lines of code in the `buildspec.yml` file:

   ```
   ...
     pre_build:
       commands:
         - echo Logging in to Amazon ECR...
         - $(aws ecr get-login --no-include-email --region $AWS_DEFAULT_REGION)
     build:
       commands:
         - echo Build started on `date`
         - echo Building the Docker image...          
         - docker build -t $IMAGE_REPO_NAME:$IMAGE_TAG .
         - docker tag $IMAGE_REPO_NAME:$IMAGE_TAG $AWS_ACCOUNT_ID.dkr.ecr.$AWS_DEFAULT_REGION.amazonaws.com/$IMAGE_REPO_NAME:$IMAGE_TAG
     post_build:
       commands:
         - echo Build completed on `date`
         - echo Pushing the Docker image...
         - docker push $AWS_ACCOUNT_ID.dkr.ecr.$AWS_DEFAULT_REGION.amazonaws.com/$IMAGE_REPO_NAME:$IMAGE_TAG
   ...
   ```

   With these Docker Hub\-specific lines of code:

   ```
   ...
     pre_build:
       commands:
         - echo Logging in to Docker Hub...
         # Type the command to log in to your Docker Hub account here.          
     build:
       commands:
         - echo Build started on `date`
         - echo Building the Docker image...
         - docker build -t $IMAGE_REPO_NAME:$IMAGE_TAG .
         - docker tag $IMAGE_REPO_NAME:$IMAGE_TAG $IMAGE_REPO_NAME:$IMAGE_TAG
     post_build:
       commands:
         - echo Build completed on `date`
         - echo Pushing the Docker image...
         - docker push $IMAGE_REPO_NAME:$IMAGE_TAG
   ...
   ```

1. Upload the edited code to an S3 input bucket or an AWS CodeCommit, GitHub, or Bitbucket repository\. 
**Important**  
Do not upload `(root directory name)`, just the files inside of `(root directory name)`\.   
If you are using an S3 input bucket, be sure to create a ZIP file that contains the files, and then upload it to the input bucket\. Do not add `(root directory name)` to the ZIP file, just the files inside of `(root directory name)`\.

1. Replace these lines of code from the JSON\-formatted input to the `create-project` command:

   ```
   ...
       "environmentVariables": [
         {
           "name": "AWS_DEFAULT_REGION",
           "value": "region-ID"
         },
         {
           "name": "AWS_ACCOUNT_ID",
           "value": "account-ID"
         },
         {
           "name": "IMAGE_REPO_NAME",
           "value": "Amazon-ECR-repo-name"
         },
         {
           "name": "IMAGE_TAG",
           "value": "latest"
         }
       ]
   ...
   ```

   With these lines of code:

   ```
   ...
       "environmentVariables": [
         {
           "name": "IMAGE_REPO_NAME",
           "value": "your-Docker-Hub-repo-name"
         },
         {
           "name": "IMAGE_TAG",
           "value": "latest"
         }
       ]
   ...
   ```

1. Follow the steps in [Run CodeBuild directly](how-to-run.md) to create a build environment, run the build, and view related build information\.

1. Confirm that AWS CodeBuild successfully pushed the Docker image to the repository\. Sign in to Docker Hub, go to the repository, and choose the **Tags** tab\. The `latest` tag should contain a very recent **Last Updated** value\.

## Related resources<a name="acb-more-info"></a>
+ For information about getting started with AWS CodeBuild, see [Getting started with AWS CodeBuild using the console](getting-started.md)\.
+ For information about troubleshooting issues in CodeBuild, see [Troubleshooting AWS CodeBuild](troubleshooting.md)\.
+ For information about quotas in CodeBuild, see [Quotas for AWS CodeBuild](limits.md)\.