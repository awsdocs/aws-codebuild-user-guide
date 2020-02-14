# Change a Build Project's Settings in CodeBuild<a name="change-project"></a>

You can use the AWS CodeBuild console, AWS CLI, or AWS SDKs to change a build project's settings\.

If you add test reporting to a build project, make sure your IAM role has the permissions described in [Working with Test Report Permissions](test-permissions.md)\.

**Topics**
+ [Change a Build Project's Settings \(Console\)](#change-project-console)
+ [Change a Build Project's Settings \(AWS CLI\)](#change-project-cli)
+ [Change a Build Project's Settings \(AWS SDKs\)](#change-project-sdks)

## Change a Build Project's Settings \(Console\)<a name="change-project-console"></a>

1. Open the AWS CodeBuild console at [https://console\.aws\.amazon\.com/codesuite/codebuild/home](https://console.aws.amazon.com/codesuite/codebuild/home)\.

1. In the navigation pane, choose **Build projects**\.

1. Do one of the following:
   + Choose the link for the build project you want to change, and then choose **Build details**\.
   + Choose the button next to the build project you want to change, choose **View details**, and then choose **Build details**\.

1. To change the project's description, in **Project configuration**, choose **Edit**, and then enter a description\.

   Choose **Update configuration**\.

   For more information about settings referred to in this procedure, see [Create a Build Project \(Console\)](create-project.md#create-project-console)\.

1. To change information about the source code location, in **Source**, choose **Edit**\. Use the following table to make selections appropriate for your source provider, and then choose **Update source**\.
**Note**  
CodeBuild does not support Bitbucket Server\.  
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codebuild/latest/userguide/change-project.html)

   To change whether CodeBuild can modify the service role you use for this project, select or clear **Allow AWS CodeBuild to modify this service role so it can be used with this build project**\. If you clear it, you must use a service role with CodeBuild permissions attached to it\. For more information, see [Add CodeBuild Access Permissions to an IAM Group or IAM User](setting-up.md#setting-up-service-permissions-group) and [Create a CodeBuild Service Role](setting-up.md#setting-up-service-role)\. 

1. To change information about the build environment, in **Environment**, choose **Edit**\. Make changes appropriate for the build environment type \(for example, **Environment image**, **Operating system**, **Runtime**, **Runtime version**, **Custom image**, **Other location**, **Amazon ECR repository**, or **Amazon ECR image**\)\.

1. If you plan to use this build project to build Docker images and the specified build environment is not provided by CodeBuild with Docker support, select **Privileged**\. Otherwise, all associated builds that attempt to interact with the Docker daemon fail\. You must also start the Docker daemon so that your builds can interact with it as needed\. You can do this by by running the following build commands to initialize the Docker daemon in the `install` phase of your buildspec file\. \(Do not run the following build commands if the specified build environment image is provided by CodeBuild with Docker support\.\)
**Note**  
By default, Docker containers do not allow access to any devices\. Privileged mode grants a build project's Docker container access to all devices\. For more information, see [Runtime Privilege and Linux Capabilities](https://docs.docker.com/engine/reference/run/#runtime-privilege-and-linux-capabilities) on the Docker Docs website\.

   ```
   - nohup /usr/local/bin/dockerd --host=unix:///var/run/docker.sock --host=tcp://127.0.0.1:2375 --storage-driver=overlay&
    - timeout -t 15 sh -c "until docker info; do echo .; sleep 1; done"
   ```

1. To change information about the CodeBuild service role, in **Service role**, change the values for **New service role**, **Existing service role**, or **Role name**\.
**Note**  
When you use the console to create or update a build project, you can create a CodeBuild service role at the same time\. By default, the role works with that build project only\. If you use the console to associate this service role with another build project, the role is updated to work with the other build project\. A service role can work with up to 10 build projects\.

1. To change information about the build timeout, in **Additional configuration**, for **Timeout**, change the values for **hours** and **minutes**\. If **hours** and **minutes** are left blank, the default value is 60 minutes\.

1. To change information about the VPC you created in Amazon VPC, in **Additional configuration**, change the values for **VPC**, **Subnets**, and **Security groups**\. 

1. To change information about a file system you created in Amazon EFS, in **Additional configuration**, change its values for **Identifier**, **ID**, **Directory path**, **Mount point**, and **Mount options**\. For more information, see [Amazon Elastic File System Sample for CodeBuild](sample-efs.md)\. 

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
The value in the build spec declaration takes lowest precedence\.

   If you use Secrets Manager, for **Type**, choose **Secrets Manager**\. For **Name**, enter an identifier for CodeBuild to reference\. For **Value**, enter a `reference-key` using the pattern `secret-id:json-key:version-stage:version-id`\. For information, see [Secrets Manager reference-key in the Buildspec File](build-spec-ref.md#secrets-manager-build-spec)\.
**Important**  
If you use Secrets Manager, we recommend that you store secrets with names that start with `/CodeBuild/` \(for example, `/CodeBuild/dockerLoginPassword`\)\. For more information, see [What Is AWS Secrets Manager?](https://docs.aws.amazon.com/secretsmanager/latest/userguide/intro.html) in the *AWS Secrets Manager User Guide*\.   
If your build project refers to secrets stored in Secrets Manager, the build project's service role must allow the `secretsmanager:GetSecretValue` action\. If you chose **New service role** earlier, CodeBuild includes this action in the default service role for your build project\. However, if you chose **Existing service role**, you must include this action to your service role separately\.   
If your build project refers to secrets stored in Secrets Manager with secret names that do not start with `/CodeBuild/`, and you chose **New service role**, you must update the service role to allow access to secret names that do not start with `/CodeBuild/`\. This is because the service role allows access only to secret names that start with `/CodeBuild/`\.  
If you choose **New service role**, the service role includes permission to decrypt all secrets under the `/CodeBuild/` namespace in the Secrets Manager\.

1. To change information about tags for this build project, in **Additional configuration**, for **Tags**, change the values of **Name** and **Value**\. Use **Add row** to add a tag\. You can add up to 50 tags\. Choose the delete \(**X**\) icon next to a tag you no longer want to use\.

1. Choose **Update environment**\.

1. To change the project's build specifications, in **Buildspec**, choose **Edit**\. By default, CodeBuild looks for a file named `buildspec.yml` in the source code root directory\. If your buildspec file uses a different name or location, enter its path from the source root in **Buildspec name** \(for example, **buildspec\-two\.yml** or **configuration/buildspec\.yml**\. If the buildspec file is in an S3 bucket, it must be in the same AWS Region as your build project\. Specify the buildspec file using its ARN \(for example, `arn:aws:s3:::my-codebuild-sample2/buildspec.yml`\)\.
   + If your source code previously did not include a buildspec\.yml file but does now, choose **Use a buildspec file**\. 
   + If your source code previously included a buildspec\.yml file but does not now, choose **Insert build commands**, and in **Build commands**, enter the commands\.

1. Choose **Update buildspec**\.

1. To change information about the build output artifact location and name, in **Artifacts**, choose **Edit**, and then change the values for **Type**, **Name**, **Path**, **Namespace type**, or **Bucket name**\. 

1. To change information about the AWS KMS customer managed key \(CMK\), in **Additional configuration**, change the value for **Encryption key**\.
**Important**  
If you leave **Encryption key** blank, CodeBuild uses the AWS\-managed CMK for Amazon S3 in your AWS account instead\.

1. Using a cache saves build time because reusable pieces of the build environment are stored in the cache and used across builds\. For information about specifying a cache in the buildspec file, see [Buildspec Syntax](build-spec-ref.md#build-spec-ref-syntax)\. To change information about the cache, expand **Additional configuration**\. In **Cache type**, do one of the following:
   + If you previously chose a cache, but do not want to use one now, choose **No cache**\.
   + If you previously chose **No cache** but now want to use one, choose **Amazon S3**, and then do the following:
     + For **Cache bucket**, choose the name of the Amazon S3 bucket where the cache is stored\.
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

## Change a Build Project's Settings \(AWS CLI\)<a name="change-project-cli"></a>

For information about using the AWS CLI with AWS CodeBuild, see the [Command Line Reference](cmd-ref.md)\.

1. Run the `update-project` command as follows:

   ```
   aws codebuild update-project --generate-cli-skeleton
   ```

   JSON\-formatted data appears in the output\. Copy the data to a file \(for example, `update-project.json`\) in a location on the local computer or instance where the AWS CLI is installed\. Then modify the copied data as described in [Create a Build Project \(AWS CLI\)](create-project.md#create-project-cli), and save your results\.
**Note**  
In the JSON\-formatted data, you must provide the name of the build project\. All other settings are optional\. You cannot change the build project's name, but you can change any of its other settings\.

1. Switch to the directory that contains the file you just saved, and run the update\-project command again\.

   ```
   aws codebuild update-project --cli-input-json file://update-project.json
   ```

1. If successful, data similar to that as described in [Create a Build Project \(AWS CLI\)](create-project.md#create-project-cli) appears in the output\.

## Change a Build Project's Settings \(AWS SDKs\)<a name="change-project-sdks"></a>

For information about using AWS CodeBuild with the AWS SDKs, see the [AWS SDKs and Tools Reference](sdk-ref.md)\.