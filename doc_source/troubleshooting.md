# Troubleshooting CodeBuild<a name="troubleshooting"></a>

Use the information in this topic to help you identify, diagnose, and address issues\.

**Topics**
+ [Error: "CodeBuild is not authorized to perform: sts:AssumeRole" when creating or updating a build project](#troubleshooting-assume-role)
+ [Error: "This build image requires selecting at least one runtime version\."](#troubleshooting-build-must-specify-runtime)
+ [Error: "Cannot connect to the Docker daemon" when running a build](#troubleshooting-cannot-connect-to-docker-daemon)
+ [Warning: "Skipping install of runtimes\. Runtime version selection is not supported by this build image" when running a build](#troubleshooting-skipping-all-runtimes-warning)
+ [Error: "The bucket you are attempting to access must be addressed using the specified endpoint" when running a build](#troubleshooting-input-bucket-different-region)
+ [Error: "error calling GetBucketAcl: either the bucket owner has changed or the service role no longer has permission to called s3:GetBucketAcl"](#troubleshooting-calling-bucket-error)
+ [Error: "Failed to upload artifacts: Invalid arn" when running a build](#troubleshooting-output-bucket-different-region)
+ [Error: "Unable to Locate Credentials"](#troubleshooting-versions)
+ [Earlier Commands in Buildspec Files Are Not Recognized by Later Commands](#troubleshooting-build-spec-commands)
+ [Apache Maven Builds Reference Artifacts from the Wrong Repository](#troubleshooting-maven-repos)
+ [Build Commands Run as root by Default](#troubleshooting-root-build-commands)
+ [The Bourne Shell \(sh\) Must Exist in Build Images](#troubleshooting-sh-build-images)
+ [Error: "CodeBuild is experiencing an issue" when running a build](#troubleshooting-large-env-vars)
+ [Error: "BUILD\_CONTAINER\_UNABLE\_TO\_PULL\_IMAGE" when using a custom build image](#troubleshooting-unable-to-pull-image)
+ [Builds Might Fail When File Names Have Non\-U\.S\. English Characters](#troubleshooting-utf-8)
+ [Builds Might Fail When Getting Parameters from Amazon EC2 Parameter Store](#troubleshooting-parameter-store)
+ [Cannot Access Branch Filter in the CodeBuild Console](#troubleshooting-webhook-filter)
+ [Procedures in This Guide Do Not Match the CodeBuild Console](#troubleshooting-old-console)
+ ["Access denied" error message when attempting to download cache](#troubleshooting-dependency-caching)
+ [Error: "Unable to download cache: RequestError: send request failed caused by: x509: failed to load system roots and no roots provided"](#troubleshooting-cache-image)
+ [Error: "Unable to download certificate from S3\. AccessDenied"](#troubleshooting-certificate-in-S3)
+ [Error: "Git Clone Failed: unable to access `'your-repository-URL'`: SSL certificate problem: self signed certificate"](#troubleshooting-self-signed-certificate)
+ [Error: "The policy's default version was not created by enhanced zero click role creation or was not the most recent version created by enhanced zero click role creation\."](#enhanced-zero-click-role-creation)
+ [Error: "Build container found dead before completing the build\. Build container died because it was out of memory, or the Docker image is not supported\. ErrorCode: 500"](#windows-server-core-version)
+ [Cannot view build success or failure](#no-status-when-build-triggered)
+ [Cannot find and select the base image of the Windows Server Core 2016 platform](#windows-image-not-available)
+ [RequestError timeout error when running CodeBuild in a proxy server](#code-request-timeout-error)
+ [Error: "QUEUED: INSUFFICIENT\_SUBNET" when a build in a build queue fails](#queued-insufficient-subnet-error)

## Error: "CodeBuild is not authorized to perform: sts:AssumeRole" when creating or updating a build project<a name="troubleshooting-assume-role"></a>

**Issue:** When you try to create or update a build project, you receive the following error: "Code:InvalidInputException, Message:CodeBuild is not authorized to perform: sts:AssumeRole on arn:aws:iam::*account\-ID*:role/*service\-role\-name*"\.

 **Possible causes:** 
+ The AWS Security Token Service \(AWS STS\) has been deactivated for the AWS region where you are attempting to create or update the build project\.
+ The AWS CodeBuild service role associated with the build project does not exist or does not have sufficient permissions to trust CodeBuild\.

 **Recommended solutions:** 
+ Make sure AWS STS is activated for the AWS region where you are attempting to create or update the build project\. For more information, see [Activating and Deactivating AWS STS in an AWS Region](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_enable-regions.html) in the *IAM User Guide*\.
+ Make sure the target CodeBuild service role exists in your AWS account\. If you are not using the console, make sure you did not misspell the Amazon Resource Name \(ARN\) of the service role when you created or updated the build project\.
+ Make sure the target CodeBuild service role has sufficient permissions to trust CodeBuild\. For more information, see the trust relationship policy statement in [Create a CodeBuild Service Role](setting-up.md#setting-up-service-role)\.

## Error: "This build image requires selecting at least one runtime version\."<a name="troubleshooting-build-must-specify-runtime"></a>

**Issue:** When you run a build, the `DOWNLOAD_SOURCE` build phase fails with the error "YAML\_FILE\_ERROR: This build image requires selecting at least one runtime version\."

**Possible cause:** Your build uses version 2\.0 or later of the Ubuntu standard image and a runtime is not specified in the buildspec file\.

**Recommended solution:** If you use the `aws/codebuild/standard:2.0` CodeBuild managed image, you must specify a runtime version in the `runtime-versions` section of the buildspec file\. For example, you might use the following buildspec file for a project that uses PHP:

```
version: 0.2

phases:
  install:
    runtime-versions:
        php: 7.3
  build:
    commands:
      - php --version
artifacts:
  files:
    -  README.md
```

**Note**  
 If you specify a `runtime-versions` section and use an image other than Ubuntu Standard Image 2\.0 or later, or the Amazon Linux 2 \(AL2\) standard image 1\.0 or later, the build issues the warning, "Skipping install of runtimes\. Runtime version selection is not supported by this build image\." 

 For more information, see [Specify Runtime Versions in the Buildspec File](build-spec-ref.md#runtime-versions-buildspec-file)\. 

## Error: "Cannot connect to the Docker daemon" when running a build<a name="troubleshooting-cannot-connect-to-docker-daemon"></a>

**Issue: **Your build fails and you receive an error similar to `Cannot connect to the Docker daemon at unix:/var/run/docker.sock. Is the docker daemon running?` in the build log\.

**Possible cause: **You are not running your build in privileged mode\.

**Recommended solution: ** Run your build in privileged mode:

1. Open the CodeBuild console at [https://console\.aws\.amazon\.com/codebuild/](https://console.aws.amazon.com/codebuild/)\.

1.  In the navigation pane, choose **Build projects**, and then choose your build project\. 

1.  From **Edit**, choose **Environment**\. 

1.  Choose **Override images**, and then choose **Environment**\. 

1.  Specify your environment image, operating system, runtime, and image\. These should match your settings for the build that failed\. 

1.  Select **Privileged**\. 

1.  Choose **Update environment**\. 

1.  Choose **Start build** to retry your build\. 

## Warning: "Skipping install of runtimes\. Runtime version selection is not supported by this build image" when running a build<a name="troubleshooting-skipping-all-runtimes-warning"></a>

**Issue:** When you run a build, the build log contains the warning, "Skipping install of runtimes\. Runtime version selection is not supported by this build image\." 

**Possible cause:** Your build does not use version 2\.0 or later of the Ubuntu standard image and a runtime is specified in a `runtime-versions` section in your buildspec file\.

**Recommended solution:** Be sure your buildspec file does not contain a `runtime-versions` section\. The `runtime-versions` section is only required if you use the Ubuntu standard image version 2\.0 or higher\.

## Error: "The bucket you are attempting to access must be addressed using the specified endpoint" when running a build<a name="troubleshooting-input-bucket-different-region"></a>

**Issue:** When you run a build, the `DOWNLOAD_SOURCE` build phase fails with the error "The bucket you are attempting to access must be addressed using the specified endpoint\. Please send all future requests to this endpoint\."

**Possible cause:** Your pre\-built source code is stored in an Amazon S3 bucket, and that bucket is in a different AWS region than the AWS CodeBuild build project\.

**Recommended solution:** Update the build project's settings to point to a bucket that contains your pre\-built source code, and that bucket is in the same region as the build project\.

## Error: "error calling GetBucketAcl: either the bucket owner has changed or the service role no longer has permission to called s3:GetBucketAcl"<a name="troubleshooting-calling-bucket-error"></a>

**Issue:** When you run a build, you receive an error about a change in ownership of an Amazon S3 bucket and `GetBucketAcl` permissions\.

**Possible cause:** You added the `s3:GetBucketACL` and `s3:GetBucketLocation` permissions to your IAM role\. These permissions secure your project's Amazon S3 bucket and ensure that only you can access it\. After adding these permissions, the owner of the Amazon S3 bucket changed\.

**Recommended solution:** Verify you are an owner of the Amazon S3 bucket, and then add permissions to your IAM role again\. For more information, see [Secure Access to Amazon S3 Buckets](auth-and-access-control-iam-access-control-identity-based.md#secure-s3-buckets)\.

## Error: "Failed to upload artifacts: Invalid arn" when running a build<a name="troubleshooting-output-bucket-different-region"></a>

**Issue:** When you run a build, the `UPLOAD_ARTIFACTS` build phase fails with the error "Failed to upload artifacts: Invalid arn"\.

**Possible cause:** Your Amazon S3 output bucket \(the bucket where AWS CodeBuild stores its output from the build\) is in a different AWS region than the CodeBuild build project\.

**Recommended solution:** Update the build project's settings to point to an output bucket that is in the same region as the build project\.

## Error: "Unable to Locate Credentials"<a name="troubleshooting-versions"></a>

**Issue:** When you try to run the AWS CLI, use an AWS SDK, or call another similar component as part of a build, you get build errors that are directly related to the AWS CLI, AWS SDK, or component\. For example, you may get a build error such as "unable to locate credentials\."

 **Possible causes:** 
+ The version of the AWS CLI, AWS SDK, or component in the build environment is incompatible with AWS CodeBuild\.
+ You are running a Docker container within a build environment that uses Docker, and that Docker container does not have access to the necessary AWS credentials by default\.

 **Recommended solutions:** 
+ Make sure your build environment has the following version or higher of the AWS CLI, AWS SDK, or component\.
  + AWS CLI: 1\.10\.47
  + AWS SDK for C\+\+: 0\.2\.19
  + AWS SDK for Go: 1\.2\.5
  + AWS SDK for Java: 1\.11\.16
  + AWS SDK for JavaScript: 2\.4\.7
  + AWS SDK for PHP: 3\.18\.28
  + AWS SDK for Python \(Boto3\): 1\.4\.0
  + AWS SDK for Ruby: 2\.3\.22
  + Botocore: 1\.4\.37
  + CoreCLR: 3\.2\.6\-beta
  + Node\.js: 2\.4\.7
+ If you need to run a Docker container in a build environment and that container requires AWS credentials, you must pass through the credentials from the build environment to the container\. In your buildspec file, include a Docker `run` command such as the following, which in this example uses the `aws s3 ls` command to list your available Amazon S3 buckets\. The `-e` option passes through the necessary environment variables for your container to access AWS credentials\.

  ```
  docker run -e AWS_DEFAULT_REGION -e AWS_CONTAINER_CREDENTIALS_RELATIVE_URI your-image-tag aws s3 ls
  ```
+ If you are building a Docker image and the build requires AWS credentials \(for example, to download a file from Amazon S3\), you must pass through the credentials from the build environment to the Docker build process as follows\.

  1. In your source code's Dockerfile for the Docker image, specify the following `ARG` instructions\.

     ```
     ARG AWS_DEFAULT_REGION
     ARG AWS_CONTAINER_CREDENTIALS_RELATIVE_URI
     ```

  1. In your buildspec file, include a Docker `build` command such as the following\. The `--build-arg` options will set the necessary environment variables for your Docker build process to access the AWS credentials\.

     ```
     docker build --build-arg AWS_DEFAULT_REGION=$AWS_DEFAULT_REGION --build-arg AWS_CONTAINER_CREDENTIALS_RELATIVE_URI=$AWS_CONTAINER_CREDENTIALS_RELATIVE_URI -t your-image-tag .
     ```

## Earlier Commands in Buildspec Files Are Not Recognized by Later Commands<a name="troubleshooting-build-spec-commands"></a>

**Issue:** The results of one or more commands in your buildspec file are not recognized by later commands in the same buildspec file\. For example, a command might set a local environment variable, but a command run later might fail to get the value of that local environment variable\. 

**Possible cause:** In buildspec file version 0\.1, AWS CodeBuild runs each command in a separate instance of the default shell in the build environment\. This means that each command runs in isolation from all other commands\. By default, then, you cannot run a single command that relies on the state of any previous commands\. 

**Recommended solutions:** We recommend you use build spec version 0\.2, which solves this issue\. If you must use build spec version 0\.1 for some reason, we recommend using the shell command chaining operator \(for example, `&&` in Linux\) to combine multiple commands into a single command\. Or include a shell script in your source code that contains multiple commands, and then call that shell script from a single command in the buildspec file\. For more information, see [Shells and Commands in Build Environments](build-env-ref-cmd.md) and [Environment Variables in Build Environments](build-env-ref-env-vars.md)\.

## Apache Maven Builds Reference Artifacts from the Wrong Repository<a name="troubleshooting-maven-repos"></a>

**Issue:** When you use Maven with an AWS CodeBuild provided Java build environment, Maven pulls build and plugin dependencies from the secure central Maven repository at [https://repo1\.maven\.org/maven2](https://repo1.maven.org/maven2)\. This happens even if your build project's `pom.xml` file explicitly declares other locations to use instead\.

**Possible cause:** CodeBuild provided Java build environments include a file named `settings.xml` that is preinstalled in the build environment's `/root/.m2` directory\. This `settings.xml` file contains the following declarations, which instruct Maven to always pull build and plugin dependencies from the secure central Maven repository at [https://repo1\.maven\.org/maven2](https://repo1.maven.org/maven2)\.

```
<settings>
  <activeProfiles>
    <activeProfile>securecentral</activeProfile>
  </activeProfiles>
  <profiles>
    <profile>
      <id>securecentral</id>
      <repositories>
        <repository>
          <id>central</id>
          <url>https://repo1.maven.org/maven2</url>
          <releases>
            <enabled>true</enabled>
          </releases>
        </repository>
      </repositories>
      <pluginRepositories>
        <pluginRepository>
          <id>central</id>
          <url>https://repo1.maven.org/maven2</url>
          <releases>
            <enabled>true</enabled>
          </releases>
        </pluginRepository>
      </pluginRepositories>
    </profile>
  </profiles>
</settings>
```

**Recommended solution:** Do the following:

1. Add a `settings.xml` file to your source code\.

1. In this `settings.xml` file, use the preceding `settings.xml` format as a guide to declare the repositories you want Maven to pull the build and plugin dependencies from instead\.

1. In the `install` phase of your build project, instruct CodeBuild to copy your `settings.xml` file to the build environment's `/root/.m2` directory\. For example, consider the following snippet from a `buildspec.yml` file that demonstrates this behavior\. 

   ```
   version 0.2
   
   phases:
     install:
       commands:
         - cp ./settings.xml /root/.m2/settings.xml
   ```

## Build Commands Run as root by Default<a name="troubleshooting-root-build-commands"></a>

**Issue:** AWS CodeBuild runs your build commands as the root user\. This happens even if your related build image's Dockerfile sets the `USER` instruction to a different user\.

**Cause:** CodeBuild runs all build commands as the root user by default\.

**Recommended solution:** None\.

## The Bourne Shell \(sh\) Must Exist in Build Images<a name="troubleshooting-sh-build-images"></a>

**Issue:** You are using a build image that is not provided by AWS CodeBuild, and your builds fail with the message "build container found dead before completing the build\." 

**Possible cause:**The Bourne shell \(`sh`\) is not included in your build image\. CodeBuild needs `sh` to run build commands and scripts\.

**Recommended solution:** If `sh` in not present in your build image, be sure to include it before you start any more builds that use your image\. \(CodeBuild already includes `sh` in its build images\.\)

## Error: "CodeBuild is experiencing an issue" when running a build<a name="troubleshooting-large-env-vars"></a>

**Issue:** When you try to run a build project, you receive the following error during the build's `PROVISIONING` phase: "CodeBuild is experiencing an issue\."

**Possible cause:** Your build is using environment variables that are too large for AWS CodeBuild\. CodeBuild can raise errors once the length of all environment variables \(all names and values added together\) reach a combined maximum of around 5,500 characters\.

**Recommended solution:** Use Amazon EC2 Systems Manager Parameter Store to store large environment variables and then retrieve them from your buildspec file\. Amazon EC2 Systems Manager Parameter Store can store an individual environment variable \(name and value added together\) that is a combined 4,096 characters or less\. To store large environment variables, see [Systems Manager Parameter Store](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-paramstore.html) and [Systems Manager Parameter Store Console Walkthrough](https://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-paramstore-walk.html#sysman-paramstore-console) in the *Amazon EC2 Systems Manager User Guide*\. To retrieve them, see the `parameter-store` mapping in [Build Spec Syntax](build-spec-ref.md#build-spec-ref-syntax)\.

## Error: "BUILD\_CONTAINER\_UNABLE\_TO\_PULL\_IMAGE" when using a custom build image<a name="troubleshooting-unable-to-pull-image"></a>

**Issue:** When you try to run a build that uses a custom build image, the build fails with the error `BUILD_CONTAINER_UNABLE_TO_PULL_IMAGE`\.

 **Possible causes:** 
+ The build image's overall uncompressed size is larger than the build environment compute type's available disk space\. To check your build image's size, use Docker to run the `docker images REPOSITORY:TAG` command\. For a list of available disk space by compute type, see [Build Environment Compute Types](build-env-ref-compute-types.md)\.
+ AWS CodeBuild does not have permission to pull the build image from your Amazon Elastic Container Registry \(Amazon ECR\)\.
+  The Amazon ECR image you requested is not available in the region that your AWS account is using\. 

 **Recommended solutions:** 
+ Use a larger compute type with more available disk space, or reduce the size of your custom build image\.
+ Update the permissions in your repository in Amazon ECR so that CodeBuild can pull your custom build image into the build environment\. For more information, see the [Amazon ECR Sample](sample-ecr.md)\.
+  Use an Amazon ECR image that is in the same region as the one your AWS account is using\. 

## Builds Might Fail When File Names Have Non\-U\.S\. English Characters<a name="troubleshooting-utf-8"></a>

**Issue:** When you run a build that uses files with file names containing non\-US English characters \(for example, Chinese characters\), the build fails\. 

**Possible cause:** : Build environments provided by AWS CodeBuild have their default locale set to `POSIX`\. `POSIX` localization settings are less compatible with CodeBuild and file names that contain non\-US English characters and can cause related builds to fail\.

**Recommended solution:** Add the following commands to the `pre_build` section of your buildspec file\. These commands make the build environment use US English UTF\-8 for its localization settings, which is more compatible with CodeBuild and file names that contain non\-US English characters\.

For build environments based on Ubuntu:

```
pre_build:
  commands:
    - export LC_ALL="en_US.UTF-8"
    - locale-gen en_US en_US.UTF-8
    - dpkg-reconfigure locales
```

For build environments based on Amazon Linux:

```
pre_build:
  commands:
    - export LC_ALL="en_US.utf8"
```

## Builds Might Fail When Getting Parameters from Amazon EC2 Parameter Store<a name="troubleshooting-parameter-store"></a>

**Issue:** When a build tries to get the value of one or more parameters stored in Amazon EC2 Parameter Store, the build fails in the `DOWNLOAD_SOURCE` phase with the following error: "Parameter does not exist\."

**Possible cause:** The service role the build project relies on does not have permission to call the `ssm:GetParameters` action or the build project uses a service role that is generated by AWS CodeBuild and allows calling the `ssm:GetParameters` action, but the parameters have names that do not start with `/CodeBuild/`\.

 **Recommended solutions:** 
+ If the service role was not generated by CodeBuild, update its definition to allow CodeBuild to call the `ssm:GetParameters` action\. For example, the following policy statement allows calling the `ssm:GetParameters` action to get parameters with names starting with `/CodeBuild/`:

  ```
  {
    "Version": "2012-10-17",
    "Statement": [
      {
        "Action": "ssm:GetParameters",
        "Effect": "Allow",
        "Resource": "arn:aws:ssm:REGION_ID:ACCOUNT_ID:parameter/CodeBuild/*"
      }
    ]
  }
  ```
+  If the service role was generated by CodeBuild, update its definition to allow CodeBuild to access parameters in Amazon EC2 Parameter Store with names other than those starting with `/CodeBuild/`\. For example, the following policy statement allows calling the `ssm:GetParameters` action to get parameters with the specified name:

  ```
  {
    "Version": "2012-10-17",
    "Statement": [
      {
        "Action": "ssm:GetParameters",
        "Effect": "Allow",
        "Resource": "arn:aws:ssm:REGION_ID:ACCOUNT_ID:parameter/PARAMETER_NAME"
      }
    ]
  }
  ```

## Cannot Access Branch Filter in the CodeBuild Console<a name="troubleshooting-webhook-filter"></a>

**Issue:** The branch filter option is not available in the console when you create or update an AWS CodeBuild project\.

 **Possible cause:** The branch filter option is deprecated\. It has been replaced by webhook filter groups, which provide more control over the webhook events that trigger a new CodeBuild build\. 

**Recommended solution:** To migrate a branch filter created prior to the introduction of webhook filters, create a webhook filter groups with a `HEAD_REF` filter with the regular expression `^refs/heads/branchName$`\. For example, if your branch filter regular expression was `^branchName$`, then the updated regular expression you put in the `HEAD_REF` filter is `^refs/heads/branchName$`\. For more information, see [Filter BitBucket Webhook Events \(Console\)](sample-bitbucket-pull-request.md#sample-bitbucket-pull-request-filter-webhook-events-console) and [Filter GitHub Webhook Events \(Console\)](sample-github-pull-request.md#sample-github-pull-request-filter-webhook-events-console)\. 

## Procedures in This Guide Do Not Match the CodeBuild Console<a name="troubleshooting-old-console"></a>

**Issue:** This guide supports procedures in the new console design\.

 **Possible cause:** You are using the old console design\. This guide supports the new consolde design\. If you choose to use the older version of the console, you will find many of the concepts and basic procedures in this guide still apply\. To access help in the new console, choose the information icon\. 

**Recommended solution:** Use the latest console design\. 

## "Access denied" error message when attempting to download cache<a name="troubleshooting-dependency-caching"></a>

**Issue:** When attempting to download the cache on a build project that has cache enabled, you receive the following generic error: "Access denied"\.

 **Possible causes:** 
+ You have just configured caching as part of your build project\.
+ The cache has recently been invalidated via the `InvalidateProjectCache` API\.
+ The service role being used by CodeBuild does not have `s3:GetObject` and `s3:PutObject` permissions to the Amazon S3 bucket that is holding the cache\.

**Recommended solution:** For first time use, it's normal to see this immediately after updating the cache configuration\. If this error persists, then you should check to see if your service role has `s3:GetObject` and `s3:PutObject` permissions to the Amazon S3 bucket that is holding the cache\. For more information, see [Specifying S3 permissions\.](https://docs.aws.amazon.com/AmazonS3/latest/dev//using-with-s3-actions.html) 

## Error: "Unable to download cache: RequestError: send request failed caused by: x509: failed to load system roots and no roots provided"<a name="troubleshooting-cache-image"></a>

**Issue:** When you try to run a build project, the build fails with the error: "RequestError: send request failed caused by: x509: failed to load system roots and no roots provided\."

 **Possible cause:** You configured caching as part of your build project and are using an older Docker image that includes an expired root certificate\. 

 **Recommended solution:** Update the Docker image that is being used in your AWS CodeBuild the project\. For more information, see [Docker Images Provided by CodeBuild](build-env-ref-available.md)\. 

## Error: "Unable to download certificate from S3\. AccessDenied"<a name="troubleshooting-certificate-in-S3"></a>

**Issue:** When you try to run a build project, the build fails with the error "Unable to download certificate from S3\. AccessDenied"\.

 **Possible causes:** 
+ You have chosen the wrong S3 bucket for your certificate\.
+ You have entered the wrong object key for your certificate\.

 **Recommended solutions:** 
+ Edit your project\. For **Bucket of certificate**, choose the S3 bucket where your SSL certificate is stored\.
+ Edit your project\. For **Object key of certificate**, type the name of your S3 object key\.

## Error: "Git Clone Failed: unable to access `'your-repository-URL'`: SSL certificate problem: self signed certificate"<a name="troubleshooting-self-signed-certificate"></a>

**Issue:** When you try to run a build project, the build fails with the error "Git Clone Failed: unable to access `'your-repository-URL'`: SSL certificate problem: self signed certificate\."

 **Possible cause:** Your source repository has a self\-signed certificate, but you have not chosen to install the certificate from your S3 bucket as part of your build project\. 

 **Recommended solutions:** 
+ Edit your project\. For **Certificate**, choose **Install certificate from S3**\. For **Bucket of certificate**, choose the S3 bucket where your SSL certificate is stored\. For **Object key of certificate**, type the name of your S3 object key\.
+ Edit your project\. Select **Insecure SSL** to ignore SSL warnings while connecting to your GitHub Enterprise project repository\.
**Note**  
We recommend that you use **Insecure SSL** for testing only\. It should not be used in a production environment\.

## Error: "The policy's default version was not created by enhanced zero click role creation or was not the most recent version created by enhanced zero click role creation\."<a name="enhanced-zero-click-role-creation"></a>

**Issue:** When you try to update a project in the console, the update failed with the error: "The policy's default version was not created by enhanced zero click role creation or was not the most recent version created by enhanced zero click role creation\."

 **Possible causes:** 
+ You have manually updated the policies attached to the target AWS CodeBuild service role\.
+ You have selected a previous version of a policy attached to the target CodeBuild service role\.

 **Recommended solutions:** 
+ Edit your CodeBuild project, and clear **Allow CodeBuild to modify this service role so it can be used with this build project**\. Manually update the target CodeBuild service role to have sufficient permissions\. For more information, see [Create a CodeBuild Service Role](setting-up.md#setting-up-service-role)\.
+ Edit your CodeBuild project, and select **Create a role**\.

## Error: "Build container found dead before completing the build\. Build container died because it was out of memory, or the Docker image is not supported\. ErrorCode: 500"<a name="windows-server-core-version"></a>

 **Issue:** When you try to use a Microsoft Windows or Linux container in AWS CodeBuild an error occurs during the PROVISIONING phase\. 

 **Possible causes:** 
+  The Container OS version is not supported by CodeBuild\. 
+  `HTTP_PROXY`, `HTTPS_PROXY`, or both are specified in the container\.

 **Recommended solutions:** 
+ For Microsoft Windows, use a Windows container with a Container OS that is version microsoft/windowsservercore:10\.0\.x\. For example, microsoft/windowsservercore:10\.0\.14393\.2125\.
+ For Linux, clear the `HTTP_PROXY` and `HTTPS_PROXY` settings in your Docker image, or specify the VPC configuration in you build project\.

## Cannot view build success or failure<a name="no-status-when-build-triggered"></a>

**Issue:** You cannot see the success or failure of a retried build\.

**Possible cause:** The option to report your build's status is not enabled\. 

**Recommended solutions:** Enable **Report build status** when you create or update a CodeBuild project\. This option tells CodeBuild to report back the status when you trigger a build\. For more information, see [reportBuildStatus](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_ProjectSource.html#CodeBuild-Type-ProjectSource-reportBuildStatus)\. 

## Cannot find and select the base image of the Windows Server Core 2016 platform<a name="windows-image-not-available"></a>

**Issue:** You cannot find or select the base image of the Windows Server Core 2016 platform\.

**Possible cause:** You are using a region that does not support this image\. 

**Recommended solutions:** Use one of the regions that supports the base image of the Windows Server Core 2016 platform:
+  US East \(N\. Virginia\) 
+  US East \(Ohio\) 
+  US East \(Ohio\) 
+  US West \(N\. California\) 

## RequestError timeout error when running CodeBuild in a proxy server<a name="code-request-timeout-error"></a>

 **Issue:** You receive an error similar to `RequestError: send request failed caused by: Post https://logs.<your-region>.amazonaws.com/: dial tcp 52.46.158.105:443: i/o timeout` from CloudWatch Logs\. 

 **Possible causes:** 
+ `ssl-bump` is not configured properly\. 
+ Your organization's security policy does not allow you to use `ssl_bump`\. 

**Recommended solutions:** 
+ Make sure `ssl-bump` is configured properly\. If you use Squid for your proxy server, see [ Configure Squid as an Explicit Proxy Server](use-proxy-server.md#use-proxy-server-explicit-squid-configure)\. 
+ Use private endpoints for Amazon S3 and CloudWatch Logs by doing the following: 

  1.  In your private subnet routing table, remove the rule you added that routes traffic destined for the internet to your proxy server\. For information, see [Creating a Subnet in Your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/working-with-vpcs.html#AddaSubnet)\. 

  1.  Create a private Amazon S3 endpoint and CloudWatch Logs endpoint and associate them with the private subnet of your Amazon VPC\. For information, see [VPC Endpoint Services \(AWS PrivateLink\)](https://docs.aws.amazon.com/vpc/latest/userguide/endpoint-service.html)\. 

  1.  Confirm **Enable Private DNS Name** in your Amazon VPC is selected\. For more information, see [Creating an Interface Endpoint ](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#create-interface-endpoint)\. 

## Error: "QUEUED: INSUFFICIENT\_SUBNET" when a build in a build queue fails<a name="queued-insufficient-subnet-error"></a>

**Issue:** A build in a build queue fails with an error similar to `QUEUED: INSUFFICIENT_SUBNET`\.

**Possible causes:** The IPv4 CIDR block specified for your VPC uses a reserved IP address\. The first four IP addresses and the last IP address in each subnet CIDR block are not available for you to use and cannot be assigned to an instance\. For example, in a subnet with CIDR block `10.0.0.0/24`, the following five IP addresses are reserved: 
+  `10.0.0.0:` Network address\. 
+  `10.0.0.1`: Reserved by AWS for the VPC router\. 
+  `10.0.0.2`: Reserved by AWS\. The IP address of the DNS server is always the base of the VPC network range plus two; however, we also reserve the base of each subnet range plus two\. For VPCs with multiple CIDR blocks, the IP address of the DNS server is located in the primary CIDR\. For more information, see [Amazon DNS Server](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_DHCP_Options.html#AmazonDNS)\.
+  `10.0.0.3`: Reserved by AWS for future use\. 
+  `10.0.0.255`: Network broadcast address\. We do not support broadcast in a VPC, therefore we reserve this address\. 

**Recommended solutions:** Check if your VPC uses a reserved IP address\. Replace any reserved IP address with an IP address that is not reserved\. For more information, see [VPC and Subnet Sizing](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html#VPC_Sizing)\. 