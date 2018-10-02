# Use AWS CodePipeline with AWS CodeBuild to Test Code and Run Builds<a name="how-to-create-pipeline"></a>

You can automate your release process by using AWS CodePipeline to test your code and run your builds with AWS CodeBuild\.

The following table lists tasks and the methods available for performing them\. Using the AWS SDKs to accomplish these tasks is outside the scope of this topic\. 


****  

| Task | Available approaches | Approaches described in this topic | 
| --- | --- | --- | 
| Create a continuous delivery \(CD\) pipeline with AWS CodePipeline that automates builds with AWS CodeBuild |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codebuild/latest/userguide/how-to-create-pipeline.html)  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codebuild/latest/userguide/how-to-create-pipeline.html)  | 
| Add test and build automation with AWS CodeBuild to an existing pipeline in AWS CodePipeline |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codebuild/latest/userguide/how-to-create-pipeline.html)  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codebuild/latest/userguide/how-to-create-pipeline.html)  | 

**Topics**
+ [Prerequisites](#how-to-create-pipeline-prerequisites)
+ [Create a Pipeline that Uses AWS CodeBuild \(AWS CodePipeline Console\)](#how-to-create-pipeline-console)
+ [Create a Pipeline that Uses AWS CodeBuild \(AWS CLI\)](#how-to-create-pipeline-cli)
+ [Add an AWS CodeBuild Build Action to a Pipeline \(AWS CodePipeline Console\)](#how-to-create-pipeline-add)
+ [Add an AWS CodeBuild Test Action to a Pipeline \(AWS CodePipeline Console\)](#how-to-create-pipeline-add-test)

## Prerequisites<a name="how-to-create-pipeline-prerequisites"></a>

1. Answer the questions in [Plan a Build](planning.md)\.

1. If you are using an IAM user to access AWS CodePipeline instead of an AWS root account or an administrator IAM user, attach the managed policy named `AWSCodePipelineFullAccess` to the user \(or to the IAM group to which the user belongs\)\. \(Using an AWS root account is not recommended\.\) This enables the user to create the pipeline in AWS CodePipeline\. For more information, see [Attaching Managed Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-using.html#attach-managed-policy-console) in the *IAM User Guide*\.
**Note**  
The IAM entity that attaches the policy to the user \(or to the IAM group to which the user belongs\) must have permission in IAM to attach policies\. For more information, see [Delegating Permissions to Administer IAM Users, Groups, and Credentials](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_delegate-permissions.html) in the *IAM User Guide*\.

1. Create an AWS CodePipeline service role, if you do not already have one available in your AWS account\. This service role enables AWS CodePipeline to interact with other AWS services, including AWS CodeBuild, on your behalf\. For example, to create an AWS CodePipeline service role by using the AWS CLI, run the IAM `create-role` command:

   For Linux, macOS, or Unix:

   ```
   aws iam create-role --role-name AWS-CodePipeline-CodeBuild-Service-Role --assume-role-policy-document '{"Version":"2012-10-17","Statement":{"Effect":"Allow","Principal":{"Service":"codepipeline.amazonaws.com"},"Action":"sts:AssumeRole"}}'
   ```

   For Windows:

   ```
   aws iam create-role --role-name AWS-CodePipeline-CodeBuild-Service-Role --assume-role-policy-document "{\"Version\":\"2012-10-17\",\"Statement\":{\"Effect\":\"Allow\",\"Principal\":{\"Service\":\"codepipeline.amazonaws.com\"},\"Action\":\"sts:AssumeRole\"}}"
   ```
**Note**  
The IAM entity that creates this AWS CodePipeline service role must have permission in IAM to create service roles\.

1. After you create an AWS CodePipeline service role or identify an existing one, you must add a policy statement to it\. Add the default AWS CodePipeline service role policy to the service role as described in [Review the Default AWS CodePipeline Service Role Policy](https://docs.aws.amazon.com/codepipeline/latest/userguide/iam-identity-based-access-control.html#how-to-custom-role) in the *AWS CodePipeline User Guide*\.
**Note**  
The IAM entity that adds this AWS CodePipeline service role policy must have permission in IAM to add service role policies to service roles\.

1. Create and upload the source code to a repository type supported by AWS CodeBuild and AWS CodePipeline, such as AWS CodeCommit, Amazon S3, or GitHub\. \(AWS CodePipeline does not currently support Bitbucket\.\) Make sure the source code contains a build spec file \(or you can declare a build spec when you define a build project later in this topic\), which provides instructions for building the source code\. For more information, see the [Build Spec Reference](build-spec-ref.md)\.
**Important**  
If you plan to use the pipeline to deploy built source code, then the build output artifact must be compatible with the deployment system you will use\.   
For AWS CodeDeploy, see the [AWS CodeDeploy Sample](sample-codedeploy.md) in this guide and see [Prepare a Revision for AWS CodeDeploy](https://docs.aws.amazon.com/codedeploy/latest/userguide/how-to-prepare-revision.html) in the *AWS CodeDeploy User Guide*\.
For AWS Elastic Beanstalk, see the [Elastic Beanstalk Sample](sample-elastic-beanstalk.md) in this guide and see [Create an Application Source Bundle](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/using-features.deployment.source.html) in the *AWS Elastic Beanstalk Developer Guide*\.
For AWS OpsWorks, see [Application Source](https://docs.aws.amazon.com/opsworks/latest/userguide/workingapps-creating.html#workingapps-creating-source) and [Using AWS CodePipeline with AWS OpsWorks](https://docs.aws.amazon.com/opsworks/latest/userguide/other-services-cp.html) in the *AWS OpsWorks User Guide*\.

## Create a Pipeline that Uses AWS CodeBuild \(AWS CodePipeline Console\)<a name="how-to-create-pipeline-console"></a>

Use the following procedure to create a pipeline that uses AWS CodeBuild to build and deploy your source code\.

To create a pipeline that only tests your source code, your options are to:
+ Use the following procedure to create the pipeline, and then delete the Build and Beta stages from the pipeline\. Then use the [Add an AWS CodeBuild Test Action to a Pipeline \(AWS CodePipeline Console\)](#how-to-create-pipeline-add-test) procedure in this topic to add to the pipeline a test action that uses AWS CodeBuild\.
+ Use one of the other procedures in this topic to create the pipeline, and then use the [Add an AWS CodeBuild Test Action to a Pipeline \(AWS CodePipeline Console\)](#how-to-create-pipeline-add-test) procedure in this topic to add to the pipeline a test action that uses AWS CodeBuild\.

**To use the create pipeline wizard in AWS CodePipeline to create a pipeline that uses AWS CodeBuild**

1. Complete the steps in [Prerequisites](#how-to-create-pipeline-prerequisites)\.

1. Open the AWS CodePipeline console, at [https://console\.aws\.amazon\.com/codepipeline](https://console.aws.amazon.com/codepipeline)\.

   You need to have already signed in to the AWS Management Console by using one of the following:
   + Your AWS root account\. This is not recommended\. For more information, see [The Account Root User](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_root-user.html) in the *IAM User Guide*\.
   + An administrator IAM user in your AWS account\. For more information, see [Creating Your First IAM Admin User and Group](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html) in the *IAM User Guide*\.
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

1. In the AWS region selector, choose the region where your pipeline and related AWS resources are located\. This region must also support AWS CodeBuild\. For more information, see [AWS CodeBuild](https://docs.aws.amazon.com/general/latest/gr/rande.html#codebuild_region) in the "Regions and Endpoints" topic in the *Amazon Web Services General Reference*\.

1. Create a pipeline as follows:

   If a welcome page is displayed, choose **Get started**\.

   If an **All Pipelines** page is displayed, choose **Create pipeline**\.

1. On the **Step 1: Name** page, for **Pipeline name**, type a name for the pipeline; for example, **CodeBuildDemoPipeline**\. If you choose a different name, substitute it throughout this procedure\. Choose **Next step**\.

1. On the **Step 2: Source** page, for **Source provider**, do one of the following:
   + If your source code is stored in an Amazon S3 bucket, choose **Amazon S3**\. For **Amazon S3 location**, type the path to the source code, using the format `s3://bucket-name/path/to/file-name.zip`\. Choose **Next step**\.
   + If your source code is stored in an AWS CodeCommit repository, choose **AWS CodeCommit**\. For **Repository name**, choose the name of the repository that contains the source code\. For **Branch name**, choose the name of the branch that represents the version of the source code you want to build\. Choose **Next step**\.
   + If your source code is stored in a GitHub repository, choose **GitHub**\. Choose **Connect to GitHub**, and follow the instructions to authenticate with GitHub\. For **Repository**, choose the name of the repository that contains the source code\. For **Branch**, choose the name of the branch that represents the version of the source code you want to build\. Choose **Next step**\.

1. On the **Step 3: Build** page, for **Build provider**, choose **AWS CodeBuild**\.

1. If you already have a build project you want to use, choose **Select an existing build project**\. For **Project name**, choose the name of the build project, and then skip ahead to step 17 in this procedure\.
**Note**  
If you choose an existing build project, it must have build output artifact settings already defined \(even though AWS CodePipeline will override them\)\. For more information, see the description of **Artifacts: Where to put the artifacts from this build project** in [Create a Build Project \(Console\)](create-project.md#create-project-console) or [Change a Build Project's Settings \(Console\)](change-project.md#change-project-console)\.
**Important**  
If you enable webhooks for an AWS CodeBuild project, and the project is used as a build step in AWS CodePipeline, then two identical builds will be created for each commit\. One build is triggered through webhooks, and one through AWS CodePipeline\. Because billing is on a per\-build basis, you will be billed for both builds\. Therefore, if you are using AWS CodePipeline, we recommend that you disable webhooks in CodeBuild\. In the AWS CodeBuild console, clear the Webhook box\. For more information, see step 9 in [Change a Build Project's Settings \(Console\)](change-project.md#change-project-console)

1. Choose **Create a new build project**\.

1. For **Project name**, type a name for this build project\. Build project names must be unique across each AWS account\.

1. \(Optional\) Type a description in the **Description** box\.

1. For **Environment image**, do one of the following:
   + To use a build environment based on a Docker image that is managed by AWS CodeBuild, choose **Use an image managed by AWS CodeBuild**\. Make your selections from the **Operating system**, **Runtime**, and **Version** drop\-down lists\. For more information, see [Docker Images Provided by AWS CodeBuild](build-env-ref-available.md)\.
   + To use a build environment based on a Docker image in an Amazon ECR repository in your AWS account, choose **Specify a Docker image**\. For **Custom image type**, choose **Amazon ECR**\. Use the **Amazon ECR repository** and **Amazon ECR image** drop\-down lists to specify the desired Amazon ECR repository and Docker image in that repository\.
   + To use a build environment based on a publicly available Docker image in Docker Hub, choose **Specify a Docker image**\. For **Custom image type**, choose **Other**\. In the **Custom image ID** box, type the Docker image ID, using the format `docker-repo-name/docker-image-name:tag`\. 

1. For **Build specification**, do one of the following:
   + If your source code includes a build spec file, choose **Use the buildspec\.yml in the source code root directory**\. 
   + If your source code does not include a build spec file, choose **Insert build commands**\. For **Build command**, type the commands you want to run during the build phase in the build environment; for multiple commands, separate each command with `&&` for Linux\-based build environments or `;` for Windows\-based build environments\. For **Output files**, type the paths to the build output files in the build environment that you want to send to AWS CodePipeline; for multiple files, separate each file path with a comma\. For more information, see the tooltips in the console\.

1. For **AWS CodeBuild service role**, do one of the following:
   + If you do not have an AWS CodeBuild service role in your AWS account, choose **Create a service role in your account**\. In the **Role name** box, type a name for the service role or leave the suggested name\. \(Service role names must be unique across your AWS account\.\) 
**Note**  
If you use the console to create an AWS CodeBuild service role, by default this service role works with this build project only\. If you use the console to associate this service role with another build project, this role will be updated to work with the other build project\. A single AWS CodeBuild service role can work with up to ten build projects\.
   + If you have an AWS CodeBuild service role in your AWS account, choose **Choose an existing service role from your account**\. In the **Role name** box, choose the name of the service role\.

1. Expand **Advanced**\.

   To specify a build timeout other than 60 minutes \(the default\), use the **hours** and **minutes** boxes to set a timeout between 5 and 480 minutes \(8 hours\) \.

   Select the **Privileged** check box only if you plan to use this build project to build Docker images, and the build environment image you chose is not one provided by AWS CodeBuild with Docker support\. Otherwise, all associated builds that attempt to interact with the Docker daemon will fail\. Note that you must also start the Docker daemon so that your builds can interact with it as needed\. One way to do this is to initialize the Docker daemon in the `install` phase of your build spec by running the following build commands\. \(Do not run the following build commands if you chose a build environment image provided by AWS CodeBuild with Docker support\.\)

   ```
   - nohup /usr/local/bin/dockerd --host=unix:///var/run/docker.sock --host=tcp://127.0.0.1:2375 --storage-driver=overlay&
   - timeout -t 15 sh -c "until docker info; do echo .; sleep 1; done"
   ```

   For **Compute**, choose one of the available options\.

   For **Environment variables**, use **Name** and **Value** to specify any optional environment variables for the build environment to use\. To add more environment variables, choose **Add row**\.
**Important**  
We strongly discourage using environment variables to store sensitive values, especially AWS access key IDs and secret access keys\. Environment variables can be displayed in plain text using tools such as the AWS CodeBuild console and the AWS CLI\.  
To store and retrieve sensitive values, we recommend your build commands use the AWS CLI to interact with the Amazon EC2 Systems Manager Parameter Store\. The AWS CLI comes preinstalled and preconfigured on all build environments provided by AWS CodeBuild\. For more information, see [Systems Manager Parameter Store](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-paramstore.html) and [Systems Manager Parameter Store CLI Walkthrough](https://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-paramstore-walk.html#sysman-paramstore-cli) in the *Amazon EC2 Systems Manager User Guide*

1. Choose **Save build project**\.

1. After the build project is saved, choose **Next step**\.

1. On the **Step 4: Deploy** page, do one of the following:
   + If you do not want to deploy the build output artifact, for **Deployment provider**, choose **No Deployment**\. 
   + If you want to deploy the build output artifact, for **Deployment provider**, choose a deployment provider, and then specify the settings when prompted\.

1. Choose **Next step**\.

1. On the **Step 5: Service Role** page, for **Role name**, choose the AWS CodePipeline service role you created or identified as part of this topic's prerequisites\.

   Do not use this page to create a AWS CodePipeline service role\. If you do, the service role will not have the permissions required to work with AWS CodeBuild\. 

1. Choose **Next step**\.

1. On the **Step 6: Review** page, choose **Create pipeline**\.

1. After the pipeline runs successfully, you can get the build output artifact\. With the pipeline displayed in the AWS CodePipeline console, in the **Build** action, rest your mouse pointer on the tooltip\. Make a note of the value for **Output artifact** \(for example, **MyAppBuild**\)\.
**Note**  
You can also get the build output artifact by choosing the **Build artifacts** link on the build details page in the AWS CodeBuild console\. To get to this page, skip the rest of the steps in this procedure, and see [View Build Details \(Console\)](view-build-details.md#view-build-details-console)\.

1. Open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. In the list of buckets, open the bucket used by the pipeline\. The name of the bucket should follow the format `codepipeline-region-ID-random-number`\. You can use the AWS CLI to run the AWS CodePipeline `get-pipeline` command to get the name of the bucket, where *my\-pipeline\-name* is the display name of your pipeline:

   ```
   aws codepipeline get-pipeline --name my-pipeline-name
   ```

    In the output, the `pipeline` object contains an `artifactStore` object, which contains a `location` value with the name of the bucket\.

1. Open the folder that matches the name of your pipeline \(depending on the length of the pipeline's name, the folder name might be truncated\), and then open the folder matching the value for **Output artifact** that you noted in step 23 of this procedure\.

1. Extract the contents of the file\. If there are multiple files in that folder, extract the contents of the file with the latest **Last Modified** timestamp\. \(You might need to give the file the `.zip` extension so that you can work with it in your system's ZIP utility\.\) The build output artifact will be in the extracted contents of the file\.

1. If you instructed AWS CodePipeline to deploy the build output artifact, use the deployment provider's instructions to get to the build output artifact on the deployment targets\.

## Create a Pipeline that Uses AWS CodeBuild \(AWS CLI\)<a name="how-to-create-pipeline-cli"></a>

Use the following procedure to create a pipeline that uses AWS CodeBuild to build your source code\.

To use the AWS CLI to create a pipeline that deploys your built source code or that only tests your source code, you can adapt the instructions in [Edit a Pipeline \(AWS CLI\)](https://docs.aws.amazon.com/codepipeline/latest/userguide/how-to-edit-pipelines.html#how-to-edit-pipelines-cli) and the [AWS CodePipeline Pipeline Structure Reference](https://docs.aws.amazon.com/codepipeline/latest/userguide/pipeline-structure.html) in the *AWS CodePipeline User Guide*\.

1. Complete the steps in [Prerequisites](#how-to-create-pipeline-prerequisites)\.

1. Create or identify a build project in AWS CodeBuild\. For more information, see [Create a Build Project](create-project.md)\.
**Important**  
The build project must define build output artifact settings \(even though AWS CodePipeline will override them\)\. For more information, see the description of `artifacts` in [Create a Build Project \(AWS CLI\)](create-project.md#create-project-cli)\.

1. Make sure you have configured the AWS CLI with the AWS access key and AWS secret access key that correspond to one of the IAM entities described in this topic\. For more information, see [Getting Set Up with the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-set-up.html) in the *AWS Command Line Interface User Guide*\.

1. Create a JSON\-formatted file that represents the structure of the pipeline\. Name the file `create-pipeline.json` or similar\. For example, this JSON\-formatted structure creates a pipeline with a source action that references an Amazon S3 input bucket and a build action that uses AWS CodeBuild:

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
               "provider": "AWS CodeBuild"
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
   + The value of `roleArn` must match the ARN of the AWS CodePipeline service role you created or identified as part of the prerequisites\.
   + The values of `S3Bucket` and `S3ObjectKey` in `configuration` assume the source code is stored in an Amazon S3 bucket\. For settings for other source code repository types, see the [AWS CodePipeline Pipeline Structure Reference](https://docs.aws.amazon.com/codepipeline/latest/userguide/pipeline-structure.html) in the *AWS CodePipeline User Guide*\.
   + The value of `ProjectName` is the name of the AWS CodeBuild build project you created earlier in this procedure\.
   + The value of `location` is the name of the Amazon S3 bucket used by this pipeline\. For more information, see [Create a Policy for an Amazon S3 Bucket to Use as the Artifact Store for AWS CodePipeline](https://docs.aws.amazon.com/codepipeline/latest/userguide/access-permissions.html#how-to-create-bucket-policy) in the *AWS CodePipeline User Guide*\.
   + The value of `name` is the name of this pipeline\. All pipeline names must be unique to your account\.

   Although this data describes only a source action and a build action, you can add actions for activities related to testing, deploying the build output artifact, invoking AWS Lambda functions, and more\. For more information, see the [AWS CodePipeline Pipeline Structure Reference](https://docs.aws.amazon.com/codepipeline/latest/userguide/pipeline-structure.html) in the *AWS CodePipeline User Guide*\.

1. Switch to the folder that contains the JSON file, and then run the AWS CodePipeline `[create\-pipeline](https://docs.aws.amazon.com/cli/latest/reference/codepipeline/create-pipeline.html)` command, specifying the file name:

   ```
   aws codepipeline create-pipeline --cli-input-json file://create-pipeline.json
   ```
**Note**  
You must create the pipeline in an AWS region that supports AWS CodeBuild\. For more information, see [AWS CodeBuild](https://docs.aws.amazon.com/general/latest/gr/rande.html#codebuild_region) in the "Regions and Endpoints" topic in the *Amazon Web Services General Reference*\.

   The JSON\-formatted data appears in the output, and AWS CodePipeline creates the pipeline\.

1. To get information about the pipeline's status, run the AWS CodePipeline `[get\-pipeline\-state](https://docs.aws.amazon.com/cli/latest/reference/codepipeline/get-pipeline-state.html)` command, specifying the name of the pipeline:

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
             "actionName": "AWS CodeBuild",
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
You can also get the build output artifact by choosing the **Build artifacts** link on the related build details page in the AWS CodeBuild console\. To get to this page, skip the rest of the steps in this procedure, and see [View Build Details \(Console\)](view-build-details.md#view-build-details-console)\.

1. In the list of buckets, open the bucket used by the pipeline\. The name of the bucket should follow the format `codepipeline-region-ID-random-number`\. You can get the bucket name from the `create-pipeline.json` file or you can run the AWS CodePipeline `get-pipeline` command to get the bucket's name\.

   ```
   aws codepipeline get-pipeline --name my-pipeline-name
   ```

    In the output, the `pipeline` object contains an `artifactStore` object, which contains a `location` value with the name of the bucket\.

1. Open the folder that matches the name of your pipeline \(for example, `my-pipeline-name`\)\.

1. In that folder, open the folder named `default`\.

1. Extract the contents of the file\. If there are multiple files in that folder, extract the contents of the file with the latest **Last Modified** timestamp\. \(You might need to give the file a `.zip` extension so that you can work with it in your system's ZIP utility\.\) The build output artifact will be in the extracted contents of the file\.

## Add an AWS CodeBuild Build Action to a Pipeline \(AWS CodePipeline Console\)<a name="how-to-create-pipeline-add"></a>

1. Open the AWS CodePipeline console at [https://console\.aws\.amazon\.com/codepipeline](https://console.aws.amazon.com/codepipeline)\.

   You should have already signed in to the AWS Management Console by using one of the following:
   + Your AWS root account\. This is not recommended\. For more information, see [The Account Root User](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_root-user.html) in the *IAM User Guide*\.
   + An administrator IAM user in your AWS account\. For more information, see [Creating Your First IAM Admin User and Group](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html) in the *IAM User Guide*\.
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

1. In the AWS region selector, choose the region where your pipeline is located\. This region must also support AWS CodeBuild\. For more information, see [AWS CodeBuild](https://docs.aws.amazon.com/general/latest/gr/rande.html#codebuild_region) in the "Regions and Endpoints" topic in the *Amazon Web Services General Reference*\.

1. On the **All Pipelines** page, choose the name of the pipeline\.

1. On the pipeline details page, in the **Source** action, rest your mouse pointer on the tooltip\. Make a note of the value for **Output artifact** \(for example, **MyApp**\):  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/output-artifact.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)
**Note**  
This procedure assumes you want to add a build action inside of a build stage between the **Source** and **Beta** stages\. If you want to add the build action somewhere else, rest your mouse pointer on the action just before the place where you want to add the build action, and make a note of the value for **Output artifact**\.

1. Choose **Edit**\.

1. Between the **Source** and **Beta** stages, choose the add symbol \(**\+**\) next to **Stage**\.
**Note**  
This procedure assumes you want to add a new build stage to your pipeline\. To add a build action to an existing stage, choose the edit \(pencil\) icon in the existing stage, and then skip to step 8 of this procedure\.  
This procedure also assumes you want to add a build stage between the **Source** and **Beta** stages\. To add the build stage somewhere else, choose the add symbol in the desired place\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/create-stage.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

1. For **Enter stage name**, type the name of the build stage \(for example, **Build**\)\. If you choose a different name, use it throughout this procedure\.

1. Inside of the selected stage, choose the add symbol \(**\+**\) next to **Action**\.
**Note**  
This procedure assumes you want to add the build action inside of a build stage\. To add the build action somewhere else, choose the add symbol in the desired place\. You might first need to choose the edit \(pencil\) icon in the existing stage where you want to add the build action\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/create-action.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

1. In the **Add action** pane, for **Action category**, choose **Build**\.

1. In **Build actions**, for **Action name**, type a name for the action \(for example, **AWS CodeBuild**\)\. If you choose a different name, use it throughout this procedure\.

1. For **Build provider**, choose **AWS CodeBuild**\.

1. If you already have a build project in AWS CodeBuild, choose **Select an existing build project**\. For **Project name**, choose the name of the build project, and then skip to step 21 of this procedure\.
**Note**  
If you choose an existing build project, it must have build output artifact settings already defined \(even though AWS CodePipeline will override them\)\. For more information, see the description of **Artifacts: Where to put the artifacts from this build project** in [Create a Build Project \(Console\)](create-project.md#create-project-console) or [Change a Build Project's Settings \(Console\)](change-project.md#change-project-console)\.
**Important**  
If you enable webhooks for an AWS CodeBuild project, and the project is used as a build step in AWS CodePipeline, then two identical builds will be created for each commit\. One build is triggered through webhooks; and one through AWS CodePipeline\. Because billing is on a per\-build basis, you will be billed for both builds\. Therefore, if you are using AWS CodePipeline, we recommend that you disable webhooks in CodeBuild\. In the AWS CodeBuild console, uncheck the webhook box\. For more information, see step 9 in [Change a Build Project's Settings \(Console\)](change-project.md#change-project-console)

1. Choose **Create a new build project**\.

1. For **Project name**, type a name for this build project\. Build project names must be unique across each AWS account\.

1. \(Optional\) Type a description in the **Description** box\.

1. For **Environment image**, do one of the following:
   + To use a build environment based on a Docker image that is managed by AWS CodeBuild, choose **Use an image managed by AWS CodeBuild**\. Make your selections from the **Operating system**, **Runtime**, and **Version** drop\-down lists\. For more information, see [Docker Images Provided by AWS CodeBuild](build-env-ref-available.md)\.
   + To use a build environment based on a Docker image in an Amazon ECR repository in your AWS account, choose **Specify a Docker image**\. For **Custom image type**, choose **Amazon ECR**\. Use the **Amazon ECR repository** and **Amazon ECR image** drop\-down lists to specify the desired Amazon ECR repository and Docker image in that repository\.
   + To use a build environment based on a Docker image in Docker Hub, choose **Specify a Docker image**\. For **Custom image type**, choose **Other**\. In the **Custom image ID** box, type the Docker image ID, using the format `docker-repo-name/docker-image-name:tag`\. 

1. For **Build specification**, do one of the following:
   + If your source code includes a build spec file, choose **Use the buildspec\.yml in the source code root directory**\. 
   + If your source code does not include a build spec file, choose **Insert build commands**\. For **Build command**, type the commands you want to run during the build phase in the build environment; for multiple commands, separate each command with `&&` for Linux\-based build environments or `;` for Windows\-based build environments\. For **Output files**, type the paths to the build output files in the build environment that you want to send to AWS CodePipeline; for multiple files, separate each file path with a comma\. For more information, see the tooltips in the console\.

1. For **AWS CodeBuild service role**, do one of the following:
   + If you do not have an AWS CodeBuild service role in your AWS account, choose **Create a service role in your account**\. In the **Role name** box, type a name for the service role or leave the suggested name\. \(Service role names must be unique across your AWS account\.\) 
**Note**  
If you use the console to create an AWS CodeBuild service role, by default this service role works with this build project only\. If you use the console to associate this service role with another build project, this role will be updated to work with the other build project\. A single AWS CodeBuild service role can work with up to ten build projects\.
   + If you have an AWS CodeBuild service role in your AWS account, choose **Choose an existing service role from your account**\. In the **Role name** box, choose the name of the service role\.

1. Expand **Advanced**\.

   To specify a build timeout other than 60 minutes \(the default\), use the **hours** and **minutes** boxes to specify a timeout between 5 and 480 minutes \(8 hours\)\.

   For **Compute**, choose one of the available options\.

   Select the **Privileged** check box only if you plan to use this build project to build Docker images, and the build environment image you chose is not one provided by AWS CodeBuild with Docker support\. Otherwise, all associated builds that attempt to interact with the Docker daemon will fail\. Note that you must also start the Docker daemon so that your builds can interact with it as needed\. One way to do this is to initialize the Docker daemon in the `install` phase of your build spec by running the following build commands\. \(Do not run the following build commands if you chose a build environment image provided by AWS CodeBuild with Docker support\.\)

   ```
   - nohup /usr/local/bin/dockerd --host=unix:///var/run/docker.sock --host=tcp://127.0.0.1:2375 --storage-driver=overlay&
   - timeout -t 15 sh -c "until docker info; do echo .; sleep 1; done"
   ```

   For **Environment variables**, use **Name** and **Value** to specify any optional environment variables for the build environment to use\. To add more environment variables, choose **Add row**\. 
**Important**  
We strongly discourage using environment variables to store sensitive values, especially AWS access key IDs and secret access keys\. Environment variables can be displayed in plain text using tools such as the AWS CodeBuild console and the AWS CLI\.  
To store and retrieve sensitive values, we recommend your build commands use the AWS CLI to interact with the Amazon EC2 Systems Manager Parameter Store\. The AWS CLI comes preinstalled and preconfigured on all build environments provided by AWS CodeBuild\. For more information, see [Systems Manager Parameter Store](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-paramstore.html) and [Systems Manager Parameter Store CLI Walkthrough](https://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-paramstore-walk.html#sysman-paramstore-cli) in the *Amazon EC2 Systems Manager User Guide*

1. Choose **Save build project**\.

1. For **Input artifact \#1**, type the value of **Output artifact** that you noted in step 4 of this procedure\.

1. For **Output artifact \#1**, type a name for the output artifact \(for example, **MyAppBuild**\)\. 

1. Choose **Add action**\.

1. Choose **Save pipeline changes**, and then choose **Save and continue**\.

1. Choose **Release change**\.

1. After the pipeline runs successfully, you can get the build output artifact\. With the pipeline displayed in the AWS CodePipeline console, in the **Build** action, rest your mouse pointer on the tooltip\. Make a note of the value for **Output artifact** \(for example, **MyAppBuild**\)\.
**Note**  
You can also get the build output artifact by choosing the **Build artifacts** link on the build details page in the AWS CodeBuild console\. To get to this page, see [View Build Details \(Console\)](view-build-details.md#view-build-details-console), and then skip to step 31 of this procedure\.

1. Open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. In the list of buckets, open the bucket used by the pipeline\. The name of the bucket should follow the format `codepipeline-region-ID-random-number`\. You can use the AWS CLI to run the AWS CodePipeline `get-pipeline` command to get the name of the bucket:

   ```
   aws codepipeline get-pipeline --name my-pipeline-name
   ```

    In the output, the `pipeline` object contains an `artifactStore` object, which contains a `location` value with the name of the bucket\.

1. Open the folder that matches the name of your pipeline \(depending on the length of the pipeline's name, the folder name might be truncated\), and then open the folder matching the value for **Output artifact** that you noted in step 26 of this procedure\.

1. Extract the contents of the file\. If there are multiple files in that folder, extract the contents of the file with the latest **Last Modified** timestamp\. \(You might need to give the file the `.zip` extension so that you can work with it in your system's ZIP utility\.\) The build output artifact will be in the extracted contents of the file\.

1. If you instructed AWS CodePipeline to deploy the build output artifact, use the deployment provider's instructions to get to the build output artifact on the deployment targets\.

## Add an AWS CodeBuild Test Action to a Pipeline \(AWS CodePipeline Console\)<a name="how-to-create-pipeline-add-test"></a>

1. Open the AWS CodePipeline console at [https://console\.aws\.amazon\.com/codepipeline](https://console.aws.amazon.com/codepipeline)\.

   You should have already signed in to the AWS Management Console by using one of the following:
   + Your AWS root account\. This is not recommended\. For more information, see [The Account Root User](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_root-user.html) in the *IAM User Guide*\.
   + An administrator IAM user in your AWS account\. For more information, see [Creating Your First IAM Admin User and Group](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html) in the *IAM User Guide*\.
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

1. In the AWS region selector, choose the region where your pipeline is located\. This region must also support AWS CodeBuild\. For more information, see [AWS CodeBuild](https://docs.aws.amazon.com/general/latest/gr/rande.html#codebuild_region) in the "Regions and Endpoints" topic in the *Amazon Web Services General Reference*\.

1. On the **All Pipelines** page, choose the name of the pipeline\.

1. On the pipeline details page, in the **Source** action, rest your mouse pointer on the tooltip\. Make a note of the value for **Output artifact** \(for example, **MyApp**\):  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/output-artifact.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)
**Note**  
This procedure assumes you want to add a test action inside of a test stage between the **Source** and **Beta** stages\. If you want to add the test action somewhere else, rest your mouse pointer on the action just before, and make a note of the value for **Output artifact**\.

1. Choose **Edit**\.

1. Immediately after the **Source** stage, choose add \(**\+**\) next to **Stage**\.
**Note**  
This procedure assumes you want to add a test stage to your pipeline\. To add a test action to an existing stage, choose the edit \(pencil\) icon in the existing stage, and then skip to step 8 of this procedure\.  
This procedure also assumes you want to add a test stage immediately after the **Source** stage\. To add the test stage somewhere else, choose the add symbol in the desired place\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/create-stage-test.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

1. For **Enter stage name**, type the name of the test stage \(for example, **Test**\)\. If you choose a different name, use it throughout this procedure\.

1. Inside of the selected stage, choose add \(**\+**\) next to **Action**\.
**Note**  
This procedure assumes you want to add the test action inside of a test stage\. To add the test action somewhere else, choose the add symbol in the desired place\. You might first need to choose the edit \(pencil\) icon in the existing stage where you want to add the test action\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/create-test-action.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

1. In the **Add action** pane, for **Action category**, choose **Test**\.

1. In **Test actions**, for **Action name**, type a name for the action \(for example, **Test**\)\. If you choose a different name, use it throughout this procedure\.

1. For **Test provider**, choose **AWS CodeBuild**\.

1. If you already have a build project in AWS CodeBuild, choose **Select an existing build project**\. For **Project name**, choose the name of the build project, and then skip to step 21 of this procedure\.
**Important**  
If you enable webhooks for an AWS CodeBuild project, and the project is used as a build step in AWS CodePipeline, then two identical builds will be created for each commit\. One build is triggered through webhooks; and one through AWS CodePipeline\. Because billing is on a per\-build basis, you will be billed for both builds\. Therefore, if you are using AWS CodePipeline, we recommend that you disable webhooks in CodeBuild\. In the AWS CodeBuild console, clear the webhook box\. For more information, see step 9 in [Change a Build Project's Settings \(Console\)](change-project.md#change-project-console)

1. Choose **Create a new build project**\.

1. For **Project name**, type a name for this build project\. Build project names must be unique across each AWS account\.

1. \(Optional\) Type a description in the **Description** box\.

1. For **Environment image**, do one of the following:
   + To use a build environment based on a Docker image that is managed by AWS CodeBuild, choose **Use an image managed by AWS CodeBuild**\. Make your selections from the **Operating system**, **Runtime**, and **Version** drop\-down lists\. For more information, see [Docker Images Provided by AWS CodeBuild](build-env-ref-available.md)\.
   + To use a build environment based on a Docker image in an Amazon ECR repository in your AWS account, choose **Specify a Docker image**\. For **Custom image type**, choose **Amazon ECR**\. Use the **Amazon ECR repository** and **Amazon ECR image** drop\-down lists to specify the desired Amazon ECR repository and Docker image in that repository\.
   + To use a build environment based on a Docker image in Docker Hub, choose **Specify a Docker image**\. For **Custom image type**, choose **Other**\. In the **Custom image ID** box, type the Docker image ID, using the format `docker-repo-name/docker-image-name:tag`\. 

1. For **Build specification**, do one of the following:
   + If your source code includes a build spec file, choose **Use the buildspec\.yml in the source code root directory**\. 
   + If your source code does not include a build spec file, choose **Insert build commands**\. For **Build command**, type the commands you want to run during the build phase in the build environment\. For multiple commands, separate each command with `&&` for Linux\-based build environments or `;` for Windows\-based build environments\. For **Output files**, type the paths to the build output files in the build environment that you want to send to AWS CodePipeline\. For multiple files, separate each file path with a comma\. For more information, see the tooltips in the console\.

1. For **AWS CodeBuild service role**, do one of the following:
   + If you do not have an AWS CodeBuild service role in your AWS account, choose **Create a service role in your account**\. In the **Role name** box, type a name for the service role or leave the suggested name\. \(Service role names must be unique across your AWS account\.\) 
**Note**  
If you use the console to create an AWS CodeBuild service role, by default, this service role works with this build project only\. If you use the console to associate this service role with another build project, this role will be updated to work with the other build project\. A single AWS CodeBuild service role can work with up to ten build projects\.
   + If you have an AWS CodeBuild service role in your AWS account, choose **Choose an existing service role from your account**\. In the **Role name** box, choose the name of the service role\.

1. \(Optional\) Expand **Advanced**\.

   To specify a build timeout other than 60 minutes \(the default\), use the **hours** and **minutes** boxes to specify a timeout between 5 and 480 minutes \(8 hours\)\.

   Select the **Privileged** check box only if you plan to use this build project to build Docker images, and the build environment image you chose is not one provided by AWS CodeBuild with Docker support\. Otherwise, all associated builds that attempt to interact with the Docker daemon will fail\. Note that you must also start the Docker daemon so that your builds can interact with it as needed\. One way to do this is to initialize the Docker daemon in the `install` phase of your build spec by running the following build commands\. \(Do not run the following build commands if you chose a build environment image provided by AWS CodeBuild with Docker support\.\)

   ```
   - nohup /usr/local/bin/dockerd --host=unix:///var/run/docker.sock --host=tcp://127.0.0.1:2375 --storage-driver=overlay&
   - timeout -t 15 sh -c "until docker info; do echo .; sleep 1; done"
   ```

   For **Compute**, choose one of the available options\.

   For **Environment variables**, use **Name** and **Value** to specify any optional environment variables for the build environment to use\. To add more environment variables, choose **Add row**\. 
**Important**  
We strongly discourage using environment variables to store sensitive values, especially AWS access key IDs and secret access keys\. Environment variables can be displayed in plain text using tools such as the AWS CodeBuild console and the AWS CLI\.  
To store and retrieve sensitive values, we recommend your build commands use the AWS CLI to interact with the Amazon EC2 Systems Manager Parameter Store\. The AWS CLI comes preinstalled and preconfigured on all build environments provided by AWS CodeBuild\. For more information, see [Systems Manager Parameter Store](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-paramstore.html) and [Systems Manager Parameter Store CLI Walkthrough](https://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-paramstore-walk.html#sysman-paramstore-cli) in the *Amazon EC2 Systems Manager User Guide*

1. Choose **Save build project**\.

1. For **Input artifacts \#1**, type the **Output artifact** value you noted in step 4 of this procedure\.

1. \(Optional\) If you want your test action to produce an output artifact, and you set up your build spec accordingly, then for **Output artifact \#1**, type the value you want to assign to the output artifact\.

1. Choose **Add action**\.

1. Choose **Save pipeline changes**, and then choose **Save and continue**\.

1. Choose **Release change**\.

1. After the pipeline runs successfully, you can get the test results\. In the pipeline's **Test** stage, choose the **AWS CodeBuild** hyperlink to open the related build project page in the AWS CodeBuild console\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/test-action-link.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

1. On the build project page, in the **Build history** area, choose the related **Build run** hyperlink\.

1. On the build run page, in the **Build logs** area, choose the **View entire log** hyperlink to open the related build log in the Amazon CloudWatch console\.

1. Scroll through the build log to view the test results\.