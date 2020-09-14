# Change a build project's settings \(console\)<a name="change-project-console"></a>

1. Open the AWS CodeBuild console at [https://console\.aws\.amazon\.com/codesuite/codebuild/home](https://console.aws.amazon.com/codesuite/codebuild/home)\.

1. In the navigation pane, choose **Build projects**\.

1. Do one of the following:
   + Choose the link for the build project you want to change, and then choose **Build details**\.
   + Choose the button next to the build project you want to change, choose **View details**, and then choose **Build details**\.

1. To change the project's description, in **Project configuration**, choose **Edit**, and then enter a description\.

   Choose **Update configuration**\.

   For more information about settings referred to in this procedure, see [Create a build project \(console\)](create-project-console.md)\.

1. To change information about the source code location, in **Source**, choose **Edit**\. Use the following lists to make selections appropriate for your source provider, and then choose **Update source**\.
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

   If you chose **Rebuild every time a code change is pushed to this repository**, in **Event type**, choose an event that you want to trigger a build\. You use regular expressions to create a filter\. If no filter is specified, all update and create pull requests, and all push events, trigger a build\. For more information, see [GitHub webhook events](github-webhook.md) and [Bitbucket webhook events](bitbucket-webhook.md)\. 

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

   If you chose **Rebuild every time a code change is pushed to this repository**, in **Event type**, choose an event that you want to trigger a build\. You use regular expressions to create a filter\. If no filter is specified, all update and create pull requests, and all push events, trigger a build\. For more information, see [GitHub webhook events](github-webhook.md) and [Bitbucket webhook events](bitbucket-webhook.md)\. 

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

   If you chose **Rebuild every time a code change is pushed to this repository**, in **Event type**, choose an event that you want to trigger a build\. You use regular expressions to create a filter\. If no filter is specified, all update and create pull requests, and all push events, trigger a build\. For more information, see [GitHub webhook events](github-webhook.md) and [Bitbucket webhook events](bitbucket-webhook.md)\. 

------

   To change whether CodeBuild can modify the service role you use for this project, select or clear **Allow AWS CodeBuild to modify this service role so it can be used with this build project**\. If you clear it, you must use a service role with CodeBuild permissions attached to it\. For more information, see [Add CodeBuild access permissions to an IAM group or IAM user](setting-up.md#setting-up-service-permissions-group) and [Create a CodeBuild service role](setting-up.md#setting-up-service-role)\. 

1. To change information about the build environment, in **Environment**, choose **Edit**\. Make changes appropriate for the build environment type \(for example, **Environment image**, **Operating system**, **Runtime**, **Runtime version**, **Custom image**, **Other location**, **Amazon ECR repository**, or **Amazon ECR image**\)\.

1. If you plan to use this build project to build Docker images and the specified build environment is not provided by CodeBuild with Docker support, select **Privileged**\. Otherwise, all associated builds that attempt to interact with the Docker daemon fail\. You must also start the Docker daemon so that your builds can interact with it as needed\. You can do this by by running the following build commands to initialize the Docker daemon in the `install` phase of your buildspec file\. \(Do not run the following build commands if the specified build environment image is provided by CodeBuild with Docker support\.\)
**Note**  
By default, Docker containers do not allow access to any devices\. Privileged mode grants a build project's Docker container access to all devices\. For more information, see [Runtime Privilege and Linux Capabilities](https://docs.docker.com/engine/reference/run/#runtime-privilege-and-linux-capabilities) on the Docker Docs website\.

   ```
   - nohup /usr/local/bin/dockerd --host=unix:///var/run/docker.sock --host=tcp://127.0.0.1:2375 --storage-driver=overlay2
   - timeout -t 15 sh -c "until docker info; do echo .; sleep 1; done"
   ```

1. To change information about the CodeBuild service role, in **Service role**, change the values for **New service role**, **Existing service role**, or **Role name**\.
**Note**  
When you use the console to create or update a build project, you can create a CodeBuild service role at the same time\. By default, the role works with that build project only\. If you use the console to associate this service role with another build project, the role is updated to work with the other build project\. A service role can work with up to 10 build projects\.

1. To change information about the build timeout, in **Additional configuration**, for **Timeout**, change the values for **hours** and **minutes**\. If **hours** and **minutes** are left blank, the default value is 60 minutes\.

1. To change information about the VPC you created in Amazon VPC, in **Additional configuration**, change the values for **VPC**, **Subnets**, and **Security groups**\. 

1. To change information about a file system you created in Amazon EFS, in **Additional configuration**, change its values for **Identifier**, **ID**, **Directory path**, **Mount point**, and **Mount options**\. For more information, see [Amazon Elastic File System sample for AWS CodeBuild](sample-efs.md)\. 

1. To change the amount of memory and vCPUs that are used to run builds, in **Additional configuration**, change the value for **Compute**\.

1. To change information about environment variables you want builds to use, in **Additional configuration**, for **Environment variables**, change the values for **Name**, **Value**, and **Type**\. Use **Add environment variable** to add an environment variable\. Choose **Remove** next to an environment variable you no longer want to use\.

   Others can see environment variables by using the CodeBuild console and the AWS CLI\. If you have no concerns about the visibility of your environment variable, set the **Name** and **Value** fields, and then set **Type** to **Plaintext**\.

   We recommend that you store an environment variable with a sensitive value, such as an AWS access key ID, an AWS secret access key, or a password as a parameter in Amazon EC2 Systems Manager Parameter Store or AWS Secrets Manager\.

   If you use Amazon EC2 Systems Manager Parameter Store, then for **Type**, choose **Parameter**\. For **Name**, enter an identifier for CodeBuild to reference\. For **Value**, enter the parameter's name as stored in Amazon EC2 Systems Manager Parameter Store\. Using a parameter named `/CodeBuild/dockerLoginPassword` as an example, for **Type**, choose **Parameter**\. For **Name**, enter `LOGIN_PASSWORD`\. For **Value**, type `/CodeBuild/dockerLoginPassword`\. 
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

1. Choose **Update environment**\.

1. To change the project's build specifications, in **Buildspec**, choose **Edit**\. By default, CodeBuild looks for a file named `buildspec.yml` in the source code root directory\. If your buildspec file uses a different name or location, enter its path from the source root in **Buildspec name** \(for example, **buildspec\-two\.yml** or **configuration/buildspec\.yml**\. If the buildspec file is in an S3 bucket, it must be in the same AWS Region as your build project\. Specify the buildspec file using its ARN \(for example, `arn:aws:s3:::my-codebuild-sample2/buildspec.yml`\)\.
   + If your source code previously did not include a buildspec\.yml file but does now, choose **Use a buildspec file**\. 
   + If your source code previously included a buildspec\.yml file but does not now, choose **Insert build commands**, and in **Build commands**, enter the commands\.

1. Choose **Update buildspec**\.

1. To change information about the batch build configuration, in **Batch configuration**, choose **Edit** and update the folowing values as needed\.  
**Batch service role**  
Choose one of the following:  
   + If you do not have a batch service role, choose **New service role**\. In **Service role**, enter a name for the new role\.
   + If you have a batch service role, choose **Existing service role**\. In **Service role**, choose the service role\.
Batch builds introduce a new security role in the batch configuration\. This new role is required as CodeBuild must be able to call the `StartBuild`, `StopBuild`, and `RetryBuild` actions on your behalf to run builds as part of a batch\. Customers should use a new role, and not the same role they use in their build, for two reasons:  
   + Giving the build role `StartBuild`, `StopBuild`, and `RetryBuild` permissions would allow a single build to start more builds via the buildspec\.
   + CodeBuild batch builds provide restrictions that restrict the number of builds and compute types that can be used for the builds in the batch\. If the build role has these permissions, it is possible the builds themselves could bypass these restrictions\.  
**Allowed compute type\(s\) for batch**  
Select the compute types allowed for the batch\. Select all that apply\.  
**Maximum builds allowed in batch**  
Enter the maximum number of builds allowed in the batch\. If a batch exceeds this limit, the batch will fail\.  
**Batch timeout**  
Enter the maximum amount of time for the batch build to complete\.  
**Combine artifacts**  
Select **Combine all artifacts from batch into a single location** to have all of the artifacts from the batch combined into a single location\.

1. Choose **Update batch configuration**\.

1. To change information about the build output artifact location and name, in **Artifacts**, choose **Edit**, and then change the values for **Type**, **Name**, **Path**, **Namespace type**, or **Bucket name**\. 

1. To change information about the AWS KMS customer managed key \(CMK\), in **Additional configuration**, change the value for **Encryption key**\.
**Important**  
If you leave **Encryption key** blank, CodeBuild uses the AWS\-managed CMK for Amazon S3 in your AWS account instead\.

1. Using a cache saves build time because reusable pieces of the build environment are stored in the cache and used across builds\. For information about specifying a cache in the buildspec file, see [Buildspec syntax](build-spec-ref.md#build-spec-ref-syntax)\. To change information about the cache, expand **Additional configuration**\. In **Cache type**, do one of the following:
   + If you previously chose a cache, but do not want to use one now, choose **No cache**\.
   + If you previously chose **No cache** but now want to use one, choose **Amazon S3**, and then do the following:
     + For **Cache bucket**, choose the name of the S3 bucket where the cache is stored\.
     + \(Optional\) For **Cache path prefix**, enter an Amazon S3 path prefix\. The cache path prefix value is similar to a directory name\. You use it to store the cache under the same directory in a bucket\. 
**Important**  
Do not append a forward slash \(/\) to the end of **Path prefix**\.

1. To change your log settings, in **Logs**, select or clear **CloudWatch logs** and **S3 logs**\.

   If you select **CloudWatch logs**:
   +  In **Group name**, enter the name of your Amazon CloudWatch Logs group\. 
   +  In **Stream name**, enter your Amazon CloudWatch Logs stream name\. 

   If you select **S3 logs**:
   +  From **Bucket**, choose the name of the S3 bucket for your logs\. 
   +  In **Path prefix**, enter the prefix for your logs\. 
   +  Select **Remove S3 log encryption** if you do not want your S3 logs encrypted\. 

1. To change information about the way build output artifacts are stored, in **Additional configuration**, change the value of **Artifacts packaging**\.

1. To change whether build artifacts are encrypted, use **Disable artifacts encryption**\.

1. Choose **Update artifacts**\.