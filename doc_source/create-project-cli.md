# Create a build project \(AWS CLI\)<a name="create-project-cli"></a>

For more information about using the AWS CLI with CodeBuild, see the [Command line reference](cmd-ref.md)\.

To create a CodeBuild build project using the AWS CLI, you create a JSON\-formatted [Project](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_Project.html) structure, fill in the structure, and call the [https://docs.aws.amazon.com/cli/latest/reference/codebuild/create-project.html](https://docs.aws.amazon.com/cli/latest/reference/codebuild/create-project.html) command to create the project\.

## Create the JSON file<a name="cp-cli-create-file"></a>

Create a skeleton JSON file with the [https://docs.aws.amazon.com/cli/latest/reference/codebuild/create-project.html](https://docs.aws.amazon.com/cli/latest/reference/codebuild/create-project.html) command, using the `--generate-cli-skeleton` option:

```
aws codebuild create-project --generate-cli-skeleton > <json-file>
```

This creates a JSON file with the path and file name specified by *<json\-file>*\.

## Fill in the JSON file<a name="cp-cli-fill-in-file"></a>

Modify the JSON data as follows and save your results\.

```
{
  "name": "<project-name>",
  "description": "<description>",
  "source": {
    "type": "CODECOMMIT" | "CODEPIPELINE" | "GITHUB" | "GITHUB_ENTERPRISE" | "BITBUCKET" | "S3" | "NO_SOURCE",
    "location": "<source-location>",
    "gitCloneDepth": "<git-clone-depth>",
    "buildspec": "<buildspec>",
    "InsecureSsl": "<insecure-ssl>",
    "reportBuildStatus": "<report-build-status>",
    "buildStatusConfig": {
      "context": "<context>",
      "targetUrl": "<target-url>"
    },
    "gitSubmodulesConfig": {
      "fetchSubmodules": "<fetch-submodules>"
    },
    "auth": {
      "type": "<auth-type>",
      "resource": "<auth-resource>"
    },
    "sourceIdentifier": "<source-identifier>"
  },
  "secondarySources": [
    {
        "type": "CODECOMMIT" | "CODEPIPELINE" | "GITHUB" | "GITHUB_ENTERPRISE" | "BITBUCKET" | "S3" | "NO_SOURCE",
        "location": "<source-location>",
        "gitCloneDepth": "<git-clone-depth>",
        "buildspec": "<buildspec>",
        "InsecureSsl": "<insecure-ssl>",
        "reportBuildStatus": "<report-build-status>",
        "auth": {
          "type": "<auth-type>",
          "resource": "<auth-resource>"
        },
        "sourceIdentifier": "<source-identifier>"
    }
  ],
  "secondarySourceVersions": [
    {
      "sourceIdentifier": "<secondary-source-identifier>",
      "sourceVersion": "<secondary-source-version>"
    }
  ],
  "sourceVersion": "<source-version>",
  "artifacts": {
    "type": "CODEPIPELINE" | "S3" | "NO_ARTIFACTS",
    "location": "<artifacts-location>",
    "path": "<artifacts-path>",
    "namespaceType": "<artifacts-namespacetype>",
    "name": "<artifacts-name>",
    "overrideArtifactName": "<override-artifact-name>",
    "packaging": "<artifacts-packaging>"
  },
  "secondaryArtifacts": [
    {
      "type": "CODEPIPELINE" | "S3" | "NO_ARTIFACTS",
      "location": "<secondary-artifact-location>",
      "path": "<secondary-artifact-path>",
      "namespaceType": "<secondary-artifact-namespaceType>",
      "name": "<secondary-artifact-name>",
      "packaging": "<secondary-artifact-packaging>",
      "artifactIdentifier": "<secondary-artifact-identifier>"
    }
  ],
  "cache": {
    "type": "<cache-type>",
    "location": "<cache-location>",
    "mode": [
      "<cache-mode>"
    ]
  },
  "environment": {
    "type": "WINDOWS_CONTAINER" | "LINUX_CONTAINER" | "LINUX_GPU_CONTAINER" | "ARM_CONTAINER" | "WINDOWS_SERVER_2019_CONTAINER",
    "image": "<image>",
    "computeType": "BUILD_GENERAL1_SMALL" | "BUILD_GENERAL1_MEDIUM" | "BUILD_GENERAL1_LARGE" | "BUILD_GENERAL1_2XLARGE",
    "certificate": "<certificate>",
    "environmentVariables": [
      {
        "name": "<environmentVariable-name>",
        "value": "<environmentVariable-value>",
        "type": "<environmentVariable-type>"
      }
    ],
    "registryCredential": [
      {
        "credential": "<credential-arn-or-name>",
        "credentialProvider": "<credential-provider>"
      }
    ],
    "imagePullCredentialsType": "CODEBUILD" | "SERVICE_ROLE",
    "privilegedMode": "<privileged-mode>"
  },
  "serviceRole": "<service-role>",
  "timeoutInMinutes": <timeout>,
  "queuedTimeoutInMinutes": <queued-timeout>,
  "encryptionKey": "<encryption-key>",
  "tags": [
    {
      "key": "<tag-key>",
      "value": "<tag-value>"
    }
  ],
  "vpcConfig": {
    "securityGroupIds": [
         "<security-group-id>"
    ],
    "subnets": [
         "<subnet-id>"
    ],
    "vpcId": "<vpc-id>"
  },
  "badgeEnabled": "<badge-enabled>",
  "logsConfig": {
    "cloudWatchLogs": {
      "status": "<cloudwatch-logs-status>",
      "groupName": "<group-name>",
      "streamName": "<stream-name>"
    },
    "s3Logs": {
      "status": "<s3-logs-status>",
      "location": "<s3-logs-location>",
      "encryptionDisabled": "<s3-logs-encryption-disabled>"
    }
  },
  "fileSystemLocations": [
    {
      "type": "EFS",
      "location": "<EFS-DNS-name-1>:/<directory-path>",
      "mountPoint": "<mount-point>",
      "identifier": "<efs-identifier>",
      "mountOptions": "<efs-mount-options>"
    }
  ],
  "buildBatchConfig": {
    "serviceRole": "<batch-service-role>",
    "combineArtifacts": <combine-artifacts>,
    "restrictions": {
      "maximumBuildsAllowed": <max-builds>,
      "computeTypesAllowed": [
        "<compute-type>"
      ]
    },
    "timeoutInMins": <batch-timeout>
  }
}
```

Replace the following:

### **name**<a name="cli.project-name"></a>

Required\. The name for this build project\. This name must be unique across all of the build projects in your AWS account\.

### **description**<a name="cli.description"></a>

Optional\. The description for this build project\.

### **source**<a name="cli.source"></a>

Required\. A [ProjectSource](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_ProjectSource.html) object that contains information about this build project's source code settings\. After you add a `source` object, you can add up to 12 more sources using the [**secondarySources**](#cli.secondarysources)\. These settings include the following:

source/**type**  <a name="cli.source.type"></a>
Required\. The type of repository that contains the source code to build\. Valid values include:  
+ `CODECOMMIT`
+ `CODEPIPELINE`
+ `GITHUB`
+ `GITHUB_ENTERPRISE`
+ `BITBUCKET`
+ `S3`
+ `NO_SOURCE`
If you use `NO_SOURCE`, the buildspec cannot be a file because the project does not have a source\. Instead, you must use the `buildspec` attribute to specify a YAML\-formatted string for your buildspec\. For more information, see [Project without a source sample](sample-multi-in-out.md#no-source)\.

source/**location**  <a name="cli.source.location"></a>
Required unless you set *<source\-type>* to `CODEPIPELINE`\. The location of the source code for the specified repository type\.  
+ For CodeCommit, the HTTPS clone URL to the repository that contains the source code and the buildspec file \(for example, `https://git-codecommit.<region-id>.amazonaws.com/v1/repos/<repo-name>`\)\.
+ For Amazon S3, the build input bucket name, followed by the path and name of the ZIP file that contains the source code and the buildspec\. For example:
  + For a ZIP file located at the root of the input bucket: `<bucket-name>/<object-name>.zip`\.
  + For a ZIP file located in a subfolder in the input bucket: `<bucket-name>/<subfoler-path>/<object-name>.zip`\.
+ For GitHub, the HTTPS clone URL to the repository that contains the source code and the buildspec file\. The URL must contain github\.com\. You must connect your AWS account to your GitHub account\. To do this, use the CodeBuild console to create a build project\.

  1. On the GitHub **Authorize application** page, in the **Organization access** section, choose **Request access** next to each repository you want CodeBuild to be able to access in the \.

  1. Choose **Authorize application**\. \(After you have connected to your GitHub account, you do not need to finish creating the build project\. You can close the CodeBuild console\.\) 
+ For GitHub Enterprise Server, the HTTP or HTTPS clone URL to the repository that contains the source code and the buildspec file\. You must also connect your AWS account to your GitHub Enterprise Server account\. To do this, use the CodeBuild console to create a build project\.

  1. Create a personal access token in GitHub Enterprise Server\.

  1. Copy this token to your clipboard so you can use it when you create your CodeBuild project\. For more information, see [Creating a personal access token for the command line](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/) on the GitHub Help website\. 

  1. When you use the console to create your CodeBuild project, in **Source**, for **Source provider**, choose **GitHub Enterprise**\.

  1. For **Personal Access Token**, paste the token that was copied to your clipboard\. Choose **Save Token**\. Your CodeBuild account is now connected to your GitHub Enterprise Server account\.
+ For Bitbucket, the HTTPS clone URL to the repository that contains the source code and the buildspec file\. The URL must contain bitbucket\.org\. You must also connect your AWS account to your Bitbucket account\. To do this, use the CodeBuild console to create a build project\. 

  1. When you use the console to connect \(or reconnect\) with Bitbucket, on the Bitbucket **Confirm access to your account** page, choose **Grant access**\. \(After you have connected to your Bitbucket account, you do not need to finish creating the build project\. You can close the CodeBuild console\.\) 
+ For AWS CodePipeline, do not specify a `location` value for `source`\. CodePipeline ignores this value because when you create a pipeline in CodePipeline, you specify the source code location in the Source stage of the pipeline\.

source/**gitCloneDepth**  <a name="cli.source.gitclonedepth"></a>
Optional\. The depth of history to download\. Minimum value is 0\. If this value is 0, greater than 25, or not provided, then the full history is downloaded with each build project\. If your source type is Amazon S3, this value is not supported\.

source/**buildspec**  <a name="cli.source.buildspec"></a>
Optional\. The build specification definition or file to use\. If this value is not provided or is set to an empty string, the source code must contain a `buildspec.yml` file in its root directory\. If this value is set, it can be either an inline buildspec definition, the path to an alternate buildspec file relative to the root directory of your primary source, or the path to an S3 bucket\. The bucket must be in the same AWS Region as the build project\. Specify the buildspec file using its ARN \(for example, `arn:aws:s3:::my-codebuild-sample2/buildspec.yml`\)\. For more information, see [Buildspec file name and storage location](build-spec-ref.md#build-spec-ref-name-storage)\.

source/**auth**  <a name="cli.source.auth"></a>
Do not use\. This object is used by the CodeBuild console only\.

source/**reportBuildStatus**  <a name="cli.source.reportbuildstatus"></a>
Specifies whether to send your source provider the status of a build's start and completion\. If you set this with a source provider other than GitHub, GitHub Enterprise Server, or Bitbucket, an `invalidInputException` is thrown\.

source/**buildStatusConfig**  <a name="cli.source.buildstatusconfig"></a>
Contains information that defines how the CodeBuild build project reports the build status to the source provider\. This option is only used when the source type is `GITHUB`, `GITHUB_ENTERPRISE`, or `BITBUCKET`\.    
source/buildStatusConfig/**context**  
For Bitbucket sources, this parameter is used for the `name` parameter in the Bitbucket commit status\. For GitHub sources, this parameter is used for the `context` parameter in the GitHub commit status\.   
For example, you can have the `context` contain the build number and the webhook trigger using the CodeBuild environment variables:  

```
AWS CodeBuild sample-project Build #$CODEBUILD_BUILD_NUMBER - $CODEBUILD_WEBHOOK_TRIGGER
```
This results in the context appearing like this for build \#24 triggered by a webhook pull request event:  

```
AWS CodeBuild sample-project Build #24 - pr/8
```  
source/buildStatusConfig/**targetUrl**  
For Bitbucket sources, this parameter is used for the `url` parameter in the Bitbucket commit status\. For GitHub sources, this parameter is used for the `target_url` parameter in the GitHub commit status\.  
For example, you can set the `targetUrl` to `https://aws.amazon.com/codebuild/` and the commit status will link to this URL\.

source/**gitSubmodulesConfig**  <a name="cli.source.gitsubmodulesconfig"></a>
Optional\. Information about the Git submodules configuration\. Used with CodeCommit, GitHub, GitHub Enterprise Server, and Bitbucket only\.     
source/gitSubmodulesConfig/**fetchSubmodules**  
Set `fetchSubmodules` to `true` if you want to include the Git submodules in your repository\. Git submodules that are included must be configured as HTTPS\.

source/**InsecureSsl**  <a name="cli.source.insecuressl"></a>
Optional\. Used with GitHub Enterprise Server only\. Set this value to `true` to ignore TLS warnings while connecting to your GitHub Enterprise Server project repository\. The default value is `false`\. `InsecureSsl` should be used for testing purposes only\. It should not be used in a production environment\.

source/**sourceIdentifier**  <a name="cli.source.sourceidentifier"></a>
A user\-defined identifier for the project source\. Optional for the primary source\. Required for secondary sources\.

### **secondarySources**<a name="cli.secondarysources"></a>

Optional\. An array of [ProjectSource](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_ProjectSource.html) objects that contain information about the secondary sources for a build project\. You can add up to 12 secondary sources\. The `secondarySources` objects use the same properties used by the [**source**](#cli.source) object\. In a secondary source object, the `sourceIdentifier` is required\.

### **secondarySourceVersions**<a name="cli.secondarysourceversions"></a>

Optional\. An array of [ProjectSourceVersion](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_ProjectSourceVersion.html) objects\. If `secondarySourceVersions` is specified at the build level, then they take precedence over this\. 

### **sourceVersion**<a name="cli.sourceversion"></a>

Optional\. The version of the build input to be built for this project\. If not specified, the latest version is used\. If specified, it must be one of: 
+ For CodeCommit, the commit ID, branch, or Git tag to use\.
+ For GitHub, the commit ID, pull request ID, branch name, or tag name that corresponds to the version of the source code you want to build\. If a pull request ID is specified, it must use the format `pr/pull-request-ID` \(for example `pr/25`\)\. If a branch name is specified, the branch's HEAD commit ID is used\. If not specified, the default branch's HEAD commit ID is used\. 
+ For Bitbucket, the commit ID, branch name, or tag name that corresponds to the version of the source code you want to build\. If a branch name is specified, the branch's HEAD commit ID is used\. If not specified, the default branch's HEAD commit ID is used\. 
+ For Amazon S3, the version ID of the object that represents the build input ZIP file to use\. 

If `sourceVersion` is specified at the build level, then that version takes precedence over this `sourceVersion` \(at the project level\)\. For more information, see [Source version sample with AWS CodeBuild](sample-source-version.md)\. 

### **artifacts**<a name="cli.artifacts"></a>

Required\. A [ProjectArtifiacts](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_ProjectArtifacts.html) object that contains information about this build project's output artifact settings\. After you add an `artifacts` object, you can add up to 12 more artifacts using the [secondaryArtifacts](#cli.secondaryartifacts)\. These settings include the following: 

artifacts/**type**  <a name="cli.artifacts.type"></a>
Required\. The type of build output artifact\. Valid values are:   
+ `CODEPIPELINE`
+ `NO_ARTIFACTS`
+ `S3`

artifacts/**location**  <a name="cli.artifacts.location"></a>
Only used with the `S3` artifact type\. Not used for other artifact types\.  
The name of the output bucket you created or identified in the prerequisites\. 

artifacts/**path**  <a name="cli.artifacts.path"></a>
Only used with the `S3` artifact type\. Not used for other artifact types\.  
The path in of the output bucket to place ZIP file or folder\. If you do not specify a value for `path`, CodeBuild uses `namespaceType` \(if specified\) and `name` to determine the path and name of the build output ZIP file or folder\. For example, if you specify `MyPath` for `path` and `MyArtifact.zip` for `name`, the path and name would be `MyPath/MyArtifact.zip`\. 

artifacts/**namespaceType**  <a name="cli.artifacts.namespacetype"></a>
Only used with the `S3` artifact type\. Not used for other artifact types\.  
The namespace of the build output ZIP file or folder\. Valid values include `BUILD_ID` and `NONE`\. Use `BUILD_ID` to insert the build ID into the path of the build output ZIP file or folder\. Otherwise, use `NONE`\. If you do not specify a value for `namespaceType`, CodeBuild uses `path` \(if specified\) and `name` to determine the path and name of the build output ZIP file or folder\. For example, if you specify `MyPath` for `path`, `BUILD_ID` for `namespaceType`, and `MyArtifact.zip` for `name`, the path and name would be `MyPath/build-ID/MyArtifact.zip`\. 

artifacts/**name**  <a name="cli.artifacts.name"></a>
Only used with the `S3` artifact type\. Not used for other artifact types\.  
The name of the build output ZIP file or folder inside of `location`\. For example, if you specify `MyPath` for `path` and `MyArtifact.zip` for `name`, the path and name would be `MyPath/MyArtifact.zip`\. 

artifacts/**overrideArtifactName**  <a name="cli.artifacts.overrideartifactname"></a>
Only used with the S3 artifact type\. Not used for other artifact types\.  
Optional\. If set to `true`, the name specified in the `artifacts` block of the buildspec file overrides `name`\. For more information, see [Build specification reference for CodeBuild](build-spec-ref.md)\. 

artifacts/**packaging**  <a name="cli.artifacts.packaging"></a>
Only used with the `S3` artifact type\. Not used for other artifact types\.   
Optional\. Specifies how to package the artifacts\. Allowed values are:    
NONE  
Create a folder that contains the build artifacts\. This is the default value\.   
ZIP  
Create a ZIP file that contains the build artifacts\.

### secondaryArtifacts<a name="cli.secondaryartifacts"></a>

Optional\. An array of [ProjectArtifiacts](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_ProjectArtifacts.html) objects that contain information about the secondary artifacts settings for a build project\. You can add up to 12 secondary artifacts\. The `secondaryArtifacts` uses many of the same settings used by the [**artifacts**](#cli.artifacts) object\. 

### cache<a name="cli.cache"></a>

Required\. A [ProjectCache](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_ProjectCache.html) object that contains information about this build project's cache settings\. For more information, see [Build caching](build-caching.md)\. 

### environment<a name="cli.environment"></a>

Required\. A [ProjectEnvironment](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_ProjectEnvironment.html) object that contains information about this project's build environment settings\. These settings include:

environment/**type**  <a name="cli.environment.type"></a>
Required\. The type of build environment\. For more information, see [type](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_ProjectEnvironment.html#CodeBuild-Type-ProjectEnvironment-type) in the *CodeBuild API Reference*\.

environment/**image**  <a name="cli.environment.image"></a>
Required\. The Docker image identifier used by this build environment\. Typically, this identifier is expressed as *image\-name*:*tag*\. For example, in the Docker repository that CodeBuild uses to manage its Docker images, this could be `aws/codebuild/standard:4.0`\. In Docker Hub, `maven:3.3.9-jdk-8`\. In Amazon ECR, `account-id.dkr.ecr.region-id.amazonaws.com/your-Amazon-ECR-repo-name:tag`\. For more information, see [Docker images provided by CodeBuild](build-env-ref-available.md)\. 

environment/**computeType**  <a name="cli.environment.computetype"></a>
Required\. Specifies the compute resources used by this build environment\. For more information, see [computeType](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_ProjectEnvironment.html#CodeBuild-Type-ProjectEnvironment-computeType) in the *CodeBuild API Reference*\.

environment/**certificate**  <a name="cli.environment.certificate"></a>
Optional\. The ARN of the Amazon S3 bucket, path prefix, and object key that contains the PEM\-encoded certificate\. The object key can be either just the \.pem file or a \.zip file containing the PEM\-encoded certificate\. For example, if your Amazon S3 bucket name is `my-bucket`, your path prefix is `cert`, and your object key name is `certificate.pem`, then acceptable formats for `certificate` are `my-bucket/cert/certificate.pem` or `arn:aws:s3:::my-bucket/cert/certificate.pem`\.

environment/**environmentVariables**  <a name="cli.environment.environmentvariables"></a>
Optional\. An array of [EnvironmentVariable](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_EnvironmentVariable.html) objects that contains the environment variables you want to specify for this build environment\. Each environment variable is expressed as an object that contains a `name`, `value`, and `type` of `name`, `value`, and `type`\.   
Console and AWS CLI users can see all environment variables\. If you have no concerns about the visibility of your environment variable, set `name` and `value`, and set `type` to `PLAINTEXT`\.  
We recommend you store environment variables with sensitive values, such as an AWS access key ID, an AWS secret access key, or a password, as a parameter in Amazon EC2 Systems Manager Parameter Store or AWS Secrets Manager\. For `name`, for that stored parameter, set an identifier for CodeBuild to reference\.   
If you use Amazon EC2 Systems Manager Parameter Store, for `value`, set the parameter's name as stored in the Parameter Store\. Set `type` to `PARAMETER_STORE`\. Using a parameter named `/CodeBuild/dockerLoginPassword` as an example, set `name` to `LOGIN_PASSWORD`\. Set `value` to `/CodeBuild/dockerLoginPassword`\. Set `type` to `PARAMETER_STORE`\.   
If you use Amazon EC2 Systems Manager Parameter Store, we recommend that you store parameters with parameter names that start with `/CodeBuild/` \(for example, `/CodeBuild/dockerLoginPassword`\)\. You can use the CodeBuild console to create a parameter in Amazon EC2 Systems Manager\. Choose **Create parameter**, and then follow the instructions in the dialog box\. \(In that dialog box, for **KMS key**, you can specify the ARN of an AWS KMS key in your account\. Amazon EC2 Systems Manager uses this key to encrypt the parameter's value during storage and decrypt it during retrieval\.\) If you use the CodeBuild console to create a parameter, the console starts the parameter name with `/CodeBuild/` as it is being stored\. For more information, see [Systems Manager Parameter Store](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-paramstore.html) and [Systems Manager Parameter Store Console Walkthrough](https://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-paramstore-walk.html#sysman-paramstore-console) in the *Amazon EC2 Systems Manager User Guide*\.  
If your build project refers to parameters stored in Amazon EC2 Systems Manager Parameter Store, the build project's service role must allow the `ssm:GetParameters` action\. If you chose **New service role** earlier, CodeBuild includes this action in the default service role for your build project\. However, if you chose **Existing service role**, you must include this action to your service role separately\.  
If your build project refers to parameters stored in Amazon EC2 Systems Manager Parameter Store with parameter names that do not start with `/CodeBuild/`, and you chose **New service role**, you must update that service role to allow access to parameter names that do not start with `/CodeBuild/`\. This is because that service role allows access only to parameter names that start with `/CodeBuild/`\.  
If you choose **New service role**, the service role includes permission to decrypt all parameters under the `/CodeBuild/` namespace in the Amazon EC2 Systems Manager Parameter Store\.  
Environment variables you set replace existing environment variables\. For example, if the Docker image already contains an environment variable named `MY_VAR` with a value of `my_value`, and you set an environment variable named `MY_VAR` with a value of `other_value`, then `my_value` is replaced by `other_value`\. Similarly, if the Docker image already contains an environment variable named `PATH` with a value of `/usr/local/sbin:/usr/local/bin`, and you set an environment variable named `PATH` with a value of `$PATH:/usr/share/ant/bin`, then `/usr/local/sbin:/usr/local/bin` is replaced by the literal value `$PATH:/usr/share/ant/bin`\.  
Do not set any environment variable with a name that begins with `CODEBUILD_`\. This prefix is reserved for internal use\.  
If an environment variable with the same name is defined in multiple places, the value is determined as follows:  
+ The value in the start build operation call takes highest precedence\.
+ The value in the build project definition takes next precedence\.
+ The value in the buildspec declaration takes lowest precedence\.
If you use Secrets Manager, for `value`, set the parameter's name as stored in Secrets Manager\. Set `type` to `SECRETS_MANAGER`\. Using a secret named `/CodeBuild/dockerLoginPassword` as an example, set `name` to `LOGIN_PASSWORD`\. Set `value` to `/CodeBuild/dockerLoginPassword`\. Set `type` to `SECRETS_MANAGER`\.  
If you use Secrets Manager, we recommend that you store secrets with names that start with `/CodeBuild/` \(for example, `/CodeBuild/dockerLoginPassword`\)\. For more information, see [What Is AWS Secrets Manager?](https://docs.aws.amazon.com/secretsmanager/latest/userguide/intro.html) in the *AWS Secrets Manager User Guide*\.   
If your build project refers to secrets stored in Secrets Manager, the build project's service role must allow the `secretsmanager:GetSecretValue` action\. If you chose **New service role** earlier, CodeBuild includes this action in the default service role for your build project\. However, if you chose **Existing service role**, you must include this action to your service role separately\.   
If your build project refers to secrets stored in Secrets Manager with secret names that do not start with `/CodeBuild/`, and you chose **New service role**, you must update the service role to allow access to secret names that do not start with `/CodeBuild/`\. This is because the service role allows access only to secret names that start with `/CodeBuild/`\.  
If you choose **New service role**, the service role includes permission to decrypt all secrets under the `/CodeBuild/` namespace in the Secrets Manager\.

environment/**registryCredential**  <a name="cli.environment.registrycredential"></a>
Optional\. A [RegistryCredential](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_RegistryCredential.html) object that specifies the credentials that provide access to a private Docker registry\.     
environment/registryCredential/**credential**  
Specifies the ARN or name of credentials created using AWS Managed Services \. You can use the name of the credentials only if they exist in your current Region\.  
environment/registryCredential/**credentialProvider**  
The only valid value is `SECRETS_MANAGER`\.
When this is set:   
+ `imagePullCredentials` must be set to `SERVICE_ROLE`\.
+ The image cannot be a curated image or an Amazon ECR image\.

environment/**imagePullCredentialsType**  <a name="cli.environment.imagepullcredentialstype"></a>
Optional\. The type of credentials CodeBuild uses to pull images in your build\. There are two valid values:    
CODEBUILD  
`CODEBUILD` specifies that CodeBuild uses its own credentials\. You must edit your Amazon ECR repository policy to trust the CodeBuild service principal\.   
SERVICE\_ROLE  
Specifies that CodeBuild uses your build project's service role\. 
When you use a cross\-account or private registry image, you must use `SERVICE_ROLE` credentials\. When you use a CodeBuild curated image, you must use `CODEBUILD` credentials\. 

environment/**privilegedMode**  <a name="cli.environment.privilegedmode"></a>
Set to `true` only if you plan to use this build project to build Docker images, and the build environment image you specified is not provided by CodeBuild with Docker support\. Otherwise, all associated builds that attempt to interact with the Docker daemon fail\. You must also start the Docker daemon so that your builds can interact with it\. One way to do this is to initialize the Docker daemon in the `install` phase of your buildspec file by running the following build commands\. Do not run these commands if you specified a build environment image provided by CodeBuild with Docker support\.  
By default, Docker containers do not allow access to any devices\. Privileged mode grants a build project's Docker container access to all devices\. For more information, see [Runtime Privilege and Linux Capabilities](https://docs.docker.com/engine/reference/run/#runtime-privilege-and-linux-capabilities) on the Docker Docs website\.

```
- nohup /usr/local/bin/dockerd --host=unix:///var/run/docker.sock --host=tcp://127.0.0.1:2375 --storage-driver=overlay2
- timeout -t 15 sh -c "until docker info; do echo .; sleep 1; done"
```

### serviceRole<a name="cli.servicerole"></a>

Required\. The ARN of the service role CodeBuild uses to interact with services on behalf of the IAM user \(for example, `arn:aws:iam::account-id:role/role-name`\)\.

### timeoutInMinutes<a name="cli.timeoutinminutes"></a>

Optional\. The number of minutes, between 5 to 480 \(8 hours\), after which CodeBuild stops the build if it is not complete\. If not specified, the default of 60 is used\. To determine if and when CodeBuild stopped a build due to a timeout, run the `batch-get-builds` command\. To determine if the build has stopped, look in the output for a `buildStatus` value of `FAILED`\. To determine when the build timed out, look in the output for the `endTime` value associated with a `phaseStatus` value of `TIMED_OUT`\. 

### queuedTimeoutInMinutes<a name="cli.queuedtimeoutinminutes"></a>

Optional\. The number of minutes, between 5 to 480 \(8 hours\), after which CodeBuild stops the build if it is is still queued\. If not specified, the default of 60 is used\. 

### encryptionKey<a name="cli.encryptionkey"></a>

Optional\. The alias or ARN of the AWS KMS customer managed key \(CMK\) used by CodeBuild to encrypt the build output\. If you specify an alias, use the format `arn:aws:kms:region-ID:account-ID:key/key-ID` or, if an alias exists, use the format `alias/key-alias`\. If not specified, the AWS\-managed CMK for Amazon S3 is used\.

### tags<a name="cli.tags"></a>

Optional\. An array of [Tag](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_Tag.html) objects that provide the tags you want to associate with this build project\. You can specify up to 50 tags\. These tags can be used by any AWS service that supports CodeBuild build project tags\. Each tag is expressed as an object with a `key` and a `value`\.

### vpcConfig<a name="cli.vpcconfig"></a>

Optional\. A [VpcConfig](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_VpcConfig.html) object that contains information information about the VPC configuration for you rproject\. These properties include: 

vpcId  
Required\. The VPC ID that CodeBuild uses\. Run this command to get a list of all VPC IDs in your Region:  

```
aws ec2 describe-vpcs --region <region-ID>
```

subnets  
Required\. An array of subnet IDs that include resources used by CodeBuild\. Run this command to get these IDs:  

```
aws ec2 describe-subnets --filters "Name=vpc-id,Values=<vpc-id>" --region <region-ID>
```

securityGroupIds  
Required\. An array of security group IDs used by CodeBuild to allow access to resources in the VPC\. Run this command to get these IDs:  

```
aws ec2 describe-security-groups --filters "Name=vpc-id,Values=<vpc-id>" --<region-ID>
```

### badgeEnabled<a name="cli.badgeenabled"></a>

Optional\. Specifies whener to include build badges with your CodeBuild project\. Set to `true` to enable build baddes, or `false` otehrwise\. For more information, see [Build badges sample with CodeBuild](sample-build-badges.md)\.

### logsConfig<a name="cli.logsconfig"></a>

A [LogsConfig](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_LogsConfig.html) object that contains information about where this build's logs are located\.

logsConfig/**cloudWatchLogs**  <a name="cli.logsconfig.cloudwatchlogs"></a>
A [CloudWatchLogsConfig](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_CloudWatchLogsConfig.html) object that contains information about pushing logs to CloudWatch Logs\.

logsConfig/**s3Logs**  <a name="cli.logsconfig.s3logs"></a>
An [S3LogsConfig](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_S3LogsConfig.html) object that contains information about pushing logs to Amazon S3\.

### fileSystemLocations<a name="cli.filesystemlocations"></a>

Optional\. An array of [ProjectFileSystemsLocation](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_ProjectFileSystemLocation.html) objects that contains informationabout your Amazon EFS configuration\. 

### buildBatchConfig<a name="cli.buildbatchconfig"></a>

Optional\. The `buildBatchConfig` object is a [ProjectBuildBatchConfig](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_ProjectBuildBatchConfig.html) structure that contains the batch build configuration information for the project\.

buildBatchConfig/**serviceRole**  
The service role ARN for the batch build project\.

buildBatchConfig/**combineArtifacts**  
A Boolean value that specifies whether to combine the build artifacts for the batch build into a single artifact location\.

buildBatchConfig/restrictions/**maximumBuildsAllowed**  
The maximum number of builds allowed\.

buildBatchConfig/restrictions/**computeTypesAllowed**  
An array of strings that specify the compute types that are allowed for the batch build\. See [Build environment compute types](https://docs.aws.amazon.com/codebuild/latest/userguide/build-env-ref-compute-types.html) for these values\. 

buildBatchConfig/**timeoutInMinutes**  
The maximum amount of time, in minutes, that the batch build must be completed in\.

## Create the project<a name="cp-cli-create-project"></a>

To create the project, run the [https://docs.aws.amazon.com/cli/latest/reference/codebuild/create-project.html](https://docs.aws.amazon.com/cli/latest/reference/codebuild/create-project.html) command again, passing your JSON file:

```
aws codebuild create-project --cli-input-json file://<json-file>
```

If successful, the JSON representation of a [Project](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_Project.html) object appears in the console output\. See the [CreateProject Response Syntax](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_CreateProject.html#API_CreateProject_ResponseSyntax) for an example of this data\.

Except for the build project name, you can change any of the build project's settings later\. For more information, see [Change a build project's settings \(AWS CLI\)](change-project-cli.md)\.

To start running a build, see [Run a build \(AWS CLI\)](run-build-cli.md)\.

If your source code is stored in a GitHub repository, and you want CodeBuild to rebuild the source code every time a code change is pushed to the repository, see [Start running builds automatically \(AWS CLI\)](run-build-cli-auto-start.md)\.