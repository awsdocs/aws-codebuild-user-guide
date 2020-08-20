# Use AWS CodePipeline with AWS CodeBuild to test code and run builds<a name="how-to-create-pipeline"></a>

You can automate your release process by using AWS CodePipeline to test your code and run your builds with AWS CodeBuild\.

The following table lists tasks and the methods available for performing them\. Using the AWS SDKs to accomplish these tasks is outside the scope of this topic\. 


****  

| Task | Available approaches | Approaches described in this topic | 
| --- | --- | --- | 
| Create a continuous delivery \(CD\) pipeline with CodePipeline that automates builds with CodeBuild |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codebuild/latest/userguide/how-to-create-pipeline.html)  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codebuild/latest/userguide/how-to-create-pipeline.html)  | 
| Add test and build automation with CodeBuild to an existing pipeline in CodePipeline |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codebuild/latest/userguide/how-to-create-pipeline.html)  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codebuild/latest/userguide/how-to-create-pipeline.html)  | 

**Topics**
+ [Prerequisites](#how-to-create-pipeline-prerequisites)
+ [Create a pipeline that uses CodeBuild \(CodePipeline console\)](#how-to-create-pipeline-console)
+ [Create a pipeline that uses CodeBuild \(AWS CLI\)](#how-to-create-pipeline-cli)
+ [Add a CodeBuild build action to a pipeline \(CodePipeline console\)](#how-to-create-pipeline-add)
+ [Add a CodeBuild test action to a pipeline \(CodePipeline console\)](#how-to-create-pipeline-add-test)

## Prerequisites<a name="how-to-create-pipeline-prerequisites"></a>

1. Answer the questions in [Plan a build](planning.md)\.

1. If you are using an IAM user to access CodePipeline instead of an AWS root account or an administrator IAM user, attach the managed policy named `AWSCodePipelineFullAccess` to the user \(or to the IAM group to which the user belongs\)\. Using an AWS root account is not recommended\. This policy grants the user permission to create the pipeline in CodePipeline\. For more information, see [Attaching managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-using.html#attach-managed-policy-console) in the *IAM User Guide*\.
**Note**  
The IAM entity that attaches the policy to the user \(or to the IAM group to which the user belongs\) must have permission in IAM to attach policies\. For more information, see [Delegating permissions to administer IAM users, groups, and credentials](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_delegate-permissions.html) in the *IAM User Guide*\.

1. Create a CodePipeline service role, if you do not already have one available in your AWS account\. CodePipeline uses this service role to interact with other AWS services, including AWS CodeBuild, on your behalf\. For example, to use the AWS CLI to create a CodePipeline service role, run the IAM `create-role` command:

   For Linux, macOS, or Unix:

   ```
   aws iam create-role --role-name AWS-CodePipeline-CodeBuild-Service-Role --assume-role-policy-document '{"Version":"2012-10-17","Statement":{"Effect":"Allow","Principal":{"Service":"codepipeline.amazonaws.com"},"Action":"sts:AssumeRole"}}'
   ```

   For Windows:

   ```
   aws iam create-role --role-name AWS-CodePipeline-CodeBuild-Service-Role --assume-role-policy-document "{\"Version\":\"2012-10-17\",\"Statement\":{\"Effect\":\"Allow\",\"Principal\":{\"Service\":\"codepipeline.amazonaws.com\"},\"Action\":\"sts:AssumeRole\"}}"
   ```
**Note**  
The IAM entity that creates this CodePipeline service role must have permission in IAM to create service roles\.

1. After you create a CodePipeline service role or identify an existing one, you must add the default CodePipeline service role policy to the service role as described in [Review the default CodePipeline service role policy](https://docs.aws.amazon.com/codepipeline/latest/userguide/iam-identity-based-access-control.html#how-to-custom-role) in the *AWS CodePipeline User Guide*, if it isn't already a part of the policy for the role\.
**Note**  
The IAM entity that adds this CodePipeline service role policy must have permission in IAM to add service role policies to service roles\.

1. Create and upload the source code to a repository type supported by CodeBuild and CodePipeline, such as CodeCommit, Amazon S3, or GitHub\. \(CodePipeline does not currently support Bitbucket\.\) The source code should contain a buildspec file, but you can declare one when you define a build project later in this topic\. For more information, see the [Buildspec reference](build-spec-ref.md)\.
**Important**  
If you plan to use the pipeline to deploy built source code, the build output artifact must be compatible with the deployment system you use\.   
For CodeDeploy, see the [AWS CodeDeploy sample](sample-codedeploy.md) in this guide and [Prepare a revision for CodeDeploy](https://docs.aws.amazon.com/codedeploy/latest/userguide/how-to-prepare-revision.html) in the *AWS CodeDeploy User Guide*\.
For AWS Elastic Beanstalk, see the [AWS Elastic Beanstalk sample](sample-elastic-beanstalk.md) in this guide and [Create an application source bundle](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/using-features.deployment.source.html) in the *AWS Elastic Beanstalk Developer Guide*\.
For AWS OpsWorks, see [Application source](https://docs.aws.amazon.com/opsworks/latest/userguide/workingapps-creating.html#workingapps-creating-source) and [Using CodePipeline with AWS OpsWorks](https://docs.aws.amazon.com/opsworks/latest/userguide/other-services-cp.html) in the *AWS OpsWorks User Guide*\.

## Create a pipeline that uses CodeBuild \(CodePipeline console\)<a name="how-to-create-pipeline-console"></a>

Use the following procedure to create a pipeline that uses CodeBuild to build and deploy your source code\.

To create a pipeline that only tests your source code:
+ Use the following procedure to create the pipeline, and then delete the Build and Beta stages from the pipeline\. Then use the [Add a CodeBuild test action to a pipeline \(CodePipeline console\)](#how-to-create-pipeline-add-test) procedure in this topic to add to the pipeline a test action that uses CodeBuild\.
+ Use one of the other procedures in this topic to create the pipeline, and then use the [Add a CodeBuild test action to a pipeline \(CodePipeline console\)](#how-to-create-pipeline-add-test) procedure in this topic to add to the pipeline a test action that uses CodeBuild\.

**To use the create pipeline wizard in CodePipeline to create a pipeline that uses CodeBuild**

1. Sign in to the AWS Management Console by using:
   + Your AWS root account\. This is not recommended\. For more information, see [The account root user](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_root-user.html) in the *IAM User Guide*\.
   + An administrator IAM user in your AWS account\. For more information, see [Creating your first IAM admin user and group](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html) in the *IAM User Guide*\.
   + An IAM user in your AWS account with permission to use the following minimum set of actions:

     ```
     codepipeline:*
     iam:ListRoles
     iam:PassRole
     s3:CreateBucket
     s3:GetBucketPolicy
     s3:GetObject
     s3:ListAllMyBuckets
     s3:ListBucket
     s3:PutBucketPolicy
     codecommit:ListBranches
     codecommit:ListRepositories
     codedeploy:GetApplication
     codedeploy:GetDeploymentGroup
     codedeploy:ListApplications
     codedeploy:ListDeploymentGroups
     elasticbeanstalk:DescribeApplications
     elasticbeanstalk:DescribeEnvironments
     lambda:GetFunctionConfiguration
     lambda:ListFunctions
     opsworks:DescribeStacks
     opsworks:DescribeApps
     opsworks:DescribeLayers
     ```

1. Open the AWS CodePipeline console at [https://console\.aws\.amazon\.com/codesuite/codepipeline/home](https://console.aws.amazon.com/codesuite/codepipeline/home)\.

1. In the AWS Region selector, choose the AWS Region where your build project AWS resources are located\. This must be an AWS Region where CodeBuild is supported\. For more information, see [AWS CodeBuild](https://docs.aws.amazon.com/general/latest/gr/rande.html#codebuild_region) in the *Amazon Web Services General Reference*\.

1. Create a pipeline\. If a CodePipeline information page is displayed, choose **Create pipeline**\. If a **Pipelines** page is displayed, choose **Create pipeline**\.

1. On the **Step 1: Choose pipeline settings** page, for **Pipeline name**, enter a name for the pipeline \(for example, **CodeBuildDemoPipeline**\)\. If you choose a different name, be sure to use it throughout this procedure\.

1. For **Role name**, do one of the following:

   Choose **New service role**, and in **Role Name**, enter the name for your new service role\.

   Choose **Existing service role**, and then choose the CodePipeline service role you created or identified as part of this topic's prerequisites\.

1. For **Artifact store**, do one of the following:
   + Choose **Default location** to use the default artifact store, such as the S3 artifact bucket designated as the default, for your pipeline in the AWS Region you have selected for your pipeline\.
   + Choose **Custom location** if you already have an existing artifact store you have created, such as an S3 artifact bucket, in the same AWS Region as your pipeline\.
**Note**  
This is not the source bucket for your pipeline's source code\. This is the artifact store for your pipeline\. A separate artifact store, such as an S3 bucket, is required for each pipeline, in the same AWS Region as the pipeline\.

1. Choose **Next**\.

1. On the **Step 2: Add source stage** page, for **Source provider**, do one of the following:
   + If your source code is stored in an S3 bucket, choose **Amazon S3**\. For **Bucket**, select the S3 bucket that contains your source code\. For **S3 object key**, enter the name of the file the contains the source code \(for example, `file-name.zip`\)\. Choose **Next**\.
   + If your source code is stored in an AWS CodeCommit repository, choose **CodeCommit**\. For **Repository name**, choose the name of the repository that contains the source code\. For **Branch name**, choose the name of the branch that contains the version of the source code you want to build\. Choose **Next**\.
   + If your source code is stored in a GitHub repository, choose **GitHub**\. Choose **Connect to GitHub**, and follow the instructions to authenticate with GitHub\. For **Repository**, choose the name of the repository that contains the source code\. For **Branch**, choose the name of the branch that contains the version of the source code you want to build\. 

   Choose **Next**\.

1. On the **Step 3: Add build stage** page, for **Build provider**, choose **CodeBuild**\.

1. If you already have a build project you want to use, for **Project name**, choose the name of the build project and skip ahead to step 22 in this procedure\. Otherwise, use the following steps to create a project in CodeBuild\.

   If you choose an existing build project, it must have build output artifact settings already defined \(even though CodePipeline overrides them\)\. For more information, see [Create a build project \(console\)](create-project-console.md) or [Change a build project's settings \(console\)](change-project-console.md)\.
**Important**  
If you enable webhooks for a CodeBuild project, and the project is used as a build step in CodePipeline, then two identical builds are created for each commit\. One build is triggered through webhooks, and one through CodePipeline\. Because billing is on a per\-build basis, you are billed for both builds\. Therefore, if you are using CodePipeline, we recommend that you disable webhooks in CodeBuild\. In the AWS CodeBuild console, clear the **Webhook** box\. For more information, see [Change a build project's settings \(console\)](change-project-console.md)\.

1. On the **Step 4: Add deploy stage** page, do one of the following:
   + If you do not want to deploy the build output artifact, choose **Skip**, and confirm this choice when prompted\. 
   + If you want to deploy the build output artifact, for **Deploy provider**, choose a deployment provider, and then specify the settings when prompted\.

   Choose **Next**\.

1. On the ** Review** page, review your choices, and then choose **Create pipeline**\.

1. After the pipeline runs successfully, you can get the build output artifact\. With the pipeline displayed in the CodePipeline console, in the **Build** action, choose the tooltip\. Make a note of the value for **Output artifact** \(for example, **MyAppBuild**\)\.
**Note**  
You can also get the build output artifact by choosing the **Build artifacts** link on the build details page in the CodeBuild console\. To get to this page, skip the rest of the steps in this procedure, and see [View build details \(console\)](view-build-details.md#view-build-details-console)\.

1. Open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. In the list of buckets, open the bucket used by the pipeline\. The name of the bucket should follow the format `codepipeline-region-ID-random-number`\. You can use the AWS CLI to run the CodePipeline get\-pipeline command to get the name of the bucket, where *my\-pipeline\-name* is the display name of your pipeline:

   ```
   aws codepipeline get-pipeline --name my-pipeline-name
   ```

    In the output, the `pipeline` object contains an `artifactStore` object, which contains a `location` value with the name of the bucket\.

1. Open the folder that matches the name of your pipeline \(depending on the length of the pipeline's name, the folder name might be truncated\), and then open the folder that matches the value for **Output artifact** that you noted earlier\.

1. Extract the contents of the file\. If there are multiple files in that folder, extract the contents of the file with the latest **Last Modified** timestamp\. \(You might need to give the file the `.zip` extension so that you can work with it in your system's ZIP utility\.\) The build output artifact is in the extracted contents of the file\.

1. If you instructed CodePipeline to deploy the build output artifact, use the deployment provider's instructions to get to the build output artifact on the deployment targets\.

## Create a pipeline that uses CodeBuild \(AWS CLI\)<a name="how-to-create-pipeline-cli"></a>

Use the following procedure to create a pipeline that uses CodeBuild to build your source code\.

To use the AWS CLI to create a pipeline that deploys your built source code or that only tests your source code, you can adapt the instructions in [Edit a pipeline \(AWS CLI\)](https://docs.aws.amazon.com/codepipeline/latest/userguide/how-to-edit-pipelines.html#how-to-edit-pipelines-cli) and the [CodePipeline pipeline structure reference](https://docs.aws.amazon.com/codepipeline/latest/userguide/pipeline-structure.html) in the *AWS CodePipeline User Guide*\.

1. Create or identify a build project in CodeBuild\. For more information, see [Create a build project](create-project.md)\.
**Important**  
The build project must define build output artifact settings \(even though CodePipeline overrides them\)\. For more information, see the description of `artifacts` in [Create a build project \(AWS CLI\)](create-project-cli.md)\.

1. Make sure you have configured the AWS CLI with the AWS access key and AWS secret access key that correspond to one of the IAM entities described in this topic\. For more information, see [Getting set up with the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-set-up.html) in the *AWS Command Line Interface User Guide*\.

1. Create a JSON\-formatted file that represents the structure of the pipeline\. Name the file `create-pipeline.json` or similar\. For example, this JSON\-formatted structure creates a pipeline with a source action that references an S3 input bucket and a build action that uses CodeBuild:

   ```
   {
    "pipeline": {
     "roleArn": "arn:aws:iam::account-id:role/my-AWS-CodePipeline-service-role-name",
     "stages": [
       {
         "name": "Source",
         "actions": [
           {
             "inputArtifacts": [],
             "name": "Source",
             "actionTypeId": {
               "category": "Source",
               "owner": "AWS",
               "version": "1",
               "provider": "S3"
             },
             "outputArtifacts": [
               {
                 "name": "MyApp"
               }
             ],
             "configuration": {
               "S3Bucket": "my-input-bucket-name",
               "S3ObjectKey": "my-source-code-file-name.zip"
             },
             "runOrder": 1
           }
         ]
       },
       {
         "name": "Build",
         "actions": [
           {
             "inputArtifacts": [
               {
                 "name": "MyApp"
               }
             ],
             "name": "Build",
             "actionTypeId": {
               "category": "Build",
               "owner": "AWS",
               "version": "1",
               "provider": "CodeBuild"
             },
             "outputArtifacts": [
   	     {
                 "name": "default"
               }
   	   ],
             "configuration": {
               "ProjectName": "my-build-project-name"
             },
             "runOrder": 1
           }
         ]
       }
     ],
     "artifactStore": {
       "type": "S3",
       "location": "AWS-CodePipeline-internal-bucket-name"
     },
     "name": "my-pipeline-name",
     "version": 1
    }
   }
   ```

   In this JSON\-formatted data:
   + The value of `roleArn` must match the ARN of the CodePipeline service role you created or identified as part of the prerequisites\.
   + The values of `S3Bucket` and `S3ObjectKey` in `configuration` assume the source code is stored in an S3 bucket\. For settings for other source code repository types, see the [CodePipeline pipeline structure reference](https://docs.aws.amazon.com/codepipeline/latest/userguide/pipeline-structure.html) in the *AWS CodePipeline User Guide*\.
   + The value of `ProjectName` is the name of the CodeBuild build project you created earlier in this procedure\.
   + The value of `location` is the name of the S3 bucket used by this pipeline\. For more information, see [Create a policy for an S3 Bucket to use as the artifact store for CodePipeline](https://docs.aws.amazon.com/codepipeline/latest/userguide/access-permissions.html#how-to-create-bucket-policy) in the *AWS CodePipeline User Guide*\.
   + The value of `name` is the name of this pipeline\. All pipeline names must be unique to your account\.

   Although this data describes only a source action and a build action, you can add actions for activities related to testing, deploying the build output artifact, invoking AWS Lambda functions, and more\. For more information, see the [AWS CodePipeline pipeline structure reference](https://docs.aws.amazon.com/codepipeline/latest/userguide/pipeline-structure.html) in the *AWS CodePipeline User Guide*\.

1. Switch to the folder that contains the JSON file, and then run the CodePipeline [create\-pipeline](https://docs.aws.amazon.com/cli/latest/reference/codepipeline/create-pipeline.html) command, specifying the file name:

   ```
   aws codepipeline create-pipeline --cli-input-json file://create-pipeline.json
   ```
**Note**  
You must create the pipeline in an AWS Region where CodeBuild is supported\. For more information, see [AWS CodeBuild](https://docs.aws.amazon.com/general/latest/gr/rande.html#codebuild_region) in the *Amazon Web Services General Reference*\.

   The JSON\-formatted data appears in the output, and CodePipeline creates the pipeline\.

1. To get information about the pipeline's status, run the CodePipeline [get\-pipeline\-state](https://docs.aws.amazon.com/cli/latest/reference/codepipeline/get-pipeline-state.html) command, specifying the name of the pipeline:

   ```
   aws codepipeline get-pipeline-state --name my-pipeline-name
   ```

   In the output, look for information that confirms the build was successful\. Ellipses \(`...`\) are used to show data that has been omitted for brevity\.

   ```
   {
     ...
     "stageStates": [
       ...  
       {
         "actionStates": [
           {
             "actionName": "CodeBuild",
             "latestExecution": {
               "status": "SUCCEEDED",
               ...
             },
             ...
           }
         ]
       }
     ]
   }
   ```

   If you run this command too early, you might not see any information about the build action\. You might need to run this command multiple times until the pipeline has finished running the build action\.

1. After a successful build, follow these instructions to get the build output artifact\. Open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.
**Note**  
You can also get the build output artifact by choosing the **Build artifacts** link on the related build details page in the CodeBuild console\. To get to this page, skip the rest of the steps in this procedure, and see [View build details \(console\)](view-build-details.md#view-build-details-console)\.

1. In the list of buckets, open the bucket used by the pipeline\. The name of the bucket should follow the format `codepipeline-region-ID-random-number`\. You can get the bucket name from the `create-pipeline.json` file or you can run the CodePipeline get\-pipeline command to get the bucket's name\.

   ```
   aws codepipeline get-pipeline --name my-pipeline-name
   ```

    In the output, the `pipeline` object contains an `artifactStore` object, which contains a `location` value with the name of the bucket\.

1. Open the folder that matches the name of your pipeline \(for example, `my-pipeline-name`\)\.

1. In that folder, open the folder named `default`\.

1. Extract the contents of the file\. If there are multiple files in that folder, extract the contents of the file with the latest **Last Modified** timestamp\. \(You might need to give the file a `.zip` extension so that you can work with it in your system's ZIP utility\.\) The build output artifact is in the extracted contents of the file\.

## Add a CodeBuild build action to a pipeline \(CodePipeline console\)<a name="how-to-create-pipeline-add"></a>

1. Sign in to the AWS Management Console by using:
   + Your AWS root account\. This is not recommended\. For more information, see [The account root user](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_root-user.html) in the *IAM User Guide*\.
   + An administrator IAM user in your AWS account\. For more information, see [Creating your first IAM admin user and group](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html) in the *IAM User Guide*\.
   + An IAM user in your AWS account with permission to perform the following minimum set of actions:

     ```
     codepipeline:*
     iam:ListRoles
     iam:PassRole
     s3:CreateBucket
     s3:GetBucketPolicy
     s3:GetObject
     s3:ListAllMyBuckets
     s3:ListBucket
     s3:PutBucketPolicy
     codecommit:ListBranches
     codecommit:ListRepositories
     codedeploy:GetApplication
     codedeploy:GetDeploymentGroup
     codedeploy:ListApplications
     codedeploy:ListDeploymentGroups
     elasticbeanstalk:DescribeApplications
     elasticbeanstalk:DescribeEnvironments
     lambda:GetFunctionConfiguration
     lambda:ListFunctions
     opsworks:DescribeStacks
     opsworks:DescribeApps
     opsworks:DescribeLayers
     ```

1. Open the CodePipeline console at [https://console\.aws\.amazon\.com/codesuite/codepipeline/home](https://console.aws.amazon.com/codesuite/codepipeline/home)\.

1. In the AWS region selector, choose the AWS Region where your pipeline is located\. This must be a Region where CodeBuild is supported\. For more information, see [CodeBuild](https://docs.aws.amazon.com/general/latest/gr/rande.html#codebuild_region) in the *Amazon Web Services General Reference*\.

1. On the **Pipelines** page, choose the name of the pipeline\.

1. On the pipeline details page, in the **Source** action, choose the tooltip\. Make a note of the value for **Output artifact** \(for example, **MyApp**\)\.
**Note**  
This procedure shows you how to add a build action in a build stage between the **Source** and **Beta** stages\. If you want to add the build action somewhere else, choose the tooltip on the action just before the place where you want to add the build action, and make a note of the value for **Output artifact**\.

1. Choose **Edit**\.

1. Between the **Source** and **Beta** stages, choose **Add stage**\.
**Note**  
This procedure shows you how to add a build stage between the **Source** and **Beta** stages to your pipeline\. To add a build action to an existing stage, choose **Edit stage** in the stage, and then skip to step 8 of this procedure\. To add the build stage somewhere else, choose **Add stage** in the desired place\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/create-stage.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

1. For **Stage name**, enter the name of the build stage \(for example, **Build**\)\. If you choose a different name, use it throughout this procedure\.

1. Inside of the selected stage, choose **Add action**\.
**Note**  
This procedure shows you how to add the build action inside of a build stage\. To add the build action somewhere else, choose **Add action** in the desired place\. You might first need to choose **Edit stage** in the existing stage where you want to add the build action\.

1. In **Edit action**, for **Action name**, enter a name for the action \(for example, **CodeBuild**\)\. If you choose a different name, use it throughout this procedure\.

1. For **Action provider**, choose **CodeBuild**\.

1. If you already have a build project in CodeBuild, for **Project name**, choose the name of the build project, and then skip to step 22 of this procedure\.

   If you choose an existing build project, it must have build output artifact settings already defined \(even though CodePipeline overrides them\)\. For more information, see the description of **Artifacts** in [Create a build project \(console\)](create-project-console.md) or [Change a build project's settings \(console\)](change-project-console.md)\.
**Important**  
If you enable webhooks for a CodeBuild project, and the project is used as a build step in CodePipeline, then two identical builds are created for each commit\. One build is triggered through webhooks and one through CodePipeline\. Because billing is on a per\-build basis, you are billed for both builds\. Therefore, if you are using CodePipeline, we recommend that you disable webhooks in CodeBuild\. In the CodeBuild console, clear the **Webhook** box\. For more information, see [Change a build project's settings \(console\)](change-project-console.md)

1. Open the AWS CodeBuild console at [https://console\.aws\.amazon\.com/codesuite/codebuild/home](https://console.aws.amazon.com/codesuite/codebuild/home)\.

1.  If a CodeBuild information page is displayed, choose **Create build project**\. Otherwise, on the navigation pane, expand **Build**, choose **Build projects**, and then choose **Create build project**\. 

1. For **Project name**, enter a name for this build project\. Build project names must be unique across each AWS account\.

1. \(Optional\) Enter a description\.

1. For **Environment**, do one of the following:
   + To use a build environment based on a Docker image that is managed by CodeBuild, choose **Managed image**\. Make your selections from the **Operating system**, **Runtime**, and **Runtime version** drop\-down lists\. For more information, see [Docker images provided by CodeBuild](build-env-ref-available.md)\.
   + To use a build environment based on a Docker image in an Amazon ECR repository in your AWS account, choose **Custom image**\. For **Environment type**, choose an environment type, and then choose **Amazon ECR**\. Use the **Amazon ECR repository** and **Amazon ECR image** drop\-down lists to choose the Amazon ECR repository and Docker image in that repository\.
   + To use a build environment based on a publicly available Docker image in Docker Hub, choose **Other location**\. In **Other location**, enter the Docker image ID, using the format `docker repository/docker-image-name`\. 

   Select **Privileged** only if you plan to use this build project to build Docker images, and the build environment image you chose is not one provided by CodeBuild with Docker support\. Otherwise, all associated builds that attempt to interact with the Docker daemon fail\. You must also start the Docker daemon so that your builds can interact with it as needed\. You can do this by running the following build commands to initialize the Docker daemon in the `install` phase of your buildspec\. \(Do not run the following build commands if you chose a build environment image provided by CodeBuild with Docker support\.\)

   ```
   - nohup /usr/local/bin/dockerd --host=unix:///var/run/docker.sock --host=tcp://127.0.0.1:2375 --storage-driver=overlay&
   - timeout -t 15 sh -c "until docker info; do echo .; sleep 1; done"
   ```

1. In **Service role**, do one of the following:
   + If you do not have a CodeBuild service role, choose **New service role**\. In **Role name**, enter a name for the new role\.
   + If you have a CodeBuild service role, choose **Existing service role**\. In **Role ARN**, choose the service role\.
**Note**  
When you use the console to create or update a build project, you can create a CodeBuild service role at the same time\. By default, the role works with that build project only\. If you use the console to associate this service role with another build project, the role is updated to work with the other build project\. A service role can work with up to 10 build projects\.

1. Expand **Additional configuration**\.

   To specify a build timeout other than 60 minutes \(the default\), use the **hours** and **minutes** boxes to set a timeout between 5 and 480 minutes \(8 hours\)\.

   For **Compute**, choose one of the available options\.

   For **Environment variables**, use **Name** and **Value** to specify any optional environment variables for the build environment to use\. To add more environment variables, choose **Add environment variable**\.
**Important**  
We strongly discourage storing sensitive values, especially AWS access key IDs and secret access keys, in environment variables\. Environment variables can be displayed in plain text in the CodeBuild console and AWS CLI\.  
To store and retrieve sensitive values, we recommend your build commands use the AWS CLI to interact with the Amazon EC2 Systems Manager Parameter Store\. The AWS CLI is already installed and configured on all build environments provided by CodeBuild\. For more information, see [Systems Manager Parameter Store](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-paramstore.html) and [Systems Manager Parameter Store CLI Walkthrough](https://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-paramstore-walk.html#sysman-paramstore-cli) in the *Amazon EC2 Systems Manager User Guide*

1. For **Buildspec**, do one of the following:
   + If your source code includes a buildspec file, choose **Use a buildspec file**\. 
   + If your source code does not include a buildspec file, choose **Insert build commands**\. For **Build commands**, enter the commands you want to run during the build phase in the build environment\. For multiple commands, separate each command with `&&` for Linux\-based build environments or `;` for Windows\-based build environments\. For **Output files**, enter the paths to the build output files in the build environment that you want to send to CodePipeline\. For multiple files, separate each file path with a comma\.

1. Choose **Create build project**\.

1. Return to the CodePipeline console\.

1. For **Input artifacts**, choose the output artifact that you noted earlier in this procedure\.

1. For **Output artifacts**, enter a name for the output artifact \(for example, **MyAppBuild**\)\. 

1. Choose **Add action**\.

1. Choose **Save**, and then choose **Save** to save your changes to the pipeline\.

1. Choose **Release change**\.

1. After the pipeline runs successfully, you can get the build output artifact\. With the pipeline displayed in the CodePipeline console, in the **Build** action, choose the tooltip\. Make a note of the value for **Output artifact** \(for example, **MyAppBuild**\)\.
**Note**  
You can also get the build output artifact by choosing the **Build artifacts** link on the build details page in the CodeBuild console\. To get to this page, see [View build details \(console\)](view-build-details.md#view-build-details-console), and then skip to step 31 of this procedure\.

1. Open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. In the list of buckets, open the bucket used by the pipeline\. The name of the bucket should follow the format `codepipeline-region-ID-random-number`\. You can use the AWS CLI to run the CodePipeline get\-pipeline command to get the name of the bucket:

   ```
   aws codepipeline get-pipeline --name my-pipeline-name
   ```

    In the output, the `pipeline` object contains an `artifactStore` object, which contains a `location` value with the name of the bucket\.

1. Open the folder that matches the name of your pipeline \(depending on the length of the pipeline's name, the folder name might be truncated\), and then open the folder matching the value for **Output artifact** that you noted earlier in this procedure\.

1. Extract the contents of the file\. If there are multiple files in that folder, extract the contents of the file with the latest **Last Modified** timestamp\. \(You might need to give the file the `.zip` extension so that you can work with it in your system's ZIP utility\.\) The build output artifact is in the extracted contents of the file\.

1. If you instructed CodePipeline to deploy the build output artifact, use the deployment provider's instructions to get to the build output artifact on the deployment targets\.

## Add a CodeBuild test action to a pipeline \(CodePipeline console\)<a name="how-to-create-pipeline-add-test"></a>

1. Sign in to the AWS Management Console by using:
   + Your AWS root account\. This is not recommended\. For more information, see [The account root user](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_root-user.html) in the *IAM User Guide*\.
   + An administrator IAM user in your AWS account\. For more information, see [Creating your first IAM admin user and group](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html) in the *IAM User Guide*\.
   + An IAM user in your AWS account with permission to perform the following minimum set of actions:

     ```
     codepipeline:*
     iam:ListRoles
     iam:PassRole
     s3:CreateBucket
     s3:GetBucketPolicy
     s3:GetObject
     s3:ListAllMyBuckets
     s3:ListBucket
     s3:PutBucketPolicy
     codecommit:ListBranches
     codecommit:ListRepositories
     codedeploy:GetApplication
     codedeploy:GetDeploymentGroup
     codedeploy:ListApplications
     codedeploy:ListDeploymentGroups
     elasticbeanstalk:DescribeApplications
     elasticbeanstalk:DescribeEnvironments
     lambda:GetFunctionConfiguration
     lambda:ListFunctions
     opsworks:DescribeStacks
     opsworks:DescribeApps
     opsworks:DescribeLayers
     ```

1. Open the CodePipeline console at [https://console\.aws\.amazon\.com/codesuite/codepipeline/home](https://console.aws.amazon.com/codesuite/codepipeline/home)\.

1. In the AWS region selector, choose the AWS Region where your pipeline is located\. This must be an AWS Region where CodeBuild is supported\. For more information, see [AWS CodeBuild](https://docs.aws.amazon.com/general/latest/gr/rande.html#codebuild_region) in the *Amazon Web Services General Reference*\.

1. On the **Pipelines** page, choose the name of the pipeline\.

1. On the pipeline details page, in the **Source** action, choose the tooltip\. Make a note of the value for **Output artifact** \(for example, **MyApp**\)\.
**Note**  
This procedure shows you how to add a test action inside of a test stage between the **Source** and **Beta** stages\. If you want to add the test action somewhere else, rest your mouse pointer on the action just before, and make a note of the value for **Output artifact**\.

1. Choose **Edit**\.

1. Immediately after the **Source** stage, choose **Add stage**\.
**Note**  
This procedure shows you how to add a test stage immediately after the **Source** stage to your pipeline\. To add a test action to an existing stage, choose **Edit stage** in the stage, and then skip to step 8 of this procedure\. To add the test stage somewhere else, choose **Add stage** in the desired place\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/create-stage.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

1. For **Stage name**, enter the name of the test stage \(for example, **Test**\)\. If you choose a different name, use it throughout this procedure\.

1. In the selected stage, choose **Add action**\.
**Note**  
This procedure shows you how to add the test action in a test stage\. To add the test action somewhere else, choose **Add action** in the desired place\. You might first need to choose **Edit** in the existing stage where you want to add the test action\.

1. In **Edit action**, for **Action name**, enter a name for the action \(for example, **Test**\)\. If you choose a different name, use it throughout this procedure\.

1. For **Action provider**, under **Test**, choose **CodeBuild**\.

1. If you already have a build project in CodeBuild, for **Project name**, choose the name of the build project, and then skip to step 22 of this procedure\.
**Important**  
If you enable webhooks for a CodeBuild project, and the project is used as a build step in CodePipeline, then two identical builds are created for each commit\. One build is triggered through webhooks and one through CodePipeline\. Because billing is on a per\-build basis, you are billed for both builds\. Therefore, if you are using CodePipeline, we recommend that you disable webhooks in CodeBuild\. In the CodeBuild console, clear the **Webhook**box\. For more information, see [Change a build project's settings \(console\)](change-project-console.md)

1. Open the AWS CodeBuild console at [https://console\.aws\.amazon\.com/codesuite/codebuild/home](https://console.aws.amazon.com/codesuite/codebuild/home)\.

1.  If a CodeBuild information page is displayed, choose **Create build project**\. Otherwise, on the navigation pane, expand **Build**, choose **Build projects**, and then choose **Create build project**\. 

1. For **Project name**, enter a name for this build project\. Build project names must be unique across each AWS account\.

1. \(Optional\) Enter a description\.

1. For **Environment**, do one of the following:
   + To use a build environment based on a Docker image that is managed by CodeBuild, choose **Managed image**\. Make your selections from the **Operating system**, **Runtime**, and **Runtime version** drop\-down lists\. For more information, see [Docker images provided by CodeBuild](build-env-ref-available.md)\.
   + To use a build environment based on a Docker image in an Amazon ECR repository in your AWS account, choose **Custom image**\. For **Environment type**, choose an environment type, and then choose **Amazon ECR**\. Use the **Amazon ECR repository** and **Amazon ECR image** drop\-down lists to choose the Amazon ECR repository and Docker image in that repository\.
   + To use a build environment based on a publicly available Docker image in Docker Hub, choose **Other location**\. In **Other location**, enter the Docker image ID, using the format `docker repository/docker-image-name`\. 

   Select **Privileged** only if you plan to use this build project to build Docker images, and the build environment image you chose is not one provided by CodeBuild with Docker support\. Otherwise, all associated builds that attempt to interact with the Docker daemon fail\. You must also start the Docker daemon so that your builds can interact with it as needed\. You can do this by running the following build commands to initialize the Docker daemon in the `install` phase of your buildspec\. \(Do not run the following build commands if you chose a build environment image provided by CodeBuild with Docker support\.\)

   ```
   - nohup /usr/local/bin/dockerd --host=unix:///var/run/docker.sock --host=tcp://127.0.0.1:2375 --storage-driver=overlay&
   - timeout -t 15 sh -c "until docker info; do echo .; sleep 1; done"
   ```

1. In **Service role**, do one of the following:
   + If you do not have a CodeBuild service role, choose **New service role**\. In **Role name**, enter a name for the new role\.
   + If you have a CodeBuild service role, choose **Existing service role**\. In **Role ARN**, choose the service role\.
**Note**  
When you use the console to create or update a build project, you can create a CodeBuild service role at the same time\. By default, the role works with that build project only\. If you use the console to associate this service role with another build project, the role is updated to work with the other build project\. A service role can work with up to 10 build projects\.

1. Expand **Additional configuration**\.

   To specify a build timeout other than 60 minutes \(the default\), use the **hours** and **minutes** boxes to set a timeout between 5 and 480 minutes \(8 hours\)\.

   For **Compute**, choose one of the available options\.

   For **Environment variables**, use **Name** and **Value** to specify any optional environment variables for the build environment to use\. To add more environment variables, choose **Add environment variable**\.
**Important**  
We strongly discourage storing sensitive values, especially AWS access key IDs and secret access keys, in environment variables\. Environment variables can be displayed in plain text in the CodeBuild console and AWS CLI\.  
To store and retrieve sensitive values, we recommend your build commands use the AWS CLI to interact with the Amazon EC2 Systems Manager Parameter Store\. The AWS CLI is already installed and configured on all build environments provided by CodeBuild\. For more information, see [Systems Manager Parameter Store](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-paramstore.html) and [Systems Manager Parameter Store CLI Walkthrough](https://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-paramstore-walk.html#sysman-paramstore-cli) in the *Amazon EC2 Systems Manager User Guide*

1. For **Buildspec**, do one of the following:
   + If your source code includes a buildspec file, choose **Use a buildspec file**\. 
   + If your source code does not include a buildspec file, choose **Insert build commands**\. For **Build commands**, enter the commands you want to run during the build phase in the build environment\. For multiple commands, separate each command with `&&` for Linux\-based build environments or `;` for Windows\-based build environments\. For **Output files**, enter the paths to the build output files in the build environment that you want to send to CodePipeline\. For multiple files, separate each file path with a comma\.

1. Choose **Create build project**\.

1. Return to the CodePipeline console\.

1. For **Input artifacts**, select the value for **Output artifact** that you noted earlier in this procedure\.

1. \(Optional\) If you want your test action to produce an output artifact, and you set up your buildspec accordingly, then for **Output artifact**, enter the value you want to assign to the output artifact\.

1. Choose **Save**\.

1. Choose **Release change**\.

1. After the pipeline runs successfully, you can get the test results\. In the **Test** stage of the pipeline, choose the **CodeBuild** hyperlink to open the related build project page in the CodeBuild console\.

1. On the build project page, in **Build history**, choose the **Build run** hyperlink\.

1. On the build run page, in **Build logs**, choose the **View entire log** hyperlink to open the build log in the Amazon CloudWatch console\.

1. Scroll through the build log to view the test results\.