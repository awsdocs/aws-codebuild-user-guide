# Docker Sample for AWS CodeBuild<a name="sample-docker"></a>

This sample produces as build output a Docker image and then pushes the Docker image to an Amazon Elastic Container Registry \(Amazon ECR\) image repository\. You can adapt this sample to push the Docker image to Docker Hub\. For more information, see [Adapting the Sample to Push the Image to Docker Hub](#sample-docker-docker-hub)\.

To learn how to build a Docker image by using a custom Docker build image instead \(`docker:dind` in Docker Hub\), see our [Docker in Custom Image Sample](sample-docker-custom-image.md)\.

This sample was tested referencing `golang:1.9`

This sample uses the new multi\-stage Docker builds feature, which produces a Docker image as build output\. It then pushes the Docker image to an Amazon ECR image repository\. Multi\-stage Docker image builds help to reduce the size of the final Docker image\. For more information, see [Use multi\-stage builds with Docker](https://docs.docker.com/engine/userguide/eng-image/multistage-build/)\.

**Important**  
Running this sample may result in charges to your AWS account\. These include possible charges for AWS CodeBuild and for AWS resources and actions related to Amazon S3, AWS KMS, CloudWatch Logs, and Amazon ECR\. For more information, see [AWS CodeBuild Pricing](http://aws.amazon.com/codebuild/pricing), [Amazon S3 Pricing](http://aws.amazon.com/s3/pricing), [AWS Key Management Service Pricing](http://aws.amazon.com/kms/pricing), [Amazon CloudWatch Pricing](http://aws.amazon.com/cloudwatch/pricing), and [Amazon Elastic Container Registry Pricing](http://aws.amazon.com/ecr/pricing)\.

**Topics**
+ [Running the Sample](#sample-docker-running)
+ [Directory Structure](#sample-docker-dir)
+ [Files](#sample-docker-files)
+ [Adapting the Sample to Push the Image to Docker Hub](#sample-docker-docker-hub)
+ [Related Resources](#w4aab9c45b9c23)

## Running the Sample<a name="sample-docker-running"></a>

To run this sample:

1. If you already have an image repository in Amazon ECR you want to use, skip to step 3\. Otherwise, if you are using an IAM user instead of an AWS root account or an administrator IAM user to work with Amazon ECR, add this statement \(between *\#\#\# BEGIN ADDING STATEMENT HERE \#\#\#* and *\#\#\# END ADDING STATEMENT HERE \#\#\#*\) to the user \(or IAM group the user is associated with\)\. \(Using an AWS root account is not recommended\.\) This statement enables creating Amazon ECR repositories for storing Docker images\. Ellipses \(`...`\) are used for brevity and to help you locate where to add the statement\. Do not remove any statements, and do not type these ellipses into the policy\. For more information, see [Working with Inline Policies Using the AWS Management Console](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_inline-using.html#AddingPermissions_Console) in the *IAM User Guide*\. 

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

1. Create an image repository in Amazon ECR\. Be sure to create the repository in the same AWS region where you will be creating your build environment and running your build\. For more information, see [Creating a Repository](https://docs.aws.amazon.com/AmazonECR/latest/userguide/repository-create.html) in the *Amazon ECR User Guide*\. This repository's name must match the repository name you will specify later in this procedure, represented by the `IMAGE_REPO_NAME` environment variable\.

1. Add this statement \(between *\#\#\# BEGIN ADDING STATEMENT HERE \#\#\#* and *\#\#\# END ADDING STATEMENT HERE \#\#\#*\) to the policy you attached to your AWS CodeBuild service role\. This statement enables AWS CodeBuild to upload Docker images to Amazon ECR repositories\. Ellipses \(`...`\) are used for brevity and to help you locate where to add the statement\. Do not remove any statements, and do not type these ellipses into the policy\. 

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

1. Create the files as described in the Directory Structure and Files sections of this topic, and then upload them to an Amazon S3 input bucket or an AWS CodeCommit, GitHub, or Bitbucket repository\. 
**Important**  
Do not upload `(root directory name)`, just the files inside of `(root directory name)`\.   
If you are using an Amazon S3 input bucket, be sure to create a ZIP file that contains the files, and then upload it to the input bucket\. Do not add `(root directory name)` to the ZIP file, just the files inside of `(root directory name)`\.

1. Create a build project, run the build, and view related build information by following the steps in [Run AWS CodeBuild Directly](how-to-run.md)\.

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
       "image": "aws/codebuild/docker:17.09.0",
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
       ]
     },
     "serviceRole": "arn:aws:iam::account-ID:role/role-name",
     "encryptionKey": "arn:aws:kms:region-ID:account-ID:key/key-ID"
   }
   ```

1. Confirm that AWS CodeBuild successfully pushed the Docker image to the repository:

   1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

   1. Choose **Repositories**\.

   1. Choose the repository name\. The image should be listed on the **Images** tab\.

## Directory Structure<a name="sample-docker-dir"></a>

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
If you are using Docker prior to version 17\.06, remove the `--no-include-email` option\.

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
FROM golang:1.9 as builder
RUN go get -d -v golang.org/x/net/html
RUN go get -d -v github.com/alexellis/href-counter/
WORKDIR /go/src/github.com/alexellis/href-counter/.
RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o app .

FROM alpine:latest
RUN apk --no-cache add ca-certificates
WORKDIR /root/
COPY --from=builder /go/src/github.com/alexellis/href-counter/app .
CMD ["./app"]
```

## Adapting the Sample to Push the Image to Docker Hub<a name="sample-docker-docker-hub"></a>

To push the Docker image to Docker Hub instead of Amazon ECR, modify this sample's code\.

1. Replace these Amazon ECR\-specific lines of code in the `buildspec.yml` file:
**Note**  
If you are using Docker prior to version 17\.06, remove the `--no-include-email` option\.

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

   With these Docker Hub\-specific lines of code\.

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

1. Upload the modified code to an Amazon S3 input bucket or an AWS CodeCommit, GitHub, or Bitbucket repository\. 
**Important**  
Do not upload `(root directory name)`, just the files inside of `(root directory name)`\.   
If you are using an Amazon S3 input bucket, be sure to create a ZIP file that contains the files, and then upload it to the input bucket\. Do not add `(root directory name)` to the ZIP file, just the files inside of `(root directory name)`\.

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

   With these lines of code\.

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

1. Follow the steps in [Run AWS CodeBuild Directly](how-to-run.md) to create a build environment, run the build, and view related build information\.

1. Confirm that AWS CodeBuild successfully pushed the Docker image to the repository\. Sign in to Docker Hub, go to the repository, and choose the **Tags** tab\. The `latest` tag should contain a very recent **Last Updated** value\.

## Related Resources<a name="w4aab9c45b9c23"></a>
+ For more information about getting started with AWS CodeBuild, see [Getting Started with AWS CodeBuild](getting-started.md)\.
+ For more information about troubleshooting problems with AWS CodeBuild, see [Troubleshooting AWS CodeBuild](troubleshooting.md)\.
+ For more information about limits in AWS CodeBuild, see [Limits for AWS CodeBuild](limits.md)\.