# Run a build \(console\)<a name="run-build-console"></a>

To use AWS CodePipeline to run a build with CodeBuild, skip these steps and follow the instructions in [Use CodePipeline with CodeBuild](how-to-create-pipeline.md)\.

1. Open the AWS CodeBuild console at [https://console\.aws\.amazon\.com/codesuite/codebuild/home](https://console.aws.amazon.com/codesuite/codebuild/home)\.

1. Do one of the following:
   + If you just finished creating a build project, the **Build project: *project\-name*** page should be displayed\. Choose **Start build**\.
   + If you created a build project earlier, in the navigation pane, choose **Build projects**\. Choose the build project, and then choose **Start build**\.

1. On the **Start build** page, do one of the following:
   + For Amazon S3, for the optional **Source version** value, enter the version ID for the version of the input artifact you want to build\. If **Source version** is left blank, the latest version is used\.
   + For CodeCommit, for **Reference type**, choose **Branch**, **Git tag**, or **Commit ID**\. Next, choose the branch, Git tag, or enter a commit ID to specify the version of your source code\. For more information, see [Source version sample with AWS CodeBuild](sample-source-version.md)\. Change the value for **Git clone depth**\. This creates a shallow clone with a history truncated to the specified number of commits\. If you want a full clone, choose **Full**\.
   + For GitHub or GitHub Enterprise Server, for the optional **Source version** value, enter a commit ID, pull request ID, branch name, or tag name for the version of the source code you want to build\. If you specify a pull request ID, it must use the format `pr/pull-request-ID` \(for example, `pr/25`\)\. If you specify a branch name, the branch's HEAD commit ID is used\. If **Source version** is blank, the default branch's HEAD commit ID is used\. Change the value for **Git clone depth**\. This creates a shallow clone with a history truncated to the specified number of commits\. If you want a full clone, choose **Full**\.
   + For Bitbucket, for the optional **Source version** value, enter a commit ID, branch name, or tag name for the version of the source code you want to build\. If you specify a branch name, the branch's HEAD commit ID is used\. If **Source version** is blank, the default branch's HEAD commit ID is used\. Change the value for **Git clone depth**\. This creates a shallow clone with a history truncated to the specified number of commits\. If you want a full clone, choose **Full**\.
   + To use a different source provider for this build only, choose **Advanced build options**\. For more information about source provider options and settings, see [Choose source provider](create-project-console.md#create-project-source-provider)\.

1. Choose **Advanced build overrides**\. 

   Here you can change settings for this build only\. The settings in this section are optional\. 

   Under **Build configuration**, choose from the following:\.   
**Single build**  
Choose this to perform a single build\.  
**Batch build**  
Choose this to perform a batch build\.

   Under **Batch configuration**, set the batch build configuration overrides for this build\. 
**Note**  
This section is only displayed when **Batch build** is selected in **Build configuration**\.  
**Service role**  
Provides the service role for batch builds\. Choose one of the following:  
   + If you do not have a batch service role, choose **New service role**\. In **Service role**, enter a name for the new role\.
   + If you have a batch service role, choose **Existing service role**\. In **Service role**, choose the service role\.
To change whether CodeBuild can modify the batch service role you use for this build, select or clear **Allow AWS CodeBuild to modify this service role so it can be used with this build project**\. If you clear it, you must use a service role with CodeBuild permissions attached to it\. For more information, see [Add CodeBuild access permissions to an IAM group or IAM user](setting-up.md#setting-up-service-permissions-group) and [Create a CodeBuild service role](setting-up.md#setting-up-service-role)\.   
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

   Under **Source**, you can: 
   + Choose **Add source** to add a secondary source\.
   + Choose **Remove source** to remove a secondary source\.
   + Use **Source provider** and **Source version** to modify settings for a source\. 

   Under **Environment**, you can: 
   + Override settings for **Environment image**, **Operating system**, **Runtime**, and **Runtime version**\.
   + Select or clear **Privileged**\. 
**Note**  
By default, Docker containers do not allow access to any devices\. Privileged mode grants a build project's Docker container access to all devices\. For more information, see [Runtime Privilege and Linux Capabilities](https://docs.docker.com/engine/reference/run/#runtime-privilege-and-linux-capabilities) on the Docker Docs website\.
   + In **Service role**, you can change the service role that CodeBuild uses to call dependent AWS services for you\. Choose **New service role** to have CodeBuild create a service role for you\.
   + Choose **Override build specification** to use a different build specification\. 
   + Change the value for **Timeout**\.
   + Change the value for **Compute**\. 
   + From **Certificate**, choose a different setting\.

   Under **Buildspec**, you can: 
   + Choose **Use a buildspec file** to use a buildspec\.yml file\. By default, CodeBuild looks for a file named `buildspec.yml` in the source code root directory\. If your buildspec file uses a different name or location, enter its path from the source root in **Buildspec name** \(for example, **buildspec\-two\.yml** or **configuration/buildspec\.yml**\. If the buildspec file is in an S3 bucket, it must be in the same AWS Region as your build project\. Specify the buildspec file by its ARN \(for example, **arn:aws:s3:::my\-codebuild\-sample2/buildspec\.yml**\)\. 
   + Choose **Insert build commands** to enter commands you want to run during the build phase\.

   Under **Build Artifacts**, you can: 
   + From **Type**, choose a different artifacts type\.
   + In **Name**, enter a different output artifact name\. 
   + If you want a name specified in the buildspec file to override any name specified in the console, select **Enable semantic versioning**\. The name in a buildspec file uses the Shell command language\. For example, you can append a date and time to your artifact name so that it is always unique\. Unique artifact names prevent artifacts from being overwritten\. For more information, see [Buildspec syntax](build-spec-ref.md#build-spec-ref-syntax)\.
   + In **Path**, enter a different output artifact path\. 
   + In **Namespace type**, choose a different type\. Choose **Build ID** to insert the build ID into the path of the build output file \(for example, `My-Path/Build-ID/My-Artifact.zip`\)\. Otherwise, choose **None**\. 
   + From **Bucket name** choose a different S3 bucket for your output artifacts\. 
   + If you do not want your build artifacts encrypted, select **Disable artifacts encryption**\.
   + Select **Artifacts packaging**, and then choose **Zip** to put the build artifact files in a compressed file\. To put the build artifact files in the specified S3 bucket individually \(not compressed\), choose **None**\.
   + Under **Cache**, from **Type**, choose a different cache setting\.
   + To override secondary artifacts for this build only:
     + To remove a secondary artifact, in **Secondary artifacts**, choose the **X** in its row\.
     + To add a secondary artifact, choose **Add artifact**, and then enter the information for your secondary artifact\. For more information, see step 8 in [Create a build project \(console\)](create-project-console.md)\.

   Under **Logs**, you can override your log settings by selecting or clearing **CloudWatch Logs** and **S3 logs**\. 
   + If you enable **CloudWatch logs**: 
     + In **Group name**, enter the name of your Amazon CloudWatch Logs group\. 
     + In **Stream name**, enter your Amazon CloudWatch Logs stream name\. 
   + If you enable **S3 logs**: 
     + From **Bucket**, choose the name of the S3 bucket for your logs\. 
     + In **Path prefix**, enter the prefix for your logs\. 

   Under **Service role**, you can change the service role that CodeBuild uses to call dependent AWS services for you\. Choose **Create a role** to have CodeBuild create a service role for you\.

1. Expand **Environment variables override**\. 

   The environment variable list is pre\-populated with the environment variables that are set in the build project\. If you want to change the value of a pre\-populated environment variable for this build only, change the values for **Value** and/or **Type**\. Choose **Add environment variable** to add a new environment variable for this build only\. 
**Note**  
The **Remove** button cannot be used to remove a pre\-populated environment variable\. The **Remove** button is only used to remove an environment variable added or modified for this build\.

   Others can see an environment variable by using the CodeBuild console and the AWS CLI\. If you have no concerns about the visibility of your environment variable, set the **Name** and **Value** fields, and then set **Type** to **Plaintext**\.

   We recommend that you store an environment variable with a sensitive value, such as an AWS access key ID, an AWS secret access key, or a password as a parameter in Amazon EC2 Systems Manager Parameter Store\. For **Type**, choose **Parameter**\. For **Name**, type an identifier for CodeBuild to reference\. For **Value**, enter the parameter's name as stored in Amazon EC2 Systems Manager Parameter Store\. Using a parameter named `/CodeBuild/dockerLoginPassword` as an example, for **Type**, choose **Parameter**\. For **Name**, enter `LOGIN_PASSWORD`\. For **Value**, enter `/CodeBuild/dockerLoginPassword`\. 

   We recommend that you store parameters in Amazon EC2 Systems Manager Parameter Store with parameter names that start with `/CodeBuild/` \(for example, `/CodeBuild/dockerLoginPassword`\)\. You can use the CodeBuild console to create a parameter in Amazon EC2 Systems Manager\. Choose **Create a parameter**, and then follow the instructions\. \(In that dialog box, for **KMS key**, you can optionally specify the ARN of an AWS KMS key in your account\. Amazon EC2 Systems Manager uses this key to encrypt the parameter's value during storage and decrypt during retrieval\.\) If you use the CodeBuild console to create a parameter, the console starts the parameter with `/CodeBuild/` as it is being stored\. For more information, see [Systems Manager Parameter Store](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-paramstore.html) and [Walkthrough: Create and test a String parameter \(console\)](https://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-paramstore-console.html) in the *Amazon EC2 Systems Manager User Guide*\.

   If your build project refers to parameters stored in Amazon EC2 Systems Manager Parameter Store, the build project's service role must allow the `ssm:GetParameters` action\. If you chose **Create a service role in your account** earlier, then CodeBuild includes this action in the default service role for your build project automatically\. However, if you chose **Choose an existing service role from your account**, then you must include this action in your service role separately\.

   If your build project refers to parameters stored in Amazon EC2 Systems Manager Parameter Store with parameter names that do not start with `/CodeBuild/`, and you chose **Create a service role in your account**, then you must update that service role to allow access to parameter names that do not start with `/CodeBuild/`\. This is because that service role allows access only to parameter names that start with `/CodeBuild/`\.

   Any environment variables you set replace existing environment variables\. For example, if the Docker image already contains an environment variable named `MY_VAR` with a value of `my_value`, and you set an environment variable named `MY_VAR` with a value of `other_value`, then `my_value` is replaced by `other_value`\. Similarly, if the Docker image already contains an environment variable named `PATH` with a value of `/usr/local/sbin:/usr/local/bin`, and you set an environment variable named `PATH` with a value of `$PATH:/usr/share/ant/bin`, then `/usr/local/sbin:/usr/local/bin` is replaced by the literal value `$PATH:/usr/share/ant/bin`\.

   Do not set any environment variable with a name that begins with `CODEBUILD_`\. This prefix is reserved for internal use\.

   If an environment variable with the same name is defined in multiple places, its value is determined as follows:
   + The value in the start build operation call takes highest precedence\.
   + The value in the build project definition takes next precedence\.
   + The value in the buildspec declaration takes lowest precedence\.

1. Choose **Start build**\.

   For detailed information about this build, see [View build details \(console\)](view-build-details.md#view-build-details-console)\.