# Create a build project \(console\)<a name="create-project-console"></a>

1. Open the AWS CodeBuild console at [https://console\.aws\.amazon\.com/codesuite/codebuild/home](https://console.aws.amazon.com/codesuite/codebuild/home)\.

1.  If a CodeBuild information page is displayed, choose **Create build project**\. Otherwise, on the navigation pane, expand **Build**, choose **Build projects**, and then choose **Create build project**\. 

1.  Choose **Create build project**\. 

1. In **Project configuration**:

   On the **Create build project** page, in **Project configuration**, enter a name for this build project\. Build project names must be unique across each AWS account\. You can also include an optional description of the build project to help other users understand what this project is used for\.

    In **Description**, enter an optional description for your project\. 

   Select **Build badge** to make your project's build status visible and embeddable\. For more information, see [Build badges sample](sample-build-badges.md)\.
**Note**  
 Build badge does not apply if your source provider is Amazon S3\. 

   Expand **Additional configuration**\.

    \(Optional\) For **Tags**, enter the name and value of any tags that you want supporting AWS services to use\. Use **Add row** to add a tag\. You can add up to 50 tags\. 

1. In **Source**:

   For **Source provider**, choose the source code provider type\. Use the following lists to make selections appropriate for your source provider:
**Note**  
CodeBuild does not support Bitbucket Server\.

------
#### [ Amazon S3 ]

 **Bucket**   
Choose the name of the input bucket that contains the source code\. 

 **S3 object key or S3 folder**   
Enter the name of the ZIP file or the path to the folder that contains the source code\. Enter a forward slash \(/\) to download everything in the S3 bucket\. 

 **Source version**   
Enter the version ID of the object that represents the build of your input file\. For more information, see[Source version sample with AWS CodeBuild](sample-source-version.md)\. 

------
#### [ CodeCommit ]

 **Repository**   
Choose the repository you want to use\.

**Reference type**  
Choose **Branch**, **Git tag**, or **Commit ID** to specify the version of your source code\. For more information, see [Source version sample with AWS CodeBuild](sample-source-version.md)\.

 **Git clone depth**   
Choose to create a shallow clone with a history truncated to the specified number of commits\. If you want a full clone, choose **Full**\. 

 **Use Git submodules**   
Select if you want to include Git submodules in your repository\. 

------
#### [ Bitbucket ]

 **Repository**   
Choose **Connect using OAuth** or **Connect with a Bitbucket app password ** and follow the instructions to connect \(or reconnect\) to Bitbucket\.   
Choose a public repository or a repository in your account\.

 **Source version**   
Enter a branch, commit ID, tag, or reference and a commit ID\. For more information, see [Source version sample with AWS CodeBuild](sample-source-version.md) 

 **Git clone depth**   
Choose **Git clone depth** to create a shallow clone with a history truncated to the specified number of commits\. If you want a full clone, choose **Full**\. 

 **Use Git submodules**   
Select if you want to include Git submodules in your repository\. 

   Select **Report build statuses to source provider when your builds start and finish ** if you want the status of your build's start and completion reported to your source provider\. 

**Note**  
The status of a build triggered by a webhook is always reported to your source provider\. 

   Select **Rebuild every time a code change is pushed to this repository ** if you want CodeBuild to build the source code every time a code change is pushed to this repository\. Webhooks are allowed only with your own Bitbucket, GitHub, or GitHub Enterprise repository\. 

   For **Status context**, enter the value to be used for the `name` parameter in the Bitbucket commit status\. For more information, see [build](https://developer.atlassian.com/bitbucket/api/2/reference/resource/repositories/%7Bworkspace%7D/%7Brepo_slug%7D/commit/%7Bnode%7D/statuses/build) in the Bitbucket API documentation\.

   For **Target URL**, enter the value to be used for the `url` parameter in the Bitbucket commit status\. For more information, see [build](https://developer.atlassian.com/bitbucket/api/2/reference/resource/repositories/%7Bworkspace%7D/%7Brepo_slug%7D/commit/%7Bnode%7D/statuses/build) in the Bitbucket API documentation\.

   If you chose **Rebuild every time a code change is pushed to this repository**, in **Event type**, choose an event that you want to trigger a build\. You use regular expressions to create a filter\. If no filter is specified, all update and create pull requests, and all push events, trigger a build\. For more information, see [ Filter GitHub webhook events](sample-github-pull-request.md#sample-github-pull-request-filter-webhook-events) and [ Filter Bitbucket webhook events](sample-bitbucket-pull-request.md#sample-bitbucket-pull-request-filter-webhook-events)\. 

------
#### [ GitHub ]

 **Repository**   
Choose **Connect using OAuth** or **Connect with a GitHub personal access token ** and follow the instructions to connect \(or reconnect\) to GitHub and authorize access to AWS CodeBuild\.   
Choose a public repository or a repository in your account\.

 **Source version**   
Enter a branch, commit ID, tag, or reference and a commit ID\. For more information, see [Source version sample with AWS CodeBuild](sample-source-version.md) 

 **Git clone depth**   
Choose **Git clone depth** to create a shallow clone with a history truncated to the specified number of commits\. If you want a full clone, choose **Full**\. 

 **Use Git submodules**   
Select if you want to include Git submodules in your repository\. 

   Select **Report build statuses to source provider when your builds start and finish ** if you want the status of your build's start and completion reported to your source provider\. 

**Note**  
The status of a build triggered by a webhook is always reported to your source provider\. 

   Select **Rebuild every time a code change is pushed to this repository ** if you want CodeBuild to build the source code every time a code change is pushed to this repository\. Webhooks are allowed only with your own Bitbucket, GitHub, or GitHub Enterprise repository\. 

   For **Status context**, enter the value to be used for the `context` parameter in the GitHub commit status\. For more information, see [Create a commit status](https://developer.github.com/v3/repos/statuses/#create-a-commit-status) in the GitHub developer guide\.

   For **Target URL**, enter the value to be used for the `target_url` parameter in the GitHub commit status\. For more information, see [Create a commit status](https://developer.github.com/v3/repos/statuses/#create-a-commit-status) in the GitHub developer guide\.

   If you chose **Rebuild every time a code change is pushed to this repository**, in **Event type**, choose an event that you want to trigger a build\. You use regular expressions to create a filter\. If no filter is specified, all update and create pull requests, and all push events, trigger a build\. For more information, see [ Filter GitHub webhook events](sample-github-pull-request.md#sample-github-pull-request-filter-webhook-events) and [ Filter Bitbucket webhook events](sample-bitbucket-pull-request.md#sample-bitbucket-pull-request-filter-webhook-events)\. 

------
#### [ GitHub Enterprise Server ]

 **GitHub Enterprise personal access token**   
See [GitHub Enterprise Server sample](sample-github-enterprise.md) for information about how to copy a personal access token to your clipboard\. Paste the token in the text field, and then choose **Save Token**\.   
You only need to enter and save the personal access token once\. CodeBuild uses this token in all future projects\. 

 **Source version**   
Enter a pull request, branch, commit ID, tag, or reference and a commit ID\. For more information, see [Source version sample with AWS CodeBuild](sample-source-version.md)\. 

 **Git clone depth**   
Choose **Git clone depth** to create a shallow clone with a history truncated to the specified number of commits\. If you want a full clone, choose **Full**\. 

 **Use Git submodules**   
Select if you want to include Git submodules in your repository\. 

 **Build status**   
Select **Report build statuses to source provider when your builds start and finish ** if you want the status of your build's start and completion reported to your source provider\.   
The status of a build triggered by a webhook is always reported to your source provider\. 

 **Insecure SSL**   
Choose to ignore SSL warnings while connecting to your GitHub Enterprise project repository\. 

   Select **Rebuild every time a code change is pushed to this repository ** if you want CodeBuild to build the source code every time a code change is pushed to this repository\. Webhooks are allowed only with your own Bitbucket, GitHub, or GitHub Enterprise repository\. 

   For **Status context**, enter the value to be used for the `context` parameter in the GitHub commit status\. For more information, see [Create a commit status](https://developer.github.com/v3/repos/statuses/#create-a-commit-status) in the GitHub developer guide\.

   For **Target URL**, enter the value to be used for the `target_url` parameter in the GitHub commit status\. For more information, see [Create a commit status](https://developer.github.com/v3/repos/statuses/#create-a-commit-status) in the GitHub developer guide\.

   If you chose **Rebuild every time a code change is pushed to this repository**, in **Event type**, choose an event that you want to trigger a build\. You use regular expressions to create a filter\. If no filter is specified, all update and create pull requests, and all push events, trigger a build\. For more information, see [ Filter GitHub webhook events](sample-github-pull-request.md#sample-github-pull-request-filter-webhook-events) and [ Filter Bitbucket webhook events](sample-bitbucket-pull-request.md#sample-bitbucket-pull-request-filter-webhook-events)\. 

------

   For each secondary source you want:

   1.  Choose **Add source**\. 

   1.  For **Source identifier**, enter a value that is fewer than 128 characters and contains only alphanumeric characters and underscores\.

   1.  For **Source provider**, choose the source code provider type\. Use the table earlier in this step to make selections appropriate for your secondary source provider\. 

1. In **Environment**:

   For **Environment image**, do one of the following:
   + To use a Docker image managed by AWS CodeBuild, choose **Managed image**, and then make selections from **Operating system**, **Runtime\(s\)**, **Image**, and **Image version**\. Make a selection from **Environment type** if it is available\.
   + To use another Docker image, choose **Custom image**\. For **Environment type**, choose **ARM**, **Linux**, **Linux GPU**, or **Windows**\. If you choose **Other registry**, for **External registry URL**, enter the name and tag of the Docker image in Docker Hub, using the format `docker repository/docker image name`\. If you choose **Amazon ECR**, use **Amazon ECR repository** and **Amazon ECR image** to choose the Docker image in your AWS account\.
   + To use private Docker image, choose **Custom image**\. For **Environment type**, choose **ARM**, **Linux**, **Linux GPU**, or **Windows**\. For **Image registry**, choose **Other registry**, and then enter the ARN of the credentials for your private Docker image\. The credentials must be created by Secrets Manager\. For more information, see [What Is AWS Secrets Manager?](https://docs.aws.amazon.com/secretsmanager/latest/userguide/) in the *AWS Secrets Manager User Guide*\.

   \(Optional\) Select **Privileged** only if you plan to use this build project to build Docker images, and the build environment image you chose is not provided by CodeBuild with Docker support\. Otherwise, all associated builds that attempt to interact with the Docker daemon fail\. You must also start the Docker daemon so that your builds can interact with it\. One way to do this is to initialize the Docker daemon in the `install` phase of your build spec by running the following build commands\. Do not run these commands if you chose a build environment image provided by CodeBuild with Docker support\.
**Note**  
By default, Docker containers do not allow access to any devices\. Privileged mode grants a build project's Docker container access to all devices\. For more information, see [Runtime Privilege and Linux Capabilities](https://docs.docker.com/engine/reference/run/#runtime-privilege-and-linux-capabilities) on the Docker Docs website\.

   ```
   - nohup /usr/local/bin/dockerd --host=unix:///var/run/docker.sock --host=tcp://127.0.0.1:2375 --storage-driver=overlay&
   - timeout -t 15 sh -c "until docker info; do echo .; sleep 1; done"
   ```

   In **Service role**, do one of the following:
   + If you do not have a CodeBuild service role, choose **New service role**\. In **Role name**, enter a name for the new role\.
   + If you have a CodeBuild service role, choose **Existing service role**\. In **Role ARN**, choose the service role\.
**Note**  
When you use the console to create or update a build project, you can create a CodeBuild service role at the same time\. By default, the role works with that build project only\. If you use the console to associate this service role with another build project, the role is updated to work with the other build project\. A service role can work with up to 10 build projects\.

   Expand **Additional configuration**\.

   \(Optional\) For **Timeout**, specify a value between 5 minutes and 480 minutes \(8 hours\) after which CodeBuild stops the build if it is not complete\. If **hours** and **minutes** are left blank, the default value of 60 minutes is used\.

   If you want CodeBuild to work with your VPC:
   + For **VPC**, choose the VPC ID that CodeBuild uses\.
   + For **VPC Subnets**, choose the subnets that include resources that CodeBuild uses\.
   + For **VPC Security groups**, choose the security groups that CodeBuild uses to allow access to resources in the VPCs\.

   For more information, see [Use AWS CodeBuild with Amazon Virtual Private Cloud](vpc-support.md)\.

   If you want to use one or more elastic file systems:
   +  For **Identifier**, enter a unique file system identifier\. It must be fewer than 129 characters and contain only alphanumeric characters and underscores\. CodeBuild uses this identifier to create an environment variable that identifies the elastic file system\. The environment variable format is `CODEBUILD_file-system-identifier` in capital letters\. For example, if you enter **efs\-1**, the environment variable is `CODEBUILD_EFS-1`\. 
   +  For **ID**, choose the file system ID\. 
   +  \(Optional\) Enter a directory in the file system\. CodeBuild mounts this directory\. If you leave **Directory path** blank, CodeBuild mounts the entire file system\. The path is relative to the root of the file system\. 
   +  For **Mount point**, enter the absolute path of the directory in your build container where the file system is mounted\. If this directory does not exist, CodeBuild creates it during the build\. 
   +  \(Optional\) Enter mount options\. If you leave **Mount options** blank, CodeBuild uses its default mount options \(`nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2`\)\. For more information, see [Recommended NFS Mount Options](https://docs.aws.amazon.com/efs/latest/ug/mounting-fs-nfs-mount-settings.html) in the *Amazon Elastic File System User Guide*\. 

   For **Compute**, choose one of the available options\.

   For **Environment variables**, enter the name and value, and then choose the type of each environment variable for builds to use\. 
**Note**  
CodeBuild sets the environment variable for your AWS Region automatically\. You must set the following environment variables if you haven't added them to your buildspec\.yml:  
AWS\_ACCOUNT\_ID
IMAGE\_REPO\_NAME
IMAGE\_TAG

   Console and AWS CLI users can see environment variables\. If you have no concerns about the visibility of your environment variable, set the **Name** and **Value** fields, and then set **Type** to **Plaintext**\.

   We recommend that you store an environment variable with a sensitive value, such as an AWS access key ID, an AWS secret access key, or a password as a parameter in Amazon EC2 Systems Manager Parameter Store or AWS Secrets Manager\. 

   If you use Amazon EC2 Systems Manager Parameter Store, then for **Type**, choose **Parameter**\. For **Name**, enter an identifier for CodeBuild to reference\. For **Value**, enter the parameter's name as stored in Amazon EC2 Systems Manager Parameter Store\. Using a parameter named `/CodeBuild/dockerLoginPassword` as an example, for **Type**, choose **Parameter**\. For **Name**, enter `LOGIN_PASSWORD`\. For **Value**, enter `/CodeBuild/dockerLoginPassword`\. 
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

   If you use Secrets Manager, for **Type**, choose **Secrets Manager**\. For **Name**, enter an identifier for CodeBuild to reference\. For **Value**, enter a `reference-key` using the pattern `secret-id:json-key:version-stage:version-id`\. For information, see [Secrets Manager reference-key in the buildspec file](build-spec-ref.md#secrets-manager-build-spec)\.
**Important**  
If you use Secrets Manager, we recommend that you store secrets with names that start with `/CodeBuild/` \(for example, `/CodeBuild/dockerLoginPassword`\)\. For more information, see [What Is AWS Secrets Manager?](https://docs.aws.amazon.com/secretsmanager/latest/userguide/intro.html) in the *AWS Secrets Manager User Guide*\.   
If your build project refers to secrets stored in Secrets Manager, the build project's service role must allow the `secretsmanager:GetSecretValue` action\. If you chose **New service role** earlier, CodeBuild includes this action in the default service role for your build project\. However, if you chose **Existing service role**, you must include this action to your service role separately\.   
If your build project refers to secrets stored in Secrets Manager with secret names that do not start with `/CodeBuild/`, and you chose **New service role**, you must update the service role to allow access to secret names that do not start with `/CodeBuild/`\. This is because the service role allows access only to secret names that start with `/CodeBuild/`\.  
If you choose **New service role**, the service role includes permission to decrypt all secrets under the `/CodeBuild/` namespace in the Secrets Manager\.

1. In **Buildspec**:

   For **Build specifications**, do one of the following:
   + If your source code includes a buildspec file, choose **Use a buildspec file**\. By default, CodeBuild looks for a file named `buildspec.yml` in the source code root directory\. If your buildspec file uses a different name or location, enter its path from the source root in **Buildspec name** \(for example, `buildspec-two.yml` or `configuration/buildspec.yml`\. If the buildspec file is in an S3 bucket, it must be in the same AWS Region as your build project\. Specify the buildspec file using its ARN \(for example, `arn:aws:s3:::my-codebuild-sample2/buildspec.yml`\)\.
   + If your source code does not include a buildspec file, or if you want to run build commands different from the ones specified for the `build` phase in the `buildspec.yml` file in the source code's root directory, choose **Insert build commands**\. For **Build commands**, enter the commands you want to run in the `build` phase\. For multiple commands, separate each command by `&&` \(for example, `mvn test && mvn package`\)\. To run commands in other phases, or if you have a long list of commands for the `build` phase, add a `buildspec.yml` file to the source code root directory, add the commands to the file, and then choose **Use the buildspec\.yml in the source code root directory**\.

   For more information, see the [Buildspec reference](build-spec-ref.md)\.

1. In **Artifacts**:

   For **Type**, do one of the following:
   + If you do not want to create any build output artifacts, choose **No artifacts**\. You might want to do this if you're only running build tests or you want to push a Docker image to an Amazon ECR repository\.
   + To store the build output in an S3 bucket, choose **Amazon S3**, and then do the following:
     + If you want to use your project name for the build output ZIP file or folder, leave **Name** blank\. Otherwise, enter the name\. \(If you want to output a ZIP file, and you want the ZIP file to have a file extension, be sure to include it after the ZIP file name\.\)
     + Select **Enable semantic versioning** if you want a name specified in the buildspec file to override any name that is specified in the console\. The name in a buildspec file is calculated at build time and uses the Shell command language\. For example, you can append a date and time to your artifact name so that it is always unique\. Unique artifact names prevent artifacts from being overwritten\. For more information, see [Buildspec syntax](build-spec-ref.md#build-spec-ref-syntax)\.
     + For **Bucket name**, choose the name of the output bucket\.
     + If you chose **Insert build commands** earlier in this procedure, then for **Output files**, enter the locations of the files from the build that you want to put into the build output ZIP file or folder\. For multiple locations, separate each location with a comma \(for example, `appspec.yml, target/my-app.jar`\)\. For more information, see the description of `files` in [Buildspec syntax](build-spec-ref.md#build-spec-ref-syntax)\.
     + If you do not want your build artifacts encrypted, select **Remove artifacts encryption**\.

   For each secondary set of artifacts you want:

   1. For **Artifact identifier**, enter a value that is fewer than 128 characters and contains only alphanumeric characters and underscores\.

   1. Choose **Add artifact**\.

   1. Follow the previous steps to configure your secondary artifacts\.

   1. Choose **Save artifact**\.

   Expand **Additional configuration**\.<a name="encryptionkey-console"></a>

   \(Optional\) For **Encryption key**, do one of the following:
   + To use the AWS\-managed customer managed key \(CMK\) for Amazon S3 in your account to encrypt the build output artifacts, leave **Encryption key** blank\. This is the default\.
   + To use a customer\-managed CMK to encrypt the build output artifacts, in **Encryption key**, enter the ARN of the CMK\. Use the format `arn:aws:kms:region-ID:account-ID:key/key-ID`\.

   For **Cache type**, choose one of the following:
   + If you do not want to use a cache, choose **No cache**\.
   + If you want to use an Amazon S3 cache, choose **Amazon S3**, and then do the following:
     + For **Bucket**, choose the name of the S3 bucket where the cache is stored\.
     + \(Optional\) For **Cache path prefix**, enter an Amazon S3 path prefix\. The **Cache path prefix** value is similar to a directory name\. It makes it possible for you to store the cache under the same directory in a bucket\. 
**Important**  
Do not append a trailing slash \(/\) to the end of the path prefix\.
   +  If you want to use a local cache, choose **Local**, and then choose one or more local cache modes\. 
**Note**  
Docker layer cache mode is available for Linux only\. If you choose it, your project must run in privileged mode\. The `ARM_CONTAINER` and `LINUX_GPU_CONTAINER` environment types and the `BUILD_GENERAL1_2XLARGE` compute type do not support the use of a local cache\.

   Using a cache saves considerable build time because reusable pieces of the build environment are stored in the cache and used across builds\. For information about specifying a cache in the buildspec file, see [Buildspec syntax](build-spec-ref.md#build-spec-ref-syntax)\. For more information about caching, see [Build caching in AWS CodeBuild](build-caching.md)\. 

1. In **Logs**, choose the logs you want to create\. You can create Amazon CloudWatch Logs, Amazon S3 logs, or both\. 

   If you want Amazon CloudWatch Logs logs:
   +  Select **CloudWatch logs**\. 
   +  In **Group name**, enter the name of your Amazon CloudWatch Logs log group\. 
   +  In **Stream name**, enter your Amazon CloudWatch Logs log stream name\. 

    If you want Amazon S3 logs: 
   +  Select **S3 logs**\. 
   +  From **Bucket**, choose the name of the S3 bucket for your logs\. 
   +  In **Path prefix**, enter the prefix for your logs\. 

   \(Optional\) If you chose **Amazon S3** for **Type** in **Artifacts** earlier in this procedure, then for **Artifacts packaging**, do one of the following:
   + To have CodeBuild create a ZIP file that contains the build output, choose **Zip**\.
   + To have CodeBuild create a folder that contains the build output, choose **None**\. \(This is the default\.\)
   +  Select **Remove S3 log encryption** if you do not want your S3 logs encrypted\. 

1. Choose **Create build project**\.

1. On the **Review** page, choose **Start build**\.