# Create a build project \(AWS CLI\)<a name="create-project-cli"></a>

For information about using the AWS CLI with CodeBuild, see the [Command line reference](cmd-ref.md)\.

1. Run the create\-project command:

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
       "buildStatusConfig": {
         "context": context,
         "targetUrl": target-url
       },
       "gitSubmodulesConfig": {
         "fetchSubmodules": "fetchSubmodules"
       },
       "auth": {
         "type": "auth-type",
         "resource": "resource"
       }
     },
     "sourceVersion": "source-version",
     "secondarySourceVersions": {
       "sourceIdentifier": "secondary-source-identifier",
       "sourceVersion": "secondary-source-version"
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
       "location": "cache-location",
       "mode": [
         "cache-mode"
       ]
     },
     "logsConfig": {
       "cloudWatchLogs": {
         "status": "cloudwatch-logs-status",
         "groupName": "group-name",
         "streamName": "stream-name"      
       }
       "s3Logs": {
         "status": "s3-logs-status",
         "location": "s3-logs-location",
         "encryptionDisabled": "s3-logs-encryptionDisabled"
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
     "fileSystemLocations": [
       {
         "type": "EFS",
         "location": "EFS-DNS-name-1:/directory-path",
         "mountPoint": "mount-point",
         "identifier": "efs-identifier",
         "mountOptions": "efs-mount-options"
       }, 
       { 
         "type": "EFS",
         "location": "EFS-DNS-name-2:/directory-path",
         "mountPoint": "mount-point",
         "identifier": "efs-identifier",
         "mountOptions": "efs-mount-options"
       }
     ],
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
       "registryCredential": [
         {
           "credential": "credential-arn-or-name",
           "credentialProvider": "credential-provider"
         }
       ],
       "imagePullCredentialsType": "imagePullCredentialsType-value,
       "privilegedMode": "privilegedMode"
     },
     "badgeEnabled": "badgeEnabled"
   }
   ```

   Replace the following:
   + *project\-name*: Required\. The name for this build project\. This name must be unique across all of the build projects in your AWS account\.
   + *description*: Optional\. The description for this build project\.
   + <a name="cli-sources"></a>For the required `source` object, information about this build project's source code settings\. After you add a `source` object, you can add up to 12 more sources using the [CodeBuild secondarySources object](#cli-secondary-sources)\. These settings include the following:
     + <a name="cli-sources-type"></a>*source\-type*: Required\. The type of repository that contains the source code to build\. Valid values include `CODECOMMIT`, `CODEPIPELINE`, `GITHUB`, `GITHUB_ENTERPRISE`, `BITBUCKET`, `S3`, and `NO_SOURCE`\. If you use `NO_SOURCE`, the buildspec cannot be a file because the project does not have a source\. Instead, you must use the `buildspec` attribute to specify a YAML\-formatted string for your buildspec\. For more information, see [Project without a source sample](sample-multi-in-out.md#no-source)\.
     + <a name="cli-sources-location"></a>*source\-location*: Required unless you set *source\-type* to `CODEPIPELINE`\. The location of the source code for the specified repository type\.
       + For CodeCommit, the HTTPS clone URL to the repository that contains the source code and the buildspec file \(for example, `https://git-codecommit.region-id.amazonaws.com/v1/repos/repo-name`\)\.
       + For Amazon S3, the build input bucket name, followed by a forward slash \(`/`\), followed by the name of the ZIP file that contains the source code and the buildspec \(for example, `bucket-name/object-name.zip`\)\. This assumes that the ZIP file is in the root of the build input bucket\. \(If the ZIP file is in a folder inside of the bucket, use `bucket-name/path/to/object-name.zip` instead\.\)
       + For GitHub, the HTTPS clone URL to the repository that contains the source code and the buildspec file\. The URL must contain github\.com\. You must connect your AWS account to your GitHub account\. To do this, use the CodeBuild console to create a build project\.

         1. When you use the console to connect \(or reconnect\) with GitHub, on the GitHub **Authorize application** page, for **Organization access**, choose **Request access** next to each repository you want CodeBuild to be able to access\.

         1.  Choose **Authorize application**\. \(After you have connected to your GitHub account, you do not need to finish creating the build project\. You can close the CodeBuild console\.\) 
       + For GitHub Enterprise Server, the HTTP or HTTPS clone URL to the repository that contains the source code and the buildspec file\. You must also connect your AWS account to your GitHub Enterprise Server account\. To do this, use the CodeBuild console to create a build project\.

         1. Create a personal access token in GitHub Enterprise Server\.

         1. Copy this token to your clipboard so you can use it when you create your CodeBuild project\. For more information, see [Creating a personal access token for the command line](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/) on the GitHub Help website\. 

         1. When you use the console to create your CodeBuild project, in **Source**, for **Source provider**, choose **GitHub Enterprise**\.

         1. For **Personal Access Token**, paste the token that was copied to your clipboard\. Choose **Save Token**\. Your CodeBuild account is now connected to your GitHub Enterprise Server account\.
       + For Bitbucket, the HTTPS clone URL to the repository that contains the source code and the buildspec file\. The URL must contain bitbucket\.org\. You must also connect your AWS account to your Bitbucket account\. To do this, use the CodeBuild console to create a build project\. 

         1. When you use the console to connect \(or reconnect\) with Bitbucket, on the Bitbucket **Confirm access to your account** page, choose **Grant access**\. \(After you have connected to your Bitbucket account, you do not need to finish creating the build project\. You can close the CodeBuild console\.\) 
       + For AWS CodePipeline, do not specify a `location` value for `source`\. CodePipeline ignores this value because when you create a pipeline in CodePipeline, you specify the source code location in the Source stage of the pipeline\.
     + <a name="cli-sources-gitclonedepth"></a>*gitCloneDepth*: Optional\. The depth of history to download\. Minimum value is 0\. If this value is 0, greater than 25, or not provided, then the full history is downloaded with each build project\. If your source type is Amazon S3, this value is not supported\.
     + <a name="cli-sources-buildspec"></a>*buildspec*: Optional\. The build specification definition or file to use\. If this value is set, it can be either an inline buildspec definition, the path to an alternate buildspec file relative to the value of the built\-in `CODEBUILD_SRC_DIR` environment variable, or the path to an S3 bucket\. The bucket must be in the same AWS Region as the build project\. Specify the buildspec file using its ARN \(for example, `arn:aws:s3:::my-codebuild-sample2/buildspec.yml`\)\. If this value is not provided or is set to an empty string, the source code must contain a `buildspec.yml` file in its root directory\. For more information, see [Buildspec file name and storage location](build-spec-ref.md#build-spec-ref-name-storage)\.
     + <a name="cli-sources-auth"></a>*auth*: This object is used by the CodeBuild console only\. Do not specify values for *auth\-type* \(unless *source\-type* is set to `GITHUB`\) or *resource*\.
     + <a name="cli-sources-reportbuildstatus"></a>*reportBuildStatus*: Optional\. Specifies whether to send your source provider the status of a build's start and completion\. If you set this with a source provider other than GitHub, GitHub Enterprise Server, or Bitbucket, an `invalidInputException` is thrown\.
     + <a name="create-project-cli.build-status-config"></a>*buildStatusConfig*: Optional\. Contains information that defines how the CodeBuild build project reports the build status to the source provider\. This option is only used when the source type is `GITHUB`, `GITHUB_ENTERPRISE`, or `BITBUCKET`\.
       + *context*: For Bitbucket sources, this parameter is used for the `name` parameter in the Bitbucket commit status\. For GitHub sources, this parameter is used for the `context` parameter in the GitHub commit status\. 

         For example, you can have the `context` contain the build number and the webhook trigger using the CodeBuild environment variables:

         ```
         AWS CodeBuild sample-project Build #$CODEBUILD_BUILD_NUMBER - $CODEBUILD_WEBHOOK_TRIGGER
         ```

         This results in the context appearing like this for build \#24 triggered by a webhook pull request event:

         ```
         AWS CodeBuild sample-project Build #24 - pr/8
         ```
       + *target\-url*: For Bitbucket sources, this parameter is used for the `url` parameter in the Bitbucket commit status\. For GitHub sources, this parameter is used for the `target_url` parameter in the GitHub commit status\.

         For example, you can set the `targetUrl` to `https://aws.amazon.com/codebuild/` and the commit status will link to this URL\.
     + <a name="cli-sources-gitsubmodulesconfig"></a>*gitSubmodulesConfig*: Optional\. Information about the Git submodules configuration\. Used with CodeCommit, GitHub, GitHub Enterprise Server, and Bitbucket only\. Set `fetchSubmodules` to `true` if you want to include the Git submodules in your repository\. Git submodules that are included must be configured as HTTPS\.
     + <a name="cli-sources-insecuressl"></a>*InsecureSsl*: Optional\. Used with GitHub Enterprise Server only\. Set this value to `true` to ignore TLS warnings while connecting to your GitHub Enterprise Server project repository\. The default value is `false`\. *InsecureSsl* should be used for testing purposes only\. It should not be used in a production environment\.
   + <a name="cli-sourceversion"></a> *source\-version*: Optional\. A version of the build input to be built for this project\. If not specified, the latest version is used\. If specified, it must be one of: 
     +  For CodeCommit, the commit ID to use\. 
     +  For GitHub, the commit ID, pull request ID, branch name, or tag name that corresponds to the version of the source code you want to build\. If a pull request ID is specified, it must use the format `pr/pull-request-ID` \(for example `pr/25`\)\. If a branch name is specified, the branch's HEAD commit ID is used\. If not specified, the default branch's HEAD commit ID is used\. 
     +  For Bitbucket, the commit ID, branch name, or tag name that corresponds to the version of the source code you want to build\. If a branch name is specified, the branch's HEAD commit ID is used\. If not specified, the default branch's HEAD commit ID is used\. 
     +  For Amazon S3, the version ID of the object that represents the build input ZIP file to use\. 

      If `sourceVersion` is specified at the build level, then that version takes precedence over this `sourceVersion` \(at the project level\)\. For more information, see [Source version sample with AWS CodeBuild](sample-source-version.md)\. 
   + <a name="cli-secondarysourceversions"></a> *secondarySourceVersions*: Optional\. An array of `projectSourceVersion` objects\. If `secondarySourceVersions` is specified at the build level, then they take precedence over this\. 
     +  *secondary\-source\-identifier*: An identifier for a source in the build project\. 
     +  *secondary\-source\-version*: A `sourceVersion` object\. 
   + <a name="cli-artifacts"></a>For the required `artifacts` object, information about this build project's output artifact settings\. After you add an `artifacts` object, you can add up to 12 more artifacts using the [CodeBuild secondaryArtifacts object](#cli-secondary-artifacts)\. These settings include the following: <a name="cli-artifacts-type"></a>
     + *artifacts\-type*: Required\. The type of build output artifact\. Valid values include `CODEPIPELINE`, `NO_ARTIFACTS`, and `S3`\.
     + <a name="cli-artifacts-location"></a>*artifacts\-location*: Required unless you set *artifacts\-type* to `CODEPIPELINE` or `NO_ARTIFACTS`\. The location of the build output artifact:
       + If you specified `CODEPIPELINE` for *artifacts\-type*, do not specify a `location` for `artifacts`\.
       + If you specified `NO_ARTIFACTS` for *artifacts\-type*, do not specify a `location` for `artifacts`\.
       + If you specified `S3` for *artifacts\-type*, this is the name of the output bucket you created or identified in the prerequisites\. 
     + <a name="cli-artifacts-path"></a>*path*: Optional\. The path and name of the build output ZIP file or folder:
       + If you specified `CODEPIPELINE` for *artifacts\-type*, do not specify a `path` for `artifacts`\.
       + If you specified `NO_ARTIFACTS` for *artifacts\-type*, do not specify a `path` for `artifacts`\.
       + If you specified `NO_ARTIFACTS` for *artifacts\-type*, do not specify a `path` for `artifacts`\.
       + If you specified `S3` for *artifacts\-type*, this is the path inside of *artifacts\-location* to the build output ZIP file or folder\. If you do not specify a value for *path*, CodeBuild uses *namespaceType* \(if specified\) and *artifacts\-name* to determine the path and name of the build output ZIP file or folder\. For example, if you specify `MyPath` for *path* and `MyArtifact.zip` for *artifacts\-name*, the path and name would be `MyPath/MyArtifact.zip`\.
     + <a name="cli-artifacts-namespacetype"></a>*namespaceType*: Optional\. The path and name of the build output ZIP file or folder:
       + If you specified `CODEPIPELINE` for *artifacts\-type*, do not specify a `namespaceType` for `artifacts`\. 
       + If you specified `NO_ARTIFACTS` for *artifacts\-type*, do not specify a `namespaceType` for `artifacts`\.
       + If you specified `S3` for *artifacts\-type*, valid values include `BUILD_ID` and `NONE`\. Use `BUILD_ID` to insert the build ID into the path of the build output ZIP file or folder\. Otherwise, use `NONE`\. If you do not specify a value for *namespaceType*, CodeBuild uses *path* \(if specified\) and *artifacts\-name* to determine the path and name of the build output ZIP file or folder\. For example, if you specify `MyPath` for *path*, `BUILD_ID` for *namespaceType*, and `MyArtifact.zip` for *artifacts\-name*, the path and name would be `MyPath/build-ID/MyArtifact.zip`\.
     + <a name="cli-artifacts-name"></a>*artifacts\-name*: Required unless you set *artifacts\-type* to `CODEPIPELINE` or `NO_ARTIFACTS`\. The path and name of the build output ZIP file or folder:
       + If you specified `CODEPIPELINE` for *artifacts\-type*, do not specify a `name` for `artifacts`\.
       + If you specified `NO_ARTIFACTS` for *artifacts\-type*, do not specify a `name` for `artifacts`\.
       + If you specified `S3` for *artifacts\-type*, this is the name of the build output ZIP file or folder inside of *artifacts\-location*\. For example, if you specify `MyPath` for *path* and `MyArtifact.zip` for *artifacts\-name*, the path and name would be `MyPath/MyArtifact.zip`\.
     + *override\-artifact\-name*: Optional boolean\. If set to `true`, the name specified in the `artifacts` block of the buildspec file overrides *artifacts\-name*\. For more information, see [Build specification reference for CodeBuild](build-spec-ref.md)\.
     + <a name="cli-artifacts-packaging"></a>*packaging*: Optional\. The type of build output artifact to create:
       + If you specified `CODEPIPELINE` for *artifacts\-type*, do not specify a `packaging` for `artifacts`\.
       + If you specified `NO_ARTIFACTS` for *artifacts\-type*, do not specify a `packaging` for `artifacts`\.
       + If you specified `S3` for *artifacts\-type*, valid values include `ZIP` and `NONE`\. To create a ZIP file that contains the build output, use `ZIP`\. To create a folder that contains the build output, use `NONE`\. The default value is `NONE`\.
   + For the required `cache` object, information about this build project's cache settings\. For information, see [Build caching](build-caching.md)\. These settings include the following\. 
     + *cache\-type*: Required\. Valid values are `S3`, `NO_CACHE`, or `LOCAL_CACHE`\.
     + *cache\-location*: Required only if you set *CacheType* to `S3`\. If you specified Amazon S3 for *CacheType*, this is the ARN of the S3 bucket and the path prefix\. For example, if your S3 bucket name is `my-bucket`, and your path prefix is `build-cache`, then acceptable formats for your *CacheLocation* are `my-bucket/build-cache` or `arn:aws:s3:::my-bucket/build-cache`\.
     + *cache\-mode*: Required if you set *CacheType* to `LOCAL`\. You can specify one or more of the following local cache modes: `LOCAL_SOURCE_CACHE`, `LOCAL_DOCKER_LAYER_CACHE`, `LOCAL_CUSTOM_CACHE`\.
**Note**  
Docker layer cache mode is available for Linux only\. If you choose it, your project must run in privileged mode\. The `ARM_CONTAINER` and `LINUX_GPU_CONTAINER` environment types and the `BUILD_GENERAL1_2XLARGE` compute type do not support the use of a local cache\.
   + For the `logsConfig` object, information about where this build's logs are located:
     +  *cloudwatch\-logs\-status*: Required\. Valid values are `ENABLED` or `DISABLED`\. If its value is `ENABLED`, the following values are required\. For more information, see [Working with log groups and log streams](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/Working-with-log-groups-and-streams.html) in the *Amazon CloudWatch Logs User Guide*\.
     +  *group\-name*: The name of the CloudWatch Logs group\. 
     +  *stream\-name*: The name of the CloudWatch Logs stream\. 
     +  *s3\-logs\-status*: Required\. Valid values are `ENABLED` or `DISABLED`\. 
     +  *s3\-logs\-location*: Required if *s3\-logs\-status* is `ENABLED`\. This is the ARN of an S3 bucket and the path prefix\. For example, if your S3 bucket name is `my-bucket`, and your path prefix is `build-log`, then acceptable formats for your *s3\-logs\-location* are `my-bucket/build-log` or `arn:aws:s3:::my-bucket/build-log`\. 
     +  *s3\-logs\-encryptionDisabled*: Optional boolean\. If set to `true`, your S3 build log output is not encrypted\. By default, S3 build logs are encrypted\. 
   + <a name="cli-secondary-artifacts"></a>For the optional `secondaryArtifacts` object, information about the settings of a secondary artifact for a build project\. You can add up to 12 secondary artifacts\. The `secondaryArtifacts` uses many of the same settings used by the [CodeBuild artifacts object](#cli-artifacts) object\. The settings are: 
     + *artifacts\-type*: Required\. This setting is also used by the `artifacts` object\. See [CodeBuild artifact object's type property](#cli-artifacts-type)\.
     + *artifacts\-location*: Required\. This setting is also used by the `artifacts` object\. See [CodeBuild artifact object's location property](#cli-artifacts-location)\.
     + *path*: Optional\. This setting is also used by the `artifacts` object\. See [CodeBuild artifact object's path property](#cli-artifacts-path)\.
     + *namespaceType*: Optional\. This setting is also used by the `artifacts` object\. See [CodeBuild artifact object's namespaceType property](#cli-artifacts-namespacetype)\.
     + *artifacts\-name*: Required\. This setting is also used by the `artifacts` object\. See [CodeBuild artifact object's name property](#cli-artifacts-name)\.
     + *packaging*: Optional\. This setting is also used by the `artifacts` object\. See [CodeBuild artifact object's packaging property](#cli-artifacts-packaging)\.
     + *artifact\-identifier*: Required\. A unique string identifier for a secondary artifact\.
   + <a name="cli-secondary-sources"></a>For the optional `secondarySources` object, information about the settings of a secondary source for a build project\. You can add up to 12 `secondarySources`\. The `secondarySources` object uses many of the same settings used by the [CodeBuild source object](#cli-sources)\. They include the following:
     + *source\-type*: Required\. This setting is also used by the `sources` object\. See [CodeBuild source object's type property](#cli-sources-type)\.
     + *source\-location*: Required\. This setting is also used by the `sources` object\. See [CodeBuild source object's location property](#cli-sources-location)\.
     + *gitCloneDepth*: Optional\. This setting is also used by the `sources` object\. See [CodeBuild source object's location property](#cli-sources-location)\.
     + *buildspec*: Optional\. This setting is also used by the `sources` object\. See [CodeBuild source object's buildspec property](#cli-sources-buildspec)\.
     + *auth*: This setting is also used by the `sources` object\. See [CodeBuild source object's auth property](#cli-sources-auth)\.
     + *reportBuildStatus*: Optional\. This setting is also used by the `sources` object\. See [CodeBuild source object's reportBuildStatus property](#cli-sources-reportbuildstatus)\.
     + *InsecureSsl*: Optional\. This setting is also used by the `sources` object\. See [CodeBuild source object's insecureSsl property](#cli-sources-insecuressl)\.
     + *source\-identifier*: Required\. A unique string identifier for a secondary source\.
   + *serviceRole*: Required\. The ARN of the service role CodeBuild uses to interact with services on behalf of the IAM user \(for example, `arn:aws:iam::account-id:role/role-name`\)\.
   + For the optional *vpcConfig* object, information about your VPC configuration\. These settings include: 
     + *vpcId*: Required\. The VPC ID that CodeBuild uses\. Run this command to get a list of all VPC IDs in your Region:

       ```
       aws ec2 describe-vpcs
       ```
     + *subnets*: Required\. The subnet IDs that include resources used by CodeBuild\. Run this command to get these IDs:

       ```
       aws ec2 describe-subnets --filters "Name=vpc-id,Values=<vpc-id>" --region us-east-1
       ```

       If you are using a Region other than `us-east-1`, be sure to use it when you run the command\.
     + *securityGroupIds*: Required\. The security group IDs used by CodeBuild to allow access to resources in the VPCs\. Run this command to get these IDs:

       ```
       aws ec2 describe-security-groups --filters "Name=vpc-id,Values=<vpc-id>" --region us-east-1
       ```

       If you are using a Region other than `us-east-1`, be sure to use it when you run the command\.
   + For the optional *fileSystemLocations* object, information about your Amazon EFS configuration\. These settings include: 
     +  `type`: Required\. This value must be `EFS`\. 
     +  *location*: Required\. The location specified in the format *EFS\-DNS\-name*:/*directory\-path*\. 
     +  *mountPoint*: Required\. The absolute path to the directory in your build container where the file system is mounted\. If this directory does not exist, CodeBuild creates it during the build\. 
     +  *identifier*: Required\. A unique file system identifier\. CodeBuild uses this to create an environment variable that identifies the file system\. The environment variable format is `CODEBUILD_file-system-identifier` in capital letters\. For example, if you enter **efs\-1**, the resulting environment variable is `CODEBUILD_EFS-1`\. 
     +  *mountOptions*: Optional\. If you leave this blank, CodeBuild uses its default mount options \(`nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2`\)\. For more information, see [Recommended NFS mount options](https://docs.aws.amazon.com/efs/latest/ug/mounting-fs-nfs-mount-settings.html) in the *Amazon Elastic File System User Guide*\. 
   + For the required `environment` object, information about this project's build environment settings\. These settings include: 
     + *environment\-type*: Required\. The type of build environment\. Valid values are:
       + `ARM_CONTAINER`
       + `LINUX_CONTAINER`
       + `LINUX_GPU_CONTAINER`
       + `WINDOWS_CONTAINER`
       + `WINDOWS_SERVER_2019_CONTAINER`
     + *image*: Required\. The Docker image identifier used by this build environment\. Typically, this identifier is expressed as *image\-name*:*tag*\. For example, in the Docker repository that CodeBuild uses to manage its Docker images, this could be `aws/codebuild/standard:4.0`\. In Docker Hub, `maven:3.3.9-jdk-8`\. In Amazon ECR, `account-id.dkr.ecr.region-id.amazonaws.com/your-Amazon-ECR-repo-name:tag`\. For more information, see [Docker images provided by CodeBuild](build-env-ref-available.md)\. 
     + *computeType*: Required\. A category that corresponds to the number of CPU cores and memory used by this build environment\. Allowed values include:
       + `BUILD_GENERAL1_SMALL`
       + `BUILD_GENERAL1_MEDIUM`
       + `BUILD_GENERAL1_LARGE`
       + `BUILD_GENERAL1_2XLARGE`

       `BUILD_GENERAL1_2XLARGE` is only supported with the `LINUX_CONTAINER` environment type\.
     + *certificate*: Optional\. The ARN of the S3 bucket, path prefix and object key that contains the PEM\-encoded certificate\. The object key can be either just the \.pem file or a \.zip file containing the PEM\-encoded certificate\. For example, if your S3 bucket name is `my-bucket`, your path prefix is `cert`, and your object key name is `certificate.pem`, then acceptable formats for your *certificate* are `my-bucket/cert/certificate.pem` or `arn:aws:s3:::my-bucket/cert/certificate.pem`\.
     + For the optional `environmentVariables` array, information about any environment variables you want to specify for this build environment\. Each environment variable is expressed as an object that contains a `name`, `value`, and `type` of *environmentVariable\-name*, *environmentVariable\-value*, and *environmentVariable\-type*\. 

       Console and AWS CLI users can see an environment variable\. If you have no concerns about the visibility of your environment variable, set *environmentVariable\-name* and *environmentVariable\-value*, and then set *environmentVariable\-type* to `PLAINTEXT`\.

       We recommend you store an environment variable with a sensitive value, such as an AWS access key ID, an AWS secret access key, or a password as a parameter in Amazon EC2 Systems Manager Parameter Store or AWS Secrets Manager\. For *environmentVariable\-name*, for that stored parameter, set an identifier for CodeBuild to reference\. 

       If you use Amazon EC2 Systems Manager Parameter Store, for *environmentVariable\-value*, set the parameter's name as stored in the Parameter Store\. Set *environmentVariable\-type* to `PARAMETER_STORE`\. Using a parameter named `/CodeBuild/dockerLoginPassword` as an example, set *environmentVariable\-name* to `LOGIN_PASSWORD`\. Set *environmentVariable\-value* to `/CodeBuild/dockerLoginPassword`\. Set *environmentVariable\-type* to `PARAMETER_STORE`\. 
**Important**  
If you use Amazon EC2 Systems Manager Parameter Store, we recommend that you store parameters with parameter names that start with `/CodeBuild/` \(for example, `/CodeBuild/dockerLoginPassword`\)\. You can use the CodeBuild console to create a parameter in Amazon EC2 Systems Manager\. Choose **Create parameter**, and then follow the instructions in the dialog box\. \(In that dialog box, for **KMS key**, you can specify the ARN of an AWS KMS key in your account\. Amazon EC2 Systems Manager uses this key to encrypt the parameter's value during storage and decrypt it during retrieval\.\) If you use the CodeBuild console to create a parameter, the console starts the parameter name with `/CodeBuild/` as it is being stored\. For more information, see [Systems Manager Parameter Store](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-paramstore.html) and [Systems Manager Parameter Store Console Walkthrough](https://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-paramstore-walk.html#sysman-paramstore-console) in the *Amazon EC2 Systems Manager User Guide*\.  
If your build project refers to parameters stored in Amazon EC2 Systems Manager Parameter Store, the build project's service role must allow the `ssm:GetParameters` action\. If you chose **New service role** earlier, CodeBuild includes this action in the default service role for your build project\. However, if you chose **Existing service role**, you must include this action to your service role separately\.  
If your build project refers to parameters stored in Amazon EC2 Systems Manager Parameter Store with parameter names that do not start with `/CodeBuild/`, and you chose **New service role**, you must update that service role to allow access to parameter names that do not start with `/CodeBuild/`\. This is because that service role allows access only to parameter names that start with `/CodeBuild/`\.  
If you choose **New service role**, the service role includes permission to decrypt all parameters under the `/CodeBuild/` namespace in the Amazon EC2 Systems Manager Parameter Store\.  
Environment variables you set replace existing environment variables\. For example, if the Docker image already contains an environment variable named `MY_VAR` with a value of `my_value`, and you set an environment variable named `MY_VAR` with a value of `other_value`, then `my_value` is replaced by `other_value`\. Similarly, if the Docker image already contains an environment variable named `PATH` with a value of `/usr/local/sbin:/usr/local/bin`, and you set an environment variable named `PATH` with a value of `$PATH:/usr/share/ant/bin`, then `/usr/local/sbin:/usr/local/bin` is replaced by the literal value `$PATH:/usr/share/ant/bin`\.  
Do not set any environment variable with a name that begins with `CODEBUILD_`\. This prefix is reserved for internal use\.  
If an environment variable with the same name is defined in multiple places, the value is determined as follows:  
The value in the start build operation call takes highest precedence\.
The value in the build project definition takes next precedence\.
The value in the buildspec declaration takes lowest precedence\.

       If you use Secrets Manager, for *environmentVariable\-value*, set the parameter's name as stored in Secrets Manager\. Set *environmentVariable\-type* to `SECRETS_MANAGER`\. Using a secret named `/CodeBuild/dockerLoginPassword` as an example, set *environmentVariable\-name* to `LOGIN_PASSWORD`\. Set *environmentVariable\-value* to `/CodeBuild/dockerLoginPassword`\. Set *environmentVariable\-type* to `SECRETS_MANAGER`\.
**Important**  
If you use Secrets Manager, we recommend that you store secrets with names that start with `/CodeBuild/` \(for example, `/CodeBuild/dockerLoginPassword`\)\. For more information, see [What Is AWS Secrets Manager?](https://docs.aws.amazon.com/secretsmanager/latest/userguide/intro.html) in the *AWS Secrets Manager User Guide*\.   
If your build project refers to secrets stored in Secrets Manager, the build project's service role must allow the `secretsmanager:GetSecretValue` action\. If you chose **New service role** earlier, CodeBuild includes this action in the default service role for your build project\. However, if you chose **Existing service role**, you must include this action to your service role separately\.   
If your build project refers to secrets stored in Secrets Manager with secret names that do not start with `/CodeBuild/`, and you chose **New service role**, you must update the service role to allow access to secret names that do not start with `/CodeBuild/`\. This is because the service role allows access only to secret names that start with `/CodeBuild/`\.  
If you choose **New service role**, the service role includes permission to decrypt all secrets under the `/CodeBuild/` namespace in the Secrets Manager\.
     + Use the optional `registryCredential` to specify information about credentials that provide access to a private Docker registry\. 
       + *credential\-arn\-or\-name*: Specifies the ARN or name of credentials created using AWS Managed Services \. You can use the name of the credentials only if they exist in your current Region\.
       + *credential\-provider*: The only valid value is `SECRETS_MANAGER`\.

       When this is set: 
       + `imagePullCredentials` must be set to `SERVICE_ROLE`\.
       + Images cannot be curated or an Amazon ECR image\.
     + *imagePullCredentialsType\-value*: Optional\. The type of credentials CodeBuild uses to pull images in your build\. There are two valid values:
       +  `CODEBUILD` specifies that CodeBuild uses its own credentials\. You must edit your Amazon ECR repository policy to trust the CodeBuild service principal\. 
       +  `SERVICE_ROLE` specifies that CodeBuild uses your build project's service role\. 

        When you use a cross\-account or private registry image, you must use `SERVICE_ROLE` credentials\. When you use a CodeBuild curated image, you must use `CODEBUILD` credentials\. 
     + You must specify *privilegedMode* with a value of `true` only if you plan to use this build project to build Docker images, and the build environment image you specified is not provided by CodeBuild with Docker support\. Otherwise, all associated builds that attempt to interact with the Docker daemon fail\. You must also start the Docker daemon so that your builds can interact with it\. One way to do this is to initialize the Docker daemon in the `install` phase of your buildspec file by running the following build commands\. Do not run these commands if you specified a build environment image provided by CodeBuild with Docker support\.
**Note**  
By default, Docker containers do not allow access to any devices\. Privileged mode grants a build project's Docker container access to all devices\. For more information, see [Runtime Privilege and Linux Capabilities](https://docs.docker.com/engine/reference/run/#runtime-privilege-and-linux-capabilities) on the Docker Docs website\.

       ```
       - nohup /usr/local/bin/dockerd --host=unix:///var/run/docker.sock --host=tcp://127.0.0.1:2375 --storage-driver=overlay&
       - timeout -t 15 sh -c "until docker info; do echo .; sleep 1; done"
       ```
   + *badgeEnabled*: Optional\. To include build badges with your CodeBuild project, you must specify *badgeEnabled* with a value of `true`\. For more information, see [Build badges sample with CodeBuild](sample-build-badges.md)\.
   + *timeoutInMinutes*: Optional\. The number of minutes, between 5 to 480 \(8 hours\), after which CodeBuild stops the build if it is not complete\. If not specified, the default of 60 is used\. To determine if and when CodeBuild stopped a build due to a timeout, run the `batch-get-builds` command\. To determine if the build has stopped, look in the output for a `buildStatus` value of `FAILED`\. To determine when the build timed out, look in the output for the `endTime` value associated with a `phaseStatus` value of `TIMED_OUT`\. 
   + <a name="encryptionkey-cli"></a>*encryptionKey*: Optional\. The alias or ARN of the AWS KMS customer managed key \(CMK\) used by CodeBuild to encrypt the build output\. If you specify an alias, use the format `arn:aws:kms:region-ID:account-ID:key/key-ID` or, if an alias exists, use the format `alias/key-alias`\. If not specified, the AWS\-managed CMK for Amazon S3 is used\.
   + For the optional *tags* array, information about any tags you want to associate with this build project\. You can specify up to 50 tags\. These tags can be used by any AWS service that supports CodeBuild build project tags\. Each tag is expressed as an object with a `key` and `value` value of *tag\-key* and *tag\-value*\.

1. Switch to the directory that contains the file you just saved, and run the create\-project command again:

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
     + The `arn` value is the ARN of the build project\. 

**Note**  
Except for the build project name, you can change any of the build project's settings later\. For more information, see [Change a build project's settings \(AWS CLI\)](change-project.md#change-project-cli)\.

To start running a build, see [Run a build \(AWS CLI\)](run-build-cli.md)\.

If your source code is stored in a GitHub repository, and you want CodeBuild to rebuild the source code every time a code change is pushed to the repository, see [Start running builds automatically \(AWS CLI\)](run-build-cli-auto-start.md)\.