# Create a Build Project in AWS CodeBuild<a name="create-project"></a>

You can use the AWS CodeBuild console, AWS CLI, or AWS SDKs to create a build project\.

**Topics**
+ [Prerequisites](#create-project-prerequisites)
+ [Create a Build Project \(Console\)](#create-project-console)
+ [Create a Build Project \(AWS CLI\)](#create-project-cli)
+ [Create a Build Project \(AWS SDKs\)](#create-project-sdks)
+ [Create a Build Project \(AWS CloudFormation\)](#create-project-cloud-formation)

## Prerequisites<a name="create-project-prerequisites"></a>

Answer the questions in [Plan a Build](planning.md)\.

## Create a Build Project \(Console\)<a name="create-project-console"></a>

1. Open the AWS CodeBuild console at [https://console\.aws\.amazon\.com/codebuild/](https://console.aws.amazon.com/codebuild/)\.

1. If a welcome page is displayed, choose **Get started** 

   If a welcome page is not displayed, on the navigation pane, choose **Build projects**, and then choose **Create project**\.

1. On the **Configure your project** page, for **Project name**, enter a name for this build project\. Build project names must be unique across each AWS account\.

1. \(Optional\) Choose **Add description**, and then enter a description\.

1.  **Source: What to build**, for **Source provider**, choose the source code provider type\. Use the following table to make selections appropriate for your source provider:  
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codebuild/latest/userguide/create-project.html)

1. For each secondary source you want:

   1.  Choose **Add source**\. 

   1.  For **Source identifier**,enter a value that is fewer than 128 characters and contains only alphanumeric characters and underscores\.

   1.  For **Source provider**, choose the source code provider type\. Use the table in step 5 to make selections appopropriate for your secondary source provider\. 

   1.  Choose **Save source**\. 

1. In **Environment: How to build**:

   For **Environment image**, do one of the following:
   + To use a Docker image managed by AWS CodeBuild, choose **Use an image managed by AWS CodeBuild**, and then make selections from **Operating system**, **Runtime**, and **Version**\.
   + To use another Docker image, choose **Specify a Docker image**\. For **Custom image type**, choose **Other** or **Amazon ECR**\. If you choose **Other**, then for **Custom image ID**, enter the name and tag of the Docker image in Docker Hub\. Use this format: `repository-name/image-name:image-tag`\. If you choose **Amazon ECR**, then use **Amazon ECR repository** and **Amazon ECR image** to choose the Docker image in your AWS account\. 

   For **Build specification**, do one of the following:
   + If your source code includes a buildspec file, choose **Use the buildspec\.yml in the source code root directory**\.
   + If your source code does not include a buildspec file, or if you want to run build commands different from the ones specified for the `build` phase in the `buildspec.yml` file in the source code's root directory, choose **Insert build commands**\. For **Build command**, enter the commands you want to run in the `build` phase\. For multiple commands, separate each command by `&&` \(for example, `mvn test && mvn package`\)\. To run commands in other phases, or if you have a long list of commands for the `build` phase, add a `buildspec.yml` file to the source code root directory, add the commands to the file, and then choose **Use the buildspec\.yml in the source code root directory**\.

   For more information, see the [Build Spec Reference](build-spec-ref.md)\.

1. In **Artifacts: Where to put the artifacts from this build project**, for **Artifacts type**, do one of the following:
   + If you do not want to create any build output artifacts, choose **No artifacts**\. You might want to do this if you're only running build tests or you want to push a Docker image to an Amazon ECR repository\.
   + To store the build output in an Amazon S3 bucket, choose **Amazon S3**, and then do the following:
     + If you want to use your project name for the build output ZIP file or folder, leave **Name** blank\. Otherwise, type the name in **Name**\. \(If you want to output a ZIP file, and you want the ZIP file to have a file extension, be sure to include it after the ZIP file name\.\)
     + Select **Use the name specified in the buildspec file** if you want a name specified in the buildspec file to override any name that is specified in the console\. The name in a buildspec file is calculated at build time and uses the Shell command language\. For example, you can append a date and time to your artifact name so that it is always unique\. Unique artifact names prevent artifacts from being overwritten\. For more information, see [Build Spec Syntax](build-spec-ref.md#build-spec-ref-syntax)\.
     + For **Bucket name**, choose the name of the output bucket\.
     + If you chose **Insert build commands** earlier in this procedure, then for **Output files**, type the locations of the files from the build that you want to put into the build output ZIP file or folder\. For multiple locations, separate each location with a comma \(for example, `appspec.yml, target/my-app.jar`\)\. For more information, see the description of `files` in [Build Spec Syntax](build-spec-ref.md#build-spec-ref-syntax)\.
     + If you do not want your build artifacts encrypted, select **Disable artifacts encryption**\.

1. For each secondary set of artifacts you want:

   1. For **Artifact identifier**, enter a value that is fewer than 128 characters and contains only alphanumeric characters and underscores\.

   1. Choose **Add artifact**\.

   1. Follow step 8 to configure your secondary artifacts\.

   1. Choose **Save artifact**\.

1. In **Cache**, do one of the following:
   + If you do not want to use a cache, choose **No cache**\.
   + To use a cache, choose **Amazon S3**, and then do the following:
     + For **Bucket**, choose the name of the Amazon S3 bucket where the cache is stored\.
     + \(Optional\) For **Path prefix**, enter an Amazon S3 path prefix\. The **Path prefix** value is similar to a directory name\. It makes it possible for you to store the cache under the same directory in a bucket\. 
**Important**  
Do not append "/" to the end of the path prefix\.

   Using a cache saves considerable build time because reusable pieces of the build environment are stored in the cache and used across builds\. For information about specifying a cache in the buildspec file, see [Build Spec Syntax](build-spec-ref.md#build-spec-ref-syntax)\. 

1. In **Logs**, choose the logs you want to create\. You can create Amazon CloudWatch Logs, Amazon S3 logs, or both\. 

   If you want Amazon CloudWatch Logs logs:
   +  Select **CloudWatch logs**\. 
   +  In **Group name**, enter the name of your Amazon CloudWatch Logs log group\. 
   +  In **Stream name**, enter your Amazon CloudWatch Logs log stream name\. 

    If you want Amazon S3 logs: 
   +  Select **S3 logs**\. 
   +  From **Bucket**, choose the name of the S3 bucket for your logs\. 
   +  In **Path prefix**, enter the prefix for your logs\. 

1. In **Service role**, do one of the following:
   + If you do not have an AWS CodeBuild service role, choose **Create a service role in your account**\. In **Role name**, accept the default name or type your own\.
   + If you have an AWS CodeBuild service role, choose **Choose an service existing role from your account**\. In **Role name**, choose the service role\.
**Note**  
When you use the console to create or update a build project, you can create an AWS CodeBuild service role at the same time\. By default, the role works with that build project only\. If you use the console to associate this service role with another build project, the role is updated to work with the other build project\. A service role can work with up to 10 build projects\.

1. In **VPC**, do one of the following:
   + If you are not using a VPC for your project, choose **No VPC**\.
   + If you want AWS CodeBuild to work with your VPC:
     + For **VPC**, choose the VPC ID that AWS CodeBuild uses\.
     + For **Subnets**, choose the subnets that include resources that AWS CodeBuild uses\.
     + For **Security Groups**, choose the security groups that AWS CodeBuild uses to allow access to resources in the VPCs\.

   For more information, see [Use AWS CodeBuild with Amazon Virtual Private Cloud](vpc-support.md)\.

1. Expand **Show advanced settings**\.
**Note**  
If you arrived at this page by choosing **Get started** from a welcome page, then the **Show advanced settings** section is not displayed\. Skip to step 20 of this procedure\. For information about changing default settings, see [Change a Build Project's Settings \(Console\)](change-project.md#change-project-console)\.

1. \(Optional\) For **Timeout**, specify a value between 5 minutes and 480 minutes \(8 hours\) after which AWS CodeBuild stops the build if it is not complete\. If **hours** and **minutes** are left blank, the default value of 60 minutes is used\.

1. \(Optional\) For **Encryption key**, do one of the following:
   + To use the AWS\-managed customer master key \(CMK\) for Amazon S3 in your account to encrypt the build output artifacts, leave **Encryption key** blank\. This is the default\.
   + To use a customer\-managed CMK to encrypt the build output artifacts, in **Encryption key**, enter the ARN of the CMK\. Use the format `arn:aws:kms:region-ID:account-ID:key/key-ID`\.

1. \(Optional\) Select **Privileged** only if you plan to use this build project to build Docker images, and the build environment image you chose is not provided by AWS CodeBuild with Docker support\. Otherwise, all associated builds that attempt to interact with the Docker daemon fail\. You must also start the Docker daemon so that your builds can interact with it\. One way to do this is to initialize the Docker daemon in the `install` phase of your build spec by running the following build commands\. Do not run these commands if you chose a build environment image provided by AWS CodeBuild with Docker support\.

   ```
   - nohup /usr/local/bin/dockerd --host=unix:///var/run/docker.sock --host=tcp://127.0.0.1:2375 --storage-driver=overlay&
   - timeout -t 15 sh -c "until docker info; do echo .; sleep 1; done"
   ```

1. \(Optional\) If you chose **Amazon S3** for **Artifacts type** earlier in this procedure, then for **Artifacts packaging**, do one of the following:
   + To have AWS CodeBuild create a ZIP file that contains the build output, choose **Zip**\.
   + To have AWS CodeBuild create a folder that contains the build output, choose **None**\. \(This is the default\.\)

1. For **Compute type**, choose one of the available options\.

1. For **Environment variables**, type the name, value, and type of each environment variable for builds to use\. Use **Add row** to add an environment variable\.
**Note**  
AWS CodeBuild sets the environment variable for your AWS region automatically\. If you do not add them to your buildspec\.yml, then the following environment variables must be set:  
AWS\_ACCOUNT\_ID
IMAGE\_REPO\_NAME
IMAGE\_TAG

   Others can see environment variables by using the AWS CodeBuild console and the AWS CLI\. If you have no concerns about the visibility of your environment variable, set the **Name** and **Value** fields, and then set **Type** to **Plaintext**\.

   We recommend that you store an environment variable with a sensitive value, such as an AWS access key ID, an AWS secret access key, or a password as a parameter in Amazon EC2 Systems Manager Parameter Store\. For **Type**, choose **Parameter Store**\. For **Name**, type an identifier for AWS CodeBuild to reference\. For **Value**, type the parameter's name as stored in Amazon EC2 Systems Manager Parameter Store\. Using a parameter named `/CodeBuild/dockerLoginPassword` as an example, for **Type**, choose **Parameter Store**\. For **Name**, type `LOGIN_PASSWORD`\. For **Value**, type `/CodeBuild/dockerLoginPassword`\. 
**Important**  
We recommend that you store parameters in Amazon EC2 Systems Manager Parameter Store with parameter names that start with `/CodeBuild/` \(for example, `/CodeBuild/dockerLoginPassword`\)\. You can use the AWS CodeBuild console to create a parameter in Amazon EC2 Systems Manager\. Choose **Create a parameter**, and then follow the instructions in the dialog box\. \(In that dialog box, for **KMS key**, you can optionally specify the ARN of an AWS KMS key in your account\. Amazon EC2 Systems Manager uses this key to encrypt the parameter's value during storage and decrypt during retrieval\.\) If you use the AWS CodeBuild console to create a parameter, the console starts the parameter name with `/CodeBuild/` as it is being stored\. For more information, see [Systems Manager Parameter Store](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-paramstore.html) and [Systems Manager Parameter Store Console Walkthrough](https://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-paramstore-walk.html#sysman-paramstore-console) in the *Amazon EC2 Systems Manager User Guide*\.  
If your build project refers to parameters stored in Amazon EC2 Systems Manager Parameter Store, the build project's service role must allow the `ssm:GetParameters` action\. If you chose **Create a service role in your account** earlier, then AWS CodeBuild includes this action in the default service role for your build project automatically\. However, if you chose **Choose an existing service role from your account**, then you must include this action to your service role separately\.  
If your build project refers to parameters stored in Amazon EC2 Systems Manager Parameter Store with parameter names that do not start with `/CodeBuild/`, and you chose **Create a service role in your account**, then you must update that service role to allow access to parameter names that do not start with `/CodeBuild/`\. This is because that service role allows access only to parameter names that start with `/CodeBuild/`\.  
Environment variables you set replace existing environment variables\. For example, if the Docker image already contains an environment variable named `MY_VAR` with a value of `my_value`, and you set an environment variable named `MY_VAR` with a value of `other_value`, then `my_value` is replaced by `other_value`\. Similarly, if the Docker image already contains an environment variable named `PATH` with a value of `/usr/local/sbin:/usr/local/bin`, and you set an environment variable named `PATH` with a value of `$PATH:/usr/share/ant/bin`, then `/usr/local/sbin:/usr/local/bin` is replaced by the literal value `$PATH:/usr/share/ant/bin`\.  
Do not set any environment variable with a name that begins with `CODEBUILD_`\. This prefix is reserved for internal use\.  
If an environment variable with the same name is defined in multiple places, the value is determined as follows:  
The value in the start build operation call takes highest precedence\.
The value in the build project definition takes next precedence\.
The value in the build spec declaration takes lowest precedence\.

1. \(Optional\) For **Tags**, type the name and value of any tags that you want supporting AWS services to use\. Use **Add row** to add a tag\. You can add up to 50 tags\.

1. Choose **Continue**\.

1. On the **Review** page, do one of the following:
   + To run a build, choose **Save and build**\. 
   + To finish creating the build project without running a build, choose **Save**\. 

## Create a Build Project \(AWS CLI\)<a name="create-project-cli"></a>

For information about using the AWS CLI with AWS CodeBuild, see the [Command Line Reference](cmd-ref.md)\.

1. Run the `create-project` command:

   ```
   aws codebuild create-project --generate-cli-skeleton
   ```

   JSON\-formatted data appears in the output\. Copy the data to a file \(for example, `create-project.json`\) in a location on the local computer or instance where the AWS CLI is installed\. Modify the copied data as follows, and save your results\.

   ```
   {
     "name": "project-name",
     "description": "description",
     "source": {
       "type": "source-type",
       "location": "source-location",
       "gitCloneDepth": "gitCloneDepth",
       "buildspec": "buildspec",
       "InsecureSsl": "InsecureSsl",
       "reportBuildStatus": reportBuildStatus", 
       "auth": {
         "type": "auth-type",
         "resource": "resource"
       }
     },
     "artifacts": {
       "type": "artifacts-type",
       "location": "artifacts-location",
       "path": "path",
       "namespaceType": "namespaceType",
       "name": "artifacts-name",
       "overrideArtifactName": "override-artifact-name",
       "packaging": "packaging"    
     },
     "cache": {
       "type": "cache-type",
       "location": "cache-location"
     },
     "logsConfig": {
       "cloudWatchLogs": {
         "status": "cloudwatch-logs-status",
         "groupName": "group-name",
         "streamName": "stream-name"      
       }
       "s3Logs": {
         "status": "s3-logs-status",
         "location": "s3-logs-location"
       }
     }
     "secondaryArtifacts": [
       {
           "type": "artifacts-type",
           "location": "artifacts-location",
           "path": "path",
           "namespaceType": "namespaceType",
           "name": "artifacts-name",
           "packaging": "packaging",
           "artifactIdentifier": "artifact-identifier"
       }
     ]
     ,
     "secondarySources": [
       {
           "type": "source-type",
           "location": "source-location",
           "gitCloneDepth": "gitCloneDepth",
           "buildspec": "buildspec",
           "InsecureSsl": "InsecureSsl",
           "reportBuildStatus": "reportBuildStatus", 
           "auth": {
             "type": "auth-type",
             "resource": "resource"
           },
           "sourceIdentifier": "source-identifier"
       }
     ],
     "serviceRole": "serviceRole",
     "vpcConfig": {
       "securityGroupIds": [
            "security-group-id"
       ],
       "subnets": [
            "subnet-id"
       ],
       "vpcId": "vpc-id"
     },
     "timeoutInMinutes": timeoutInMinutes,
     "encryptionKey": "encryptionKey",
     "tags": [
       {
         "key": "tag-key",
         "value": "tag-value"
       }
     ],
     "environment": {
       "type": "environment-type",
       "image": "image",
       "computeType": "computeType",
       "certificate": "certificate",
       "environmentVariables": [
         {
           "name": "environmentVariable-name",
           "value": "environmentVariable-value",
           "type": "environmentVariable-type"
         }
       ],
       "privilegedMode": "privilegedMode"
     },
     "badgeEnabled": "badgeEnabled"
   }
   ```

   Replace the following:
   + *project\-name*: Required value\. The name for this build project\. This name must be unique across all of the build projects in your AWS account\.
   + *description*: Optional value\. The description for this build project\.
   + <a name="cli-sources"></a>For the required `source` object, information about this build project's source code settings\. After you add a `source` object, you can add up to 12 more sources using the [CodeBuild secondarySources object](#cli-secondary-sources)\. These settings include the following:
     + <a name="cli-sources-type"></a>*source\-type*: Required value\. The type of repository that contains the source code to build\. Valid values include `CODECOMMIT`, `CODEPIPELINE`, `GITHUB`, `GITHUB_ENTERPRISE`, `BITBUCKET`, and `S3`\.
     + <a name="cli-sources-location"></a>*source\-location*: Required value \(unless you set *source\-type* to `CODEPIPELINE`\)\. The location of the source code for the specified repository type\.
       + For AWS CodeCommit, the HTTPS clone URL to the repository that contains the source code and the build spec \(for example, `https://git-codecommit.region-id.amazonaws.com/v1/repos/repo-name`\)\.
       + For Amazon S3, the build input bucket name, followed by a forward slash \(`/`\), followed by the name of the ZIP file that contains the source code and the build spec \(for example, `bucket-name/object-name.zip`\)\. This assumes that the ZIP file is in the root of the build input bucket\. \(If the ZIP file is in a folder inside of the bucket, use `bucket-name/path/to/object-name.zip` instead\.\)
       + For GitHub, the HTTPS clone URL to the repository that contains the source code and the build spec\. You must connect your AWS account to your GitHub account\. To do this, use the AWS CodeBuild console to create a build project\. When you use the console to connect \(or reconnect\) with GitHub, on the GitHub **Authorize application** page, for **Organization access**, choose **Request access** next to each repository you want AWS CodeBuild to be able to access\. Choose **Authorize application**\. \(After you have connected to your GitHub account, you do not need to finish creating the build project\. You can close the AWS CodeBuild console\.\) To instruct AWS CodeBuild to use this connection, in the `source` object, set the `auth` object's `type` value to `OAUTH`\.
       + For GitHub Enterprise, the HTTP or HTTPS clone URL to the repository that contains the source code and the buildspec\. You must also connect your AWS account to your GitHub Enterprise account\. To do this, use the AWS CodeBuild console to create a build project\.

         First, create a personal access token in GitHub Enterprise\. Copy this token to your clipboard so you can use it when you create your AWS CodeBuild project\. For more information, see [Creating a Personal Access Token in GitHub Enterprise](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/) on the GitHub Help website\. When you use the console to create your AWS CodeBuild project, in **Source: What to build**, for **Source provider**, choose **GitHub Enterprise**\. For **Personal Access Token**, paste the token that was copied to your clipboard\. Choose **Save Token**\. Your AWS CodeBuild account is now connected to your GitHub Enterprise account\.
       + For Bitbucket, the HTTPS clone URL to the repository that contains the source code and the build spec\. You must also connect your AWS account to your Bitbucket account\. To do this, use the AWS CodeBuild console to create a build project\. When you use the console to connect \(or reconnect\) with Bitbucket, on the Bitbucket **Confirm access to your account** page, choose **Grant access**\. \(After you have connected to your Bitbucket account, you do not need to finish creating the build project\. You can close the AWS CodeBuild console\.\) To instruct AWS CodeBuild to use this connection, in the `source` object, set the `auth` object's `type` value to `OAUTH`\.
       + For AWS CodePipeline, do not specify a `location` value for `source`\. It is ignored by AWS CodePipeline because when you create a pipeline in AWS CodePipeline, you specify the source code location in the Source stage of the pipeline\.
     + <a name="cli-sources-gitclonedepth"></a>*gitCloneDepth*: Optional value\. The depth of history to download\. Minimum value is 0\. If this value is 0, greater than 25, or not provided, then the full history is downloaded with each build project\. If your source type is Amazon S3, this value is not supported\.
     + <a name="cli-sources-buildspec"></a>*buildspec*: Optional value\. The build specification definition or file to use\. If this value is set, it can be either an inline buildspec definition or the path to an alternate buildspec file relative to the value of the built\-in `CODEBUILD_SRC_DIR` environment variable\. If this value is not provided or is set to an empty string, then the source code must contain a `buildspec.yml` file in its root directory\. For more information, see [Build Spec File Name and Storage Location](build-spec-ref.md#build-spec-ref-name-storage)\.
     + <a name="cli-sources-auth"></a>*auth*: This object is used by the AWS CodeBuild console only\. Do not specify values for *auth\-type* \(unless *source\-type* is set to `GITHUB`\) or *resource*\.
     + <a name="cli-sources-reportbuildstatus"></a>*reportBuildStatus*: Optional value\. Specifies whether to send your source provider the status of a build's start and completion\. If you set this with a source provider other than GitHub or Bitbucket, an invalidInputException is thrown\.
     + <a name="cli-sources-insecuressl"></a>*InsecureSsl*: Optional value\. Used with GitHub Enterprise only\. Set this value to `true` to ignore SSL warnings while connecting to your GitHub Enterprise project repository\. The default value is `false`\. *InsecureSsl* should be used for testing purposes only\. It should not be used in a production environment\.
   + <a name="cli-artifacts"></a>For the required `artifacts` object, information about this build project's output artifact settings\. After you add an `artifacts` object, you can add up to 12 more artifacts using the [CodeBuild secondaryArtifacts object](#cli-secondary-artifacts)\. These settings include the following: <a name="cli-artifacts-type"></a>
     + *artifacts\-type*: Required value\. The type of build output artifact\. Valid values include `CODEPIPELINE`, `NO_ARTIFACTS`, and `S3`\.
     + <a name="cli-artifacts-location"></a>*artifacts\-location*: Required value \(unless you set *artifacts\-type* to `CODEPIPELINE` or `NO_ARTIFACTS`\)\. The location of the build output artifact:
       + If you specified `CODEPIPELINE` for *artifacts\-type*, do not specify a `location` for `artifacts`\.
       + If you specified `NO_ARTIFACTS` for *artifacts\-type*, do not specify a `location` for `artifacts`\.
       + If you specified `S3` for *artifacts\-type*, then this is the name of the output bucket you created or identified in the prerequisites\. 
     + <a name="cli-artifacts-path"></a>*path*: Optional value\. The path and name of the build output ZIP file or folder:
       + If you specified `CODEPIPELINE` for *artifacts\-type*, then do not specify a `path` for `artifacts`\.
       + If you specified `NO_ARTIFACTS` for *artifacts\-type*, do not specify a `path` for `artifacts`\.
       + If you specified `S3` for *artifacts\-type*, then this is the path inside of *artifacts\-location* to the build output ZIP file or folder\. If you do not specify a value for *path*, then AWS CodeBuild uses *namespaceType* \(if specified\) and *artifacts\-name* to determine the path and name of the build output ZIP file or folder\. For example, if you specify `MyPath` for *path* and `MyArtifact.zip` for *artifacts\-name*, then the path and name would be `MyPath/MyArtifact.zip`\.
     + <a name="cli-artifacts-namespacetype"></a>*namespaceType*: Optional value\. The path and name of the build output ZIP file or folder:
       + If you specified `CODEPIPELINE` for *artifacts\-type*, do not specify a `namespaceType` for `artifacts`\. 
       + If you specified `NO_ARTIFACTS` for *artifacts\-type*, do not specify a `namespaceType` for `artifacts`\.
       + If you specified `S3` for *artifacts\-type*, valid values include `BUILD_ID` and `NONE`\. Use `BUILD_ID` to insert the build ID into the path of the build output ZIP file or folder\. Otherwise, use `NONE`\. If you do not specify a value for *namespaceType*, AWS CodeBuild uses *path* \(if specified\) and *artifacts\-name* to determine the path and name of the build output ZIP file or folder\. For example, if you specify `MyPath` for *path*, `BUILD_ID` for *namespaceType*, and `MyArtifact.zip` for *artifacts\-name*, then the path and name would be `MyPath/build-ID/MyArtifact.zip`\.
     + <a name="cli-artifacts-name"></a>*artifacts\-name*: Required value \(unless you set *artifacts\-type* to `CODEPIPELINE` or `NO_ARTIFACTS`\)\. The path and name of the build output ZIP file or folder:
       + If you specified `CODEPIPELINE` for *artifacts\-type*, do not specify a `name` for `artifacts`\.
       + If you specified `NO_ARTIFACTS` for *artifacts\-type*, do not specify a `name` for `artifacts`\.
       + If you specified `S3` for *artifacts\-type*, then this is the name of the build output ZIP file or folder inside of *artifacts\-location*\. For example, if you specify `MyPath` for *path* and `MyArtifact.zip` for *artifacts\-name*, then the path and name would be `MyPath/MyArtifact.zip`\.
     + *override\-artifact\-name*: Optional boolean value\. If set to `true`, the name specified in the `artifacts` block of the buildspec file overrides *artifacts\-name*\. For more information, see [Build Specification Reference for AWS CodeBuild](build-spec-ref.md)\.
     + <a name="cli-artifacts-packaging"></a>*packaging*: Optional value\. The type of build output artifact to create:
       + If you specified `CODEPIPELINE` for *artifacts\-type*, do not specify a `packaging` for `artifacts`\.
       + If you specified `NO_ARTIFACTS` for *artifacts\-type*, do not specify a `packaging` for `artifacts`\.
       + If you specified `S3` for *artifacts\-type*, valid values include `ZIP` and `NONE`\. To create a ZIP file that contains the build output, use `ZIP`\. To create a folder that contains the build output, use `NONE`\. The default value is `NONE`\.
   + For the required `cache` object, information about this build project's cache settings\. These settings include the following:
     + *cache\-type*: Required value\. Valid values are `S3` or `NO_CACHE`\.
     + *cache\-location*: Required value unless you set *CacheType* to `NONE`\. If you specified S3 for *CacheType*, then this is the ARN of the S3 bucket and the path prefix\. For example, if your Amazon S3 bucket name is `my-bucket`, and your path prefix is `build-cache`, then acceptable formats for your *CacheLocation* are `my-bucket/build-cache` or `arn:aws:s3:::my-bucket/build-cache`\.
   + For the `logsConfig` object, information about where this build's logs are located:
     +  *cloudwatch\-logs\-status*: Required value\. Valid values are `ENABLED` or `DISABLED`\. If its value is `ENABLED` then the following values are required\. For more information, see [Working with Log Groups and Log Streams](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/Working-with-log-groups-and-streams.html)\. 
     +  *group\-name*: The name of the CloudWatch Logs group\. 
     +  *stream\-name*: The name of the CloudWatch Logs stream\. 
     +  *s3\-logs\-status*: Required value\. Valid values are `ENABLED` or `DISABLED`\. 
     +  *s3\-logs\-location*: Required if *s3\-logs\-status* is `ENABLED`\. This is the ARN of an S3 bucket and the path prefix\. For example, if your Amazon S3 bucket name is `my-bucket`, and your path prefix is `build-log`, then acceptable formats for your *s3\-logs\-location* are `my-bucket/build-log` or `arn:aws:s3:::my-bucket/build-log`\. 
   + <a name="cli-secondary-artifacts"></a>For the optional `secondaryArtifacts` object, information about the settings of a secondary artifiact for a build project\. You can add up to 12 secondary artifacts\. The `secondaryArtifacts` uses many of the same settings used by the [CodeBuild artifacts object](#cli-artifacts) object\. The settings are: 
     + *artifacts\-type*: Required value\. This setting is also used by the `artifacts` object\. See [CodeBuild artifact object's type property](#cli-artifacts-type)\.
     + *artifacts\-location*: Required value\. This setting is also used by the `artifacts` object\. See [CodeBuild artifact object's location property](#cli-artifacts-location)\.
     + *path*: Optional value\. This setting is also used by the `artifacts` object\. See [CodeBuild artifact object's path property](#cli-artifacts-path)\.
     + *namespaceType*: Optional value\. This setting is also used by the `artifacts` object\. See [CodeBuild artifact object's namespaceType property](#cli-artifacts-namespacetype)\.
     + *artifacts\-name*: Required value\. This setting is also used by the `artifacts` object\. See [CodeBuild artifact object's name property](#cli-artifacts-name)\.
     + *packaging*: Optional value\. This setting is also used by the `artifacts` object\. See [CodeBuild artifact object's packaging property](#cli-artifacts-packaging)\.
     + *artifact\-identifier*: Required value\. A unique string identifier for a secondary artifact\.
   + <a name="cli-secondary-sources"></a>For the optional `secondarySources` object, information about the settings of a secondary source for a build project\. You can add up to 12 `secondarySources`\. The `secondarySources` object uses many of the same settings used by the [CodeBuild source object](#cli-sources)\. They include the following:
     + *source\-type*: Required value\. This setting is also used by the `sources` object\. See [CodeBuild source object's type property](#cli-sources-type)\.
     + *source\-location*: Required value\. This setting is also used by the `sources` object\. See [CodeBuild source object's location property](#cli-sources-location)\.
     + *gitCloneDepth*: Optional value\. This setting is also used by the `sources` object\. See [CodeBuild source object's gitCloneDepth property](#cli-sources-gitclonedepth)\.
     + *buildspec*: Optional value\. This setting is also used by the `sources` object\. See [CodeBuild source object's buildspec property](#cli-sources-buildspec)\.
     + *auth*: This setting is also used by the `sources` object\. See [CodeBuild source object's auth property](#cli-sources-auth)\.
     + *reportBuildStatus*: Optional value\. This setting is also used by the `sources` object\. See [CodeBuild source object's reportBuildStatus property](#cli-sources-reportbuildstatus)\.
     + *InsecureSsl*: Optional value\. This setting is also used by the `sources` object\. See [CodeBuild source object's insecureSsl property](#cli-sources-insecuressl)\.
     + *source\-identifier*: Required value\. A unique string identifier for a secondary source\.
   + *serviceRole*: Required value\. The ARN of the service role AWS CodeBuild uses to interact with services on behalf of the IAM user \(for example, `arn:aws:iam::account-id:role/role-name`\)\.
   + For the optional *vpcConfig* object, information about your VPC configuration\. These settings include: 
     + *vpcId*: Required value\. The VPC ID that AWS CodeBuild uses\. Run this command to get a list of all Amazon VPC IDs in your region:

       ```
       aws ec2 describe-vpcs
       ```
     + *subnets*: Required value\. The subnet IDs that include resources used by AWS CodeBuild\. Run this command to get these IDs:

       ```
       aws ec2 describe-subnets --filters "Name=vpc-id,Values=<vpc-id>" --region us-east-1
       ```
**Note**  
If you are using a region other than us\-east\-1, be sure to use it when you run the command\.
     + *securityGroupIds*: Required value\. The security group IDs used by AWS CodeBuild to allow access to resources in the VPCs\. Run this command to get these IDs:

       ```
       aws ec2 describe-security-groups --filters "Name=vpc-id,Values=<vpc-id>" --region us-east-1
       ```
**Note**  
If you are using a region other than us\-east\-1, be sure to use it when you run the command\.
   + For the required `environment` object, information about this project's build environment settings\. These settings include: 
     + *environment\-type*: Required value\. The type of build environment\. Valid values are `LINUX_CONTAINER` and `WINDOWS_CONTAINER`\.
     + *image*: Required value\. The Docker image identifier used by this build environment\. Typically, this identifier is expressed as *image\-name*:*tag*\. For example, in the Docker repository that AWS CodeBuild uses to manage its Docker images, this could be `aws/codebuild/java:openjdk-8`\. In Docker Hub, `maven:3.3.9-jdk-8`\. In Amazon ECR, `account-id.dkr.ecr.region-id.amazonaws.com/your-Amazon-ECR-repo-name:tag`\. For more information, see [Docker Images Provided by AWS CodeBuild](build-env-ref-available.md)\. 
     + *computeType*: Required value\. A category corresponding to the number of CPU cores and memory used by this build environment\. Allowed values include `BUILD_GENERAL1_SMALL`, `BUILD_GENERAL1_MEDIUM`, and `BUILD_GENERAL1_LARGE`\.
     + *certificate*: Optional value\. The ARN of the S3 bucket, path prefix and object key that contains the PEM\-encoded certificate\. The object key can be either just the \.pem file or a \.zip file containing the pem\-encoded certificate\. For example, if your Amazon S3 bucket name is my\-bucket, your path prefix is cert, and your object key name is certificate\.pem, then acceptable formats for your *certificate* are my\-bucket/cert/certificate\.pem or arn:aws:s3:::my\-bucket/cert/certificate\.pem\.
     + For the optional `environmentVariables` array, information about any environment variables you want to specify for this build environment\. Each environment variable is expressed as an object that contains a `name`, `value`, and `type` of *environmentVariable\-name*, *environmentVariable\-value*, and *environmentVariable\-type*\. 

       Others can see an environment variable by using the AWS CodeBuild console and the AWS CLI\. If you have no concerns about the visibility of your environment variable, set *environmentVariable\-name* and *environmentVariable\-value*, and then set *environmentVariable\-type* to `PLAINTEXT`\.

       We recommend you store an environment variable with a sensitive value, such as an AWS access key ID, an AWS secret access key, or a password as a parameter in Amazon EC2 Systems Manager Parameter Store\. For *environmentVariable\-name*, for that stored parameter, set an identifier for AWS CodeBuild to reference\. For *environmentVariable\-value*, set the parameter's name as stored in Amazon EC2 Systems Manager Parameter Store\. Set *environmentVariable\-type* to `PARAMETER_STORE`\. Using a parameter named `/CodeBuild/dockerLoginPassword` as an example, set *environmentVariable\-name* to `LOGIN_PASSWORD`\. Set *environmentVariable\-value* to `/CodeBuild/dockerLoginPassword`\. Set *environmentVariable\-type* to `PARAMETER_STORE`\. 
**Important**  
If your build project refers to parameters stored in Amazon EC2 Systems Manager Parameter Store, the build project's service role must allow the `ssm:GetParameters` action\. If you chose **Create a service role in your account** earlier, then AWS CodeBuild includes this action in the default service role for your build project automatically\. However, if you chose **Choose an existing service role from your account**, then you must include this action to your service role separately\.  
If your build project refers to parameters stored in Amazon EC2 Systems Manager Parameter Store with parameter names that do not start with `/CodeBuild/`, and you chose **Create a service role in your account**, then you must update that service role to allow access to parameter names that do not start with `/CodeBuild/`\. This is because that service role allows access only to parameter names that start with `/CodeBuild/`\.  
Any environment variables you set replace existing environment variables\. For example, if the Docker image already contains an environment variable named `MY_VAR` with a value of `my_value`, and you set an environment variable named `MY_VAR` with a value of `other_value`, then `my_value` is replaced by `other_value`\. Similarly, if the Docker image already contains an environment variable named `PATH` with a value of `/usr/local/sbin:/usr/local/bin`, and you set an environment variable named `PATH` with a value of `$PATH:/usr/share/ant/bin`, then `/usr/local/sbin:/usr/local/bin` is replaced by the literal value `$PATH:/usr/share/ant/bin`\.  
Do not set any environment variable with a name that begins with `CODEBUILD_`\. This prefix is reserved for internal use\.  
If an environment variable with the same name is defined in multiple places, the value is determined as follows:  
The value in the start build operation call takes highest precedence\.
The value in the build project definition takes next precedence\.
The value in the build spec declaration takes lowest precedence\.
     + You must specify *privilegedMode* with a value of `true` only if you plan to use this build project to build Docker images, and the build environment image you specified is not provided by AWS CodeBuild with Docker support\. Otherwise, all associated builds that attempt to interact with the Docker daemon fail\. You must also start the Docker daemon so that your builds can interact with it\. One way to do this is to initialize the Docker daemon in the `install` phase of your build spec by running the following build commands\. Do not run these commands if you specified a build environment image provided by AWS CodeBuild with Docker support\.

       ```
       - nohup /usr/local/bin/dockerd --host=unix:///var/run/docker.sock --host=tcp://127.0.0.1:2375 --storage-driver=overlay&
       - timeout -t 15 sh -c "until docker info; do echo .; sleep 1; done"
       ```
   + *badgeEnabled*: Optional value\. To include build badges with your AWS CodeBuild project, you must specify *badgeEnabled* with a value of `true`\. For more information, see [Build Badges Sample with AWS CodeBuild](sample-build-badges.md)\.
   + *timeoutInMinutes*: Optional value\. The number of minutes, between 5 to 480 \(8 hours\), after which AWS CodeBuild stops the build if it is not complete\. If not specified, the default of 60 is used\. To determine if and when AWS CodeBuild stopped a build due to a timeout, run the `batch-get-builds` command\. To determine if the build has stopped, look in the output for a `buildStatus` value of `FAILED`\. To determine when the build timed out, look in the output for the `endTime` value associated with a `phaseStatus` value of `TIMED_OUT`\. 
   + *encryptionKey*: Optional value\. The alias or ARN of the AWS KMS customer master key \(CMK\) AWS CodeBuild uses to encrypt the build output\. If you specify an alias, use the format `arn:aws:kms:region-ID:account-ID:key/key-ID` or, if an alias exists, use the format `alias/key-alias`\. If not specified, the AWS\-managed CMK for Amazon S3 is used\.
   + For the optional *tags* array, information about any tags you want to associate with this build project\. You can specify up to 50 tags\. These tags can be used by any AWS service that supports AWS CodeBuild build project tags\. Each tag is expressed as an object that contains a `key` and `value` value of *tag\-key* and *tag\-value*\.

   For an example, see [To create the build project \(AWS CLI\)](getting-started.md#getting-started-create-build-project-cli)\.

1. Switch to the directory that contains the file you just saved, and run the `create-project` command again:

   ```
   aws codebuild create-project --cli-input-json file://create-project.json
   ```

1. If successful, data similar to the following appears in the output:

   ```
   {
     "project": {
       "name": "project-name",
       "description": "description",
       "serviceRole": "serviceRole",
       "tags": [
         {
           "key": "tags-key",
           "value": "tags-value"
         }
       ],
       "artifacts": {
         "namespaceType": "namespaceType",
         "packaging": "packaging",
         "path": "path",
         "type": "artifacts-type",
         "location": "artifacts-location",
         "name": "artifacts-name"          
       },
       "lastModified": lastModified,
       "timeoutInMinutes": timeoutInMinutes,
       "created": created,
       "environment": {
         "computeType": "computeType",
         "image": "image",
         "type": "environment-type",
         "environmentVariables": [
           {
             "name": "environmentVariable-name",
             "value": "environmentVariable-value",
             "type": "environmentVariable-type"
           }
         ]
       },
       "source": {
         "type": "source-type",
         "location": "source-location",
         "buildspec": "buildspec",
         "auth": {
           "type": "auth-type",
           "resource": "resource"
         }
       },    
       "encryptionKey": "encryptionKey",
       "arn": "arn"        
     }
   }
   ```
   + The `project` object contains information about the new build project:
     + The `lastModified` value represents the time, in Unix time format, when information about the build project was last changed\.
     + The `created` value represents the time, in Unix time format, when the build project was created\. 
     + The `arn` value represents the ARN of the build project\. 

**Note**  
Except for the build project name, you can change any of the build project's settings later\. For more information, see [Change a Build Project's Settings \(AWS CLI\)](change-project.md#change-project-cli)\.

To start running a build, see [Run a Build \(AWS CLI\)](run-build.md#run-build-cli)\.

If your source code is stored in a GitHub repository, and you want AWS CodeBuild to rebuild the source code every time a code change is pushed to the repository, see [Start Running Builds Automatically \(AWS CLI\)](run-build.md#run-build-cli-auto-start)\.

## Create a Build Project \(AWS SDKs\)<a name="create-project-sdks"></a>

For information about using AWS CodeBuild with the AWS SDKs, see the [AWS SDKs and Tools Reference](sdk-ref.md)\.

## Create a Build Project \(AWS CloudFormation\)<a name="create-project-cloud-formation"></a>

For information about using AWS CodeBuild with AWS CloudFormation, see [the AWS CloudFormation template for AWS CodeBuild](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-codebuild-project.html) in the *AWS CloudFormation User Guide*\.