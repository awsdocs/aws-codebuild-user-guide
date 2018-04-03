# Change a Build Project's Settings in AWS CodeBuild<a name="change-project"></a>

To change a build project's settings in AWS CodeBuild, you can use the AWS CodeBuild console, AWS CLI, or AWS SDKs\.

**Topics**
+ [Change a Build Project's Settings \(Console\)](#change-project-console)
+ [Change a Build Project's Settings \(AWS CLI\)](#change-project-cli)
+ [Change a Build Project's Settings \(AWS SDKs\)](#change-project-sdks)

## Change a Build Project's Settings \(Console\)<a name="change-project-console"></a>

1. Open the AWS CodeBuild console at [https://console\.aws\.amazon\.com/codebuild/](https://console.aws.amazon.com/codebuild/)\.

1. In the navigation pane, choose **Build projects**\.

1. Do one of the following:
   + Choose the radio button next to the build project you want to change, choose **Actions**, and then choose **Update**\.
   + Choose the link for the build project you want to change, and then choose **Edit project**\.
**Note**  
By default, only the 10 most recent build projects are displayed\. To view more build projects, choose a different value for **Projects per page** or choose the back and forward arrows for **Viewing projects**\.

1. On the project details page, type a description in **Description**\.

   For more information about settings referred to in this procedure, see [Create a Build Project \(Console\)](create-project.md#create-project-console)\.

1. To change information about the source code location, in the **Source: What to build** area, choose **Update source**\. Use the following table to make selections appopropriate for your source provider:  
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codebuild/latest/userguide/change-project.html)

1. To change information about the build environment, in **Environment: How to build**, choose **Update image**\. Make changes appropriate for the build environment type \(for example, **Environment image**, **Operating system**, **Runtime**, **Version**, **Custom image type**, **Custom image ID**, **Amazon ECR repository**, or **Amazon ECR image**\)\.

1. Do one of the following:
   + If your source code previously did not include a buildspec\.yml file but does now, choose **Update build specification**, and then choose **Use buildspec\.yml from source code**\. 
   + If your source code previously included a buildspec\.yml file but now does not, choose **Update build specification**, then choose **Insert build commands**, and then type the commands in **Build commands**\.

1. To change information about the build output artifact location and name, in **Artifacts: Where to put the artifacts from this build project**, change the values of **Artifacts type**, **Artifact name**, **Bucket name**, or **Output files**\.

1. To change information about the cache, in **Cache**, do one of the following:
   + If you previously chose a cache but now you do not want to use a cache, choose **No cache**\.
   + If you previously chose **No cache** but now you want to use a cache, choose **Amazon S3**, and then do the following:
     + For **Bucket**, choose the name of the Amazon S3 bucket where the cache is stored\.
     + \(Optional\) For **Path prefix**, type an Amazon S3 path prefix\. The **Path prefix** value is similar to a directory name that enables you to store the cache under the same directory in a bucket\. 
**Important**  
Do not append "/" to the end of **Path prefix**\.

   Using a cache saves considerable build time because reusable pieces of the build environment are stored in the cache and used across builds\. For information about specifying a cache in the build spec file, see [Build Spec Syntax](build-spec-ref.md#build-spec-ref-syntax)\.

1. To change information about the AWS CodeBuild service role, in **Service role**, change the values of **Create a role**, **Choose an existing service role from your account**, or **Role name**\.
**Note**  
When you use the console to create or update a build project, you can create an AWS CodeBuild service role at the same time\. By default, the role works with that build project only\. If you use the console to associate this service role with another build project, the role is updated to work with the other build project\. A service role can work with up to 10 build projects\.

1. In **VPC**, do one of the following:
   + If you are not using a VPC for your project, choose **No VPC**\.
   + If you want AWS CodeBuild to work with your VPC:
     + For **VPC**, choose the VPC ID that AWS CodeBuild uses\.
     + For **Subnets**, choose the subnets that include resources that AWS CodeBuild uses\.
     + For **Security Groups**, choose the security groups that AWS CodeBuild uses to allow access to resources in the VPCs\.

   For more information, see [Use AWS CodeBuild with Amazon Virtual Private Cloud](vpc-support.md)\.

1. To change information about the build timeout, in **Show advanced settings**, for **Timeout**, change the values of **hours** and **minutes**\. If **hours** and **minutes** are left blank, the default value is 60 minutes\.

1. To change information about the AWS KMS customer master key \(CMK\), in **Show advanced settings**, change the value of **Encryption key**\.
**Important**  
If you leave **Encryption key** blank, AWS CodeBuild uses the AWS\-managed CMK for Amazon S3 in your AWS account instead\.

1. If you plan to use this build project to build Docker images and the specified build environment is not provided by AWS CodeBuild with Docker support, in **Show advanced settings**, select **Privileged**\. Otherwise, all associated builds that attempt to interact with the Docker daemon fail\. You must also start the Docker daemon so that your builds can interact with it as needed\. One way to do this is to initialize the Docker daemon in the `install` phase of your build spec by running the following build commands\. \(Do not run the following build commands if the specified build environment image is provided by AWS CodeBuild with Docker support\.\)

   ```
   - nohup /usr/local/bin/dockerd --host=unix:///var/run/docker.sock --host=tcp://127.0.0.1:2375 --storage-driver=overlay&
   - timeout -t 15 sh -c "until docker info; do echo .; sleep 1; done"
   ```

1. To change information about the way build output artifacts are stored, in **Show advanced settings**, change the value of **Artifacts packaging**\.

1. To change the amount of memory and vCPUs that are used to run builds, in **Show advanced settings**, change the value of **Compute type**\.

1. To change information about environment variables you want builds to use, in **Show advanced settings**, for **Environment variables**, change the values for **Name**, **Value**, and **Type**\. Use **Add row** to add an environment variable\. Choose the delete \(**X**\) button next to an environment variable you no longer want to use\.

   Others can see environment variables by using the AWS CodeBuild console and the AWS CLI\. If you have no concerns about the visibility of your environment variable, set the **Name** and **Value** fields, and then set **Type** to **Plaintext**\.

   We recommend that you store an environment variable with a sensitive value, such as an AWS access key ID, an AWS secret access key, or a password as a parameter in Amazon EC2 Systems Manager Parameter Store\. For **Type**, choose **Parameter Store**\. For **Name**, type an identifier for AWS CodeBuild to reference\. For **Value**, type the parameter's name as stored in Amazon EC2 Systems Manager Parameter Store\. Using a parameter named `/CodeBuild/dockerLoginPassword` as an example, for **Type** choose **Parameter Store**\. For **Name**, type `LOGIN_PASSWORD`\. For **Value**, type `/CodeBuild/dockerLoginPassword`\.
**Important**  
We recommend that you store parameters in Amazon EC2 Systems Manager Parameter Store with parameter names that start with `/CodeBuild/` \(for example, `/CodeBuild/dockerLoginPassword`\)\. You can use the AWS CodeBuild console to create a parameter in Amazon EC2 Systems Manager\. Choose **Create a parameter**, and then follow the instructions in the dialog box\. \(In that dialog box, for **KMS key**, you can optionally specify the ARN of an AWS KMS key in your account\. Amazon EC2 Systems Manager uses this key to encrypt the parameter's value during storage and decrypt during retrieval\.\) If you use the AWS CodeBuild console to create a parameter, the console starts the parameter name with `/CodeBuild/` as it is being stored\. For more information, see [Systems Manager Parameter Store](http://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-paramstore.html) and [Systems Manager Parameter Store Console Walkthrough](http://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-paramstore-walk.html#sysman-paramstore-console) in the *Amazon EC2 Systems Manager User Guide*\.  
If your build project refers to parameters stored in Amazon EC2 Systems Manager Parameter Store, the build project's service role must allow the `ssm:GetParameters` action\. If you chose **Create a service role in your account** earlier, then AWS CodeBuild includes this action in the default service role for your build project automatically\. However, if you chose **Choose an existing service role from your account**, then you must include this action to your service role separately\.  
If your build project refers to parameters stored in Amazon EC2 Systems Manager Parameter Store with parameter names that do not start with `/CodeBuild/`, and you chose **Create a service role in your account**, then you must update that service role to allow access to parameter names that do not start with `/CodeBuild/`\. This is because that service role allows access only to parameter names that start with `/CodeBuild/`\.  
Environment variables you set replace existing environment variables\. For example, if the Docker image already contains an environment variable named `MY_VAR` with a value of `my_value`, and you set an environment variable named `MY_VAR` with a value of `other_value`, then `my_value` is replaced by `other_value`\. Similarly, if the Docker image already contains an environment variable named `PATH` with a value of `/usr/local/sbin:/usr/local/bin`, and you set an environment variable named `PATH` with a value of `$PATH:/usr/share/ant/bin`, then `/usr/local/sbin:/usr/local/bin` is replaced by the literal value `$PATH:/usr/share/ant/bin`\.  
Do not set any environment variable with a name that begins with `CODEBUILD_`\. This prefix is reserved for internal use\.  
If an environment variable with the same name is defined in multiple places, its value is determined as follows:  
The value in the start build operation call takes highest precedence\.
The value in the build project definition takes next precedence\.
The value in the build spec declaration takes lowest precedence\.

1. To change information about tags for this build project, in **Show advanced settings**, for **Tags**, change the values of **Name** and **Value**\. Use **Add row** to add a tag\. You can add up to 50 tags\. Choose the delete \(**X**\) icon next to a tag you no longer want to use\.

1. Choose **Update**\.

## Change a Build Project's Settings \(AWS CLI\)<a name="change-project-cli"></a>

For more information about using the AWS CLI with AWS CodeBuild, see the [Command Line Reference](cmd-ref.md)\.

1. Run the `update-project` command as follows:

   ```
   aws codebuild update-project --generate-cli-skeleton
   ```

   JSON\-formatted data appears in the output\. Copy the data to a file \(for example, `update-project.json`\) in a location on the local computer or instance where the AWS CLI is installed\. Then modify the copied data as described in [Create a Build Project \(AWS CLI\)](create-project.md#create-project-cli), and save your results\.
**Note**  
In the JSON\-formatted data, you must provide the name of the build project that you want to change settings for\. All other settings are optional\. You cannot change the build project's name, but you can change any of its other settings\.

1. Switch to the directory containing the file you just saved, and run the `update-project` command again\.

   ```
   aws codebuild update-project --cli-input-json file://update-project.json
   ```

1. If successful, data similar to that as described in [Create a Build Project \(AWS CLI\)](create-project.md#create-project-cli) appears in the output\.

## Change a Build Project's Settings \(AWS SDKs\)<a name="change-project-sdks"></a>

For information about using AWS CodeBuild with the AWS SDKs, see the [AWS SDKs and Tools Reference](sdk-ref.md)\.